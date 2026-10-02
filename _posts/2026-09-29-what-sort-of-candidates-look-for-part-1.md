---
title: "So, what sorts of candidates are you looking for? (Part I)"
date: 2026-09-27 00:00:00 +0700
categories: [EN-tech-lead]
tags: [EN-tech-lead]
---

*Date: February, 2025<br>*
*Context: In August 2025, I was terminated for creating an incident (that event deserves its own story). I spent 3 months trying to look for a job - interviews in 2024 were unusually difficult. In November, I accepted a software position in CMC Global. 2 months later, in January, I was promoted to technical lead in another project - I passed a system design interview from client. Now, when memories of rejected interviews and unemployments were still fresh, I got to interview candidates. What would it be like?*

Warning: long text wall ahead.<br>
**TLDR**: Many (if not most) programming interviews in Viet Nam are like quizzes: questions could be solved with a quick Google search, and you can't think or reason your way to come up with answer like you think of a math problem. When I became a technical lead and interviewed candidates for the company I worked for, I realised: candidates could answer the quizzes quite well, but they lost the ability to **(1) write code** and **(2) solve problem**. Cand Quizz interviews are great, but we should have a different way to test candidate ability to think on their feet, too. Nowadays, as AI is becoming more integrated at workplace, the format of the interviews may have to adapt, to measure candidates' experience and the ability to think.

### What I personally experienced when I was a candidate
Quick summary: I started my career mainly as a Java backend engineer. But from 2022 to 2024 I didn't work in Java jobs anymore. When I had to find a new job in 2024 (after an incident I caused), I was in a weird spot: I could write code quite well, and I still handled algorithm questions quite well. But which programming language do I choose: do I use the ones I just learnt in 2022 - 2024 (Kotlin, Golang), or do I still use Java? I was still more comfortable in Java, so I chose Java...

And here is how many, if not most, interviews for programming jobs in Vietnam are conducted: sometimes they ask you to solve a problem. But more often than not, especially in Java jobs, they ask you questions that you can only know answer in advance. You can't "reason" or think your way out of the problem like you solve an algorithmic problem. However, these know-in-advance questions can be quickly solved with a single Google search (something akin to a quiz question).<br>
Here are a few examples:

|Algorithmic, problem solving questions|Quiz questions|
|Sort a list of phone numbers with area code| How does Garbage Collection in Java works?|
|Determine if a string is a [palindrome](https://en.wikipedia.org/wiki/Palindrome)|What is OOP?|
|What would you do to create a HTML parser?|What is OAuth authentication?|

Problem solving questions (algorithm questions) are at the heart of programming interviews. After all, programmers are expected to solve problems. Each problem can be solved in a number of ways. Each solution has trade-offs, depending on input size, your compute or memory power, requirements, or simply the technical choice on your hands. If a programmer can work well in a problem solving interview, you can rely quite well on that candidate to solve real technical problems in the day jobs. But algorithm questions aren't easy to be used in an interview: it requires employers to come up with the questions, with sample answer criterias for each level of candidates (e.g junior, middle, senior). Sometimes candidates face the questions before (maybe in the past, maybe tipped of by someone): candidates can ace through the algorithm question and flounder on the job.

Quiz questions, or know-in-advance questions, however, are usually reserved for senior candidates. As engineers work long and deep in their respective field, engineers can develop their expertise not commonly found in common documentation. As engineers work with, for example, Java in this case, they learn the intimacy of the tool they use: how `HashSet` and `HashMap` are implemented, how garbage collection works, what is `volatile` in Java, how synchronisation works, and so on. If you know these corners of the tools you work with, naturally, you should be expected to be a senior. And if you have always been working with the same tools day and night, it might not be too hard to learn the most secrets of the tools. But if you switch jobs, and/or switch tools, it's a different story. Not all companies use the same tools. Heck, not all projects use the same tools.

Most companies, while hiring candidates, would have a required stack of technologies: Java, Python, Docker, SQL, K8s, cloud... Naturally, companies would interview candidates based on the technologies companies are using. If companies are using Java and SQL, you can expect them to ask a lot of questions about Java and SQL. If you work with Java and SQL, you can eventually master the deepest, darkest corners of the tools you use everyday without not too much problem, and these kinds of interviews are probably the most suitable for you. However, you will pay for that advantage: the tools you master so much might be unsuitable for some real world problems out there. SQL is notoriously hard to scale out at large scale at higher IOPs - much, much worse compared to other NoSQL approaches. Java is stable, and it's hard to mess up thanks to its strong type and strong guardrails, but it's slow(er) to write (more verbose), not cloud native, and is harder to scale too - the JVM (Java Virtual Machine) was meant to solve the numerous CPU architectures and compilers in the 90s, not the dynamic autoscaling nature in the cloud computing age. Not to mention Java values aren't exactly immutable, and you may have heard about numerous bugs, along with performance issues when values are subjected to change.

Yes, you can use Java and SQL to solve *almost all* problems, and your POC will be as beautiful as when you use any other tools, but do you think you can scale easily, for *any* problem, with such limited number of tools?

But, if you are like me, if you have been through multiple projects, with different techstacks (due to different requirements), you will learn a number of different tools for different jobs. You experience a wider range of problems, and you know which problems is best solved with what tools. For any deeper knowledge you are missing, you can read documentation as you go. The most important thing is, you are good at problem solving, and although it might take you *some* more time to implementing a solution than, say, someone who works deeply with the chosen tool, at least you **identify the problem and choose the correct tool**. The tradeoff is, you aren't so deep in some of the tools, including your favourite tools. 

#### Sidenote: programming inflation (my perspective)
2023 was a down year for SWE. Demands for software products slowed down after the COVID-19 pandemics, higher interest rates forces more layoffs, along with over training for SWEs, means there was a surplus of engineers in 2023. This meant companies were more picky when choosing candidates for the role. And what happens when multiple candidates are qualified for the same position? Companies have to look for the best (or the most suitable) candidates. That means raising the level of the interview, even though much of the language they demand is not necessarily useful for the job.

I get it, when conducting interviews, companies want to know your "base" level and your "ceiling" level. It's like evaluating an engineer, to know his or her minimum and maximum limit. Many questions from the interviewer aren't necessarily required by the jobs, but companies have the option to choose the best they find. After all, there is an abundant of engineers, so why not take the best, using the most difficult question?

Frankly, in 2024, many companies I interviewed at asked me questions that gave me a feeling they were trying to write a framework by themselves: 
- Java reflection (allowing you to modify values on runtime, when you **can't** modify values at compiled time). 
- AOP (adding logic to existing logic without changing the original logic. Very good for debug and observation).
- Threading in Java Stream with exception.
- How does Garbage Collection work (objects become short lived and long lived, and garbage collecting introduces performance penalties).
- How SQL cache works (binary tree)... 

I can go on the list forever. Of course I try to learn hard, but frankly, my mental resources are limited, and I couldn't answer well all of them. In some cases I was quite bad.

Funny enough, these "quiz" questions could be quickly answered and understood with a quick Google search, provided that you have certain amount of base knowledge such as common algorithms and data structures, computer structures, operating system knowledge, database design... But you can't reason or think your way out of those question. If you don't know the answer, you don't know. You can't "*give me a minute to think about it*". Even funnier, those interviews that include those questions never ask you to write code. They expect you to be a strong and experienced member, if you know these knowledge. I agree with that, too: only someone who has worked long in the field can know the answer to these questions (or if you have a really, really good tutor who teaches you everything). But that's the only metric they use. They **never** ask you to write code, to design a system, or to solve a problem.

It felt quite unfair (personal opinion) when they expect you to <ins>memorise</ins> so many facts.

### When I began to look for candidates
Fast forward to January 2026. I was informally promoted to technical lead in CMC Global, the 2nd biggest outsourcing firm in Vietnam, after I passed an system design interview (design a system for human resources management system). But because I was there for only two months, I took on additional responsibilities without an official title and official salary increase. No problem, I have always wanted to be a technical lead, so these new responsibilities would be fun.

My new responsibilities included:
- Interview new candidates for CMC. Grade them: junior, middle, senior, technical lead. List each candidate strong and weak points.
- Train CMC candidates, so they could pass client interviews. CMC is an outsourcing firm, and the bill includes members with certain level. After client approves (interviews) a member, that member can be added to the project, and CMC can charge the bill for that member's presence.
- Lead any project I'm assigned to. Too bad there wasn't any project for me at that moment, so I never got to do this.

This article is quite long. I will focus on how I interviewed candidates. I still remember *dearly* how I spent months looking for jobs just months earlier, so, on the other side of the coin, getting to interview and grading candidates was quite exciting and nervous.

Here is how CMC interview work:
- TAs (Talent Acquisition) notify PMs (Project Manager) from each project about new CV available. 
- If the CV looks good (many times it looks better than it is), many project's technical leads will interview that candidate at the same time. The interview is 1 hour. Each technical lead, from each project, will take his time to ask candidates questions, depending either on project needs or how the CV states what the candidate is good at.
- If multiple PMs approve the same candidate (candidate passes), multiple PMs might fight for the candidate. How this process works, I don't know. 
- If that candidate fails, depending on the feedback to TA, candidate can be interviewed by different project technical leaders. From candidate's perspective, this looks like another interview round, but in reality, the interviewers consider the candidate not good enough for their projects, but might be good enough for other projects. If the feedback is too bad, the candidate might be rejected immediately.

Since multiple tech leads from other project were also in the interviews that I joined, at first I shadowed other tech leads, and then I came up with my questions. As I was not being assigned to any project in particular, I didn't ask candidate based on the project needs; I asked candidates questions I came up with myself to grade candidates. And since so many tech leads (who were very experienced) asked "quiz" questions I mentioned above (that I didn't agree with), I decided to change a little bit: I asked clients to share screen to write some code.

#### My coding questions
At first, I was using the same criteria I had when I was at college: writing code without IDE assistance. Basically writing code from memory. Of course, I didn't expect the code to be perfect. But I did expect candidates to be able express their main ideas, and their pseudo code to reasonably correct (Java wise). If the candidates were comfortable writing code without IDE support, I *believed* candidates could jump into middle of a project, read the existing code, and know what to do next. Of course the entire interview duration was only 1 hour, and I only had a window of 10 minutes, maybe 15 minutes max.

Boy I was wrong.

For freshers, who graduated from college (should be reasonably true) and had some experience on CV (could be exaggerated. That's why we have interviews), I thought I could use questions that tested their algorithm and data structures. The 1st question, the most basic question, was: 
>
In Java, create an integer array. Then write a sort algorithm by yourself. Don't use a sort algorithm from library.
>

The most basic kind of question, possible. But none could solve this question, without IDE support (on Notepad++): Many didn't know the syntax for arrays. The characters `[]`, which indicate arrays, was completely foreign to them. Candidates were so used to using wrapper `List` and `ArrayList` that they never used the bare array in the first place.
(For those of you who don't know Java, `List` and `ArrayList` allow you to work with array without worrying about array sizes. You can add, remove elements in `List` and `ArrayList`, and the array will be automatically resized for you.)

I changed the requirements:
>
Create a list of integer by yourself. Then write a sort algorithm by yourself. Don't use a sort algorithm from library. 
>

Still no result. Candidates could create a list, <br>
```
List<Integer> myList = new ArrayList<>();
```

but I kid you not, they didn't know how to write a sort algorithm. I told them they could write anything, even the most basic selection sort or bubble sort, and that there was no need to write the harder algorithms like quick sort or merge sort. But none could write. All candidates pivoted to using library code `List.sort()`, and could not think of a way to write a sort algorithm. They couldn't even think, to write pseudo code, of a sort algorithm, let alone the code itself, in Notepad++, without IDE support.

(Does it mean with IDE support, candidates could write code? No, more on that later.)

*Too many* candidates failed this simple test. Maybe when I was at school, I used to write code on a piece of paper, and I was using the same test on these candidates. Maybe candidates these days were focusing on something else, and therefore could not reliably write code without IDE support? I was careful to make sure candidates had the most recent Java experience. If candidates could answer other not-too-hard theorical questions of Java (what is SOLID, what is OOP, how HashSet and HashMap work under the hood), then how on Earth could they not write even a single line of code? Of course I understand anxiety of an interview can play a part. But too many simply refused to write a single line of code.

### My coding question with IDE
With too many simply couldn't even write any code in Notepad++, I decided to make it a little bit easier. Now candidates could write using IDE and Google search. The test was somewhat closed to real work experience: candidates can use the same tools they use at work. I only had 2 prompts:

Prompt 1 (same as above):
>
Create a list of integer in Java. Write a sort algorithm yourself.
>
TODO: write Java code answer here

Prompt 2: 
>
Create a list of string in Java. Then, using Java stream, get all members of the list that has length more than 3, map the string value to its length, and collect the result as a HashMap (the final map has the keys of the strings from the list, and each string maps to its length).
>
TODO: write Java code answer here

Easy enough, right? And I assured candidates that it's OK they didn't know everything - that's why I included Google search to simulate real life example. And since I only had about 10 - 15 minutes windows, before someone else took his turn to interview candidates, the question should be easy enough. My grading criteria was quite simple: if candidates could do this, then they qualified at least as a junior member. If candidates could do this well and fast without using Google, they qualified at least as a middle engineer.

Oh boy, was I wrong. These were the cases I encountered:
- For the first prompt (sort a list), no one could do that. Many simply using library function `List.sort()`. When I asked them to implement one by themselves, even simple, nobody could. When I was at college, algorithms were all about sorting. Sort algorithms after sort algorithm. I haven't looked at today's curriculum for candidates who attended college after 2020, and I wonder if sorting was dropped from the list.
- Some candidates, when opening their IDE (Intellij IDEA) accidentally showed me the projects they were working on. I asked them to create a brand new project, since I didn't want to look at whatever Intellectual Property of whatever organisations they were working for. They refused, and insisted that they could use the `main` function of their existing project.<br>
- This one actually made me a little bit upset. Many candidates asked me to use ChatGPT during the interview. I wouldn't be as sad if they were *secretly* using ChatGPT in the interview (cheating) - at least they would know it's not a nice thing to do. Here, they were openly asking me to use AI, on a problem that should take only a few lines of code. If they could not do a simple exercise like this, how could I expect them to work on a fairly large, complicated projects? And this was early 2025, by the way. ChatGPT was better than when it was first introduced in 2022-23, but no decent developer would rely on AI for a complicated task. AI Agent still didn't exist back then.
Maybe to them, writing code is not that important; you should delegate as much as possible to AI? Maybe to them, using Copilot integrated with IDE was a much better idea than meticulously write every line of code by yourself? Maybe I was outdated? Maybe, because I learnt programming on paper at high school, and then Notepad, and then Notepad++, so to me, typing code with a keyboard is expected?<br>
- If candidates could answer prompt 2 (use Java stream), and *especially* if they used AI to answer it, I added a twist: what happens if the original input list contained a duplicate? The thing is, when you are in a stream in Java, and when you map a value to another value to create a HashMap in the end, if there is any duplicate, Java throws an exception: it doesn't know whether to map to the previously encountered value or map to the new value. To overcome this, you simply get the error in the stack trace, throw that in Google search, and get the first Stackoverflow post you can find, paste the answer back into your code. I could promise you for every part of the question, when you throw that on Google, you can find the answer in the first Google search result, or at least the answer in the first Google page. Of course candidates who used AI and encountered this error, given the new input edge case I just introduced, didn't even bother to solve this error. To this day, I don't know why. If a quick Google search (and Google was allowed) could solve this, then surely throwing that error in ChatGPT could solve this too. And they were already using ChatGPT, so why not used ChatGPT again? I don't know.
- Some candidates typed really slowly, it actually upset me. It took them at least a second to type a character on screen (usually it took 2, 3 seconds). I had to check if the internet was broken. I didn't even dare to ask them to type faster because I feared I was being rude (thinking back, I should have done that). Were they looking at another screen nearby to cheat with the answer someone else was providing to them? I couldn't tell. I simply assumed they were really slow typist, and it still got under my skin. I expect candidates to output a reason amount of work, and I doubt they could do that when typing *that* slow. I remember Joel Spolsky, the founder of StackOverflow, wrote on his blog "[if you can’t whiz through the easy stuff at 100 m.p.h., you’re never gonna get the advanced stuff](https://www.joelonsoftware.com/2006/10/25/the-guerrilla-guide-to-interviewing-version-30/)".
- There was an instance, when my colleague, who is a very experienced technical lead, interviewed a candidate using my prompt #2 (Java stream) without my presence. He later told me: he only asked candidate if candidate knew about Java stream. The candidate immediately produced code exactly like the questions I usually used (get all members of the list that has length more than xxx, map the string value to its length, and collect the result as a HashMap). He suspected this question was leaked after being used (by me) so many times, so he came up with another set of supplement, **much harder** questions for that candidate only.

### Candidates can't write code?
Not really. As I was interviewed so many times (kudos to looking for a job in 3 months), I noticed most companies prefer candidates who can answer quizz questions. It's easier to grade, to measure how candidates can immediately match an ongoing project. But IMO it doens't test how well a candidate work overall. Candidates who recently have a tech gap (aka doesn't use the same tech the project requires) are at a **massive** disadvantages. 

But since "quiz" interviews are so popular, candidates, especially freshers, juniors, and middle engineers all try to adapt to the most popular system. I don't blame them at all: finding a programming job after 2022 has been a challenge. But in doing so, they lose the 2 most important abilities: (1) the ability to write code with their favourite language and (2) the ability to think on their feet, while solving a problem.

I don't know what to say about AI. At this timeline, I only used AI as a supplement, not a primary source of information. We, the (not so) old generation, were taught to not immediately believe anything written on Google or on the Internet. If so, we should expect the same doubt regarding AI. And frankly, in the interview, I was testing candidates' ability to think by themselves, not testing their ability to prompt. If the candidate has a good aptitude, I can expect him or her to leverage AI in daily work well. But I can't say the same if the person is a good prompter. As of writing this in October 2026, I'm confident to say there are problems AI can't solve (in a reasonable amount of tokens). Just like an experienced accountant who can count without using calculator, a good engineer should be able to deterministically work well to a certain degree without AI.

### Conclusion: what sort of candidates are you looking for?
This blog post is too long already. These are my personal opinions what traits a candidate should have. They may not be fully suitable in your case. Please feel free to comment down below what else I'm missing:

- [**Must have**] Candidate a problem solving ability. If you give candidate a problem, candidate should be able to solve, in a reasonable amount of time. Usually this is an algorithm question, or a system design interview. Solution doesn't have to be perfect. Interviewers may hint about an edge case, or an performance improvement, and see if candidate can think of an improvement. You can also see how candidate writes code - if candidate can write fast and/or explain fast, you can be sure that candidate has a fair amount of experience.
This method worked well traditionally before the advent of AI. But now, when AI is so prevalent, maybe just a question is not enough. We can just paste the question into AI chatbox, and get a fairly decent answer in a minute. Also questions can be leaked (as my colleague suspected in an occasion). This format alone is not perfect, but without factoring in AI, you can test candidate's ability to reason through a problem.
- [**Must have**] Candidate must be able to explain his or her choices after submitting the solution. Bonus point if candidate can explain while writing the solution. Solution doesn't have to be perfect, but candidate should be able to explain the choices.
- Instead of a prompt question, we can create a "dummy" project for candidates to work on. Candidates can trace the code and finds hidden bug. This is quite important: usually, when candidates is hired, they are put in an ongoing project. Being able to jump in the middle of a project (parachute) and trace the ongoing code (on whatever quality the code is being maintained) is normal work. This interview format tests this too. Of course candidate who understands the tech stack of the interviewing project has an advantage. If you use a new language like Golang or Rust, you may find it hard to recruit brilliant people who have little background in these technologies (you *know* learning a new languaage on a job isn't that hard, but interview sessions are short).
- Candidate has a set of skills that match, or at least partially match, the project that you are working on.

These are my opinions, code-wise, what traits a candidate should show. This series will continue on, when we look for other traits a candidate should have. Understandably, it's quite hard to judge a candidate, given just an hour interview, and a CV.
