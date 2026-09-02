---
title: "Indiana Jones and the Lost Solution"
date: 2026-09-01 00:00:00 +0700
categories: [EN-But it's not my fault]
tags: [EN-But it's not my fault]
---

# Indiana Jones and the Lost Solution

*Date: Friday, 30th August, 2025<br>*
*Context: We need to test if we can deploy open source models from HuggingFace on GKE (Google Kubernetes Engine) cluster, rather than deploy on the standard Google Vertex. It's closed to holiday (Vietnam Independence day on 2 September), and we need a result to report soon. But deploying such solution right before holiday, while keeping update communication is easier said than done*

### Feature explanation
Our company had a few running models on Google Vertex AI. We wanted to consider whether these models can run well on GKE, which has better autoscaling support and better price (this was in 2025. Now in 2026 Vertex AI has remedied this issue). We didn't run immediately our models on GKE, but instead, we wanted to run open source models from HuggingFace first. We wanted to see what the most optimal infrastructure (GPU card) we needed on GKE. While deploying multiple models for benchmark (accuracy, load test, response timem, token cost, etc) on GKE, we needed a ML API Gateway. The mechanism is very simple: each HTTP request contains an exact model name from HuggingFace (eg Qwen/Qwen2.5-3B) and a prompt, and the ML API Gateway will forward that prompt to the cluster hosting that model, and return the response.

Another member handled the deployment on GKE. I was in charge of the ML API Gateway.

### Codebase
My colleague had written a Terraform code for the GKE infrastructure and Knative YAML scripts for the models running on the GKE, and had deployed images of models (Qwen/Qwen2.5-3B, Qwen/Qwen-7B) on GKE services, and tested successfully.
*sidenote: it's OK if our audience aren't familiar with how K8s works. Not all of us employ K8s frequently in our daily work. K8s, also known as GKE (Google Kubernetes Engine) on Google Cloud (this post may write K8s and GKE interchangeably) allows you to autoscale horizontally workload on demand. When you deploy on K8s service, you are given an URL. The URL is deterministically computed based on the service name you defined. You can use this URL to access this service workload, given that you are on the same network with K8s.*

Now it's my turn. I just need to "turn on" our experimental K8s cluster, deploy the models again, then write a common API Gateway for that. *Because our K8s cluster is experimental, we shut it down when we didn't need anymore. Preferably outside working hour.*

At our company, we used Knative to manage our K8s cluster. This was my first time I used Knative, and so my knowledge was quite limited.

### Action
I immediately started writing the API Gateway (Python). It's short and quick. Then I turned on our experimental K8s cluster, deployed the models, and tested my API Gateway. In my mind: user sent request to API Gateway with model name (Qwen/Qwen2.5-3B, Qwen/Qwen-7B). API Gateway constructed the service URLs directly from those names, forward user requests to those service name. 

At first, healthcheck requests failed. Service were not found. My colleage **hardcoded** in the Knative configuration service names: "**qwen3b**" and "**qwen7b**" (important, more on this later). No matter, I would just change the service name to model names Qwen/Qwen2.5-3B, Qwen/Qwen-7B, deploy again, then test. But actually, this time service could not be ready. Then I did some research (and asked AI too) and I realized: [K8s service name must follow RFC 1035](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#dns-label-names). That means the K8s service names (copy from the link above):
- contain at most 63 characters
- contain only lowercase alphanumeric characters or '-'
- start with an alphabetic character
- end with an alphanumeric character

These rules are logical, but they completely wreck our model names from HuggingFace! I had to somehow match the current HuggingFace model names from client HTTP requests with the current service names ("**qwen3b**", "**qwen7b**"). It seemed that my colleague, when deploying these models, recognized this too, and he decided to hardcode the service names. No problem for him. He could deploy and test and consider the task done. But from my side, it's not that simple: if I hardcoded the model's real name with K8s service valid name, (say, hardcode Qwen/Qwen2.5-3B to "qwen3b" service name like my colleague did), then, whenever a data engineer or AI engineer wanted to deploy a new model for test, *we* engineers had to change the API Gateway source code and deployed again, too.

We could register the model name before deployment, but that introduced another layer: what will the persistence database look like? We also wanted to shutdown the K8s cluster whenever we didn't need it (outside office hour). And it's the last day before holiday, on *Friday*. My teamlead wanted to see the result too. He said he didn't care as long as I got his ML API Gateway, written in Python.

### Solution - K8s service
I wrote a list of what I needed:
- For deployment: map the model name to a K8s service valid name.
- For client calling API Gateway with HTTP request: Map the model name from HTTP request to a K8s service valid name.
- Both steps above must have the same K8s service valid name.

My colleague deployed the models with just his terminal. It's quick and simple enough. Just pull the model from HuggingFace, bake that model in our Docker image build process, deploy that Docker image containing the model on the K8s cluster. I figured, if I didn't want the Python API Gateway to use hardcode model names, then the Docker image build process and the deployment process should not hardcode model names. The name should come from user input. That means a **bash deployment script**!

User input is model name (*what models from HuggingFace do you want to deploy:*, asks the terminal). Then the Knative YAML file must be a template. But I didn't know much about how Knative worked. And using `sed` to replace text in Knative YAML file seems a bit risky: I wasn't sure if one day, `sed` could change the YAML file unintentionally.

*sidenote: from my previous experience, I didn't know much about Knative. And I was sure I could not rely on asking Gemini to edit Knative for me. Many times, I asked Gemini (our company provided us with Gemini Pro) for help, and the results were unusable. I had to pull up the official Knative documents, and edit the YAML file by myself. Seems normal back in the days, but in the era of AI, that can be seen as being unproductive.*

But I know Helm! Helm is hands down, much much easier to use than Knative. I knew I should inform the guy who handled the deployment, and knew more that I should let my teamlead know about the current blocker. But my teamlead urgently inquired for the Python code and the test results in the chat. So, I decided to partially worked in the part of my teamlead. His hometown is a bit far away from the office in HCM city, and he's probably on his way to the airport. *I'm on my own*.

| What we were having currently | What I needed to change |
| We deployed using terminal | A deployment bash script |
| Knative for our K8s cluster | Use Helm for my little experiment |

So, I setup a bash script: input from user is model name, and output is the same string that had been sanitised: all invalid characters were replaced with "-", and all letters were converted to lowercase. *Hopefully there won't be any edge case in the future where 2 different model names have name collision after sanitising*. But this is good for now. 

Using that sanitised model name (qwen-qwen2-53b, qwen-qwen7b), I filed in the values in Helm templates. Then I deployed the models from the Helm. The deployment was successful, and the service name and service URLs were exactly as I expected. Now move on to the Python code.

### Solution - Python API Gateway
User input: model name, and prompt for model to answer.

**Important**: If the sanitise bash script can clean the model names, then the Python code must do exactly that. Here, there were 2 separate functions that must perform the same thing: same input, same output. If there were any deviation, then either the deployment failed or HTTP request failed.

The rest were easy. With the deployments in place, I just forward the prompt to the corrected service name on K8s, and returned the result to user.

### Commit
I had the Python code, and the successful test result. But there's a problem: 
- I was stepping out of my work scope. My scope is the Python API Gateway, not the bash script, and certainly not the deployment with Helm.
- Our company uses Knative instead of Helm.

So, I made another branch in Git. The new branch contained the Python code plus all the "unauthorised" changes: the bash scripts, the Helm template. And the main branch contained all the Python code changes and the test results (just manual test using Postman). My teamlead, earlier, was screamming why the hell was I writing Bash script while he wanted the Python Gateway. To him, the solution right now only contained the Python code and the Postman successful test results.

I figured, even though I was stepping beyond my work scope, at least I had the solution for the K8s service naming problem. Maybe 1 day I might need it. So, I buried that solution in one of my branch. Hence the name of this post, *Indiana Jones and the lost solution*. I added Indiana Jones, because I thought this might make a good Indiana Jones movie title (by the way, you should check out the real movie series *Indiana Jones* from the 80s. They are really fun, even if you are watching an 80s series in 2026).

In the following week after our Independence holiday, my teamlead inquired my about the very same name problem in K8s service. Why me, and not my colleague who handled the deployment (and hardcoded the service name), I didn't know, and I didn't ask. I presented him the solution I saved earlier.

It worked, but in hindsight, I should have told him first. It wasn't fun for him to be kept in the dark. I believe, in software development or in any field in general, roadblocks and unexpected challenges are unavoidable. What matters more is that we keep transparency in our report. Managers have to report themselves too, and it's best to keep them informed so they can make the best decisions.
