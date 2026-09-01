---
title: "But it's not my fault!"
date: 2026-08-30 00:00:00 +0700
categories: [EN-But it's not my fault]
tags: [EN-But it's not my fault]
---

# But it's not my fault

Welcome to the series **But it's not my fault!** This is where I talk about fun stories in my career. Some of the fun stories are my faults, some are not (hence the name of the series).
I hope that from the stories in the series, both you and I can learn from these. Or at least, I hope you find these stories entertaining. Most of the stories are recollections of events in the past. There can be points I miss, and there are my own personal opinions and observations, too. Please feel free to comment, and let me know what you think.

This is the first post in the series. I come up with this name "But it's not my fault", because, in my opinion when this happened, it's not my fault. But it's my responsibility to prove that to my manager.

*Date: May, 2025<br>*
*Context: We need to apply PDF translation on PDF annotation. The previous code, written in Python, does this successfully. But now, as we intend to move away from Python library (to use a service in NodeJS), it seems harder than expected, because the equivalent library doesn't exist, or at least we don't know how to. I was in **probation** during this time, and I was assigned to work on this hard bug that no one seemed able to do. Can I do it?*

### Feature explaination
We have a Translation tool. Input is PDF, and the output is the PDF, plus the translation text onto the PDF. There are 3 translation mode: **Overwrite**, **Side by Side**, and **Annotation**.

For example, here is a sample PDF.
![Sample PDF](assets/images/But-its-not-my-fault/First-post/Sample-PDF-image.png)

*(The text says: I see there is a cat, in French)*

In **Overwrite** mode, the translated text will be written directly on top of the original text, like this.
![Sample PDF](assets/images/But-its-not-my-fault/First-post/Sample-PDF-image-overwrite.png)

In **Side by Side** mode, the text is similar to Overwrite mode, but the original PDf is displayed side by side with the translated PDF. You can view either or both PDFs at the same time.

In **Annotation** mode, the text to be translated has a clickable icon. Often times it looks like a sticky note
![Sample Annotation](assets/images/But-its-not-my-fault/First-post/Sample-PDF-image-annotation.png)

Clicking the annotation icon to reveal the translated text
![Sample Annotation click on text icon](assets/images/But-its-not-my-fault/First-post/Sample-PDF-image-annotation-click.png)

### Codebase 
The original codebase, written in Python, has done all these features perfectly. The only problem with the Python codebase, is that it is a monolithic God object: it does everything. 
There has been a decision somewhere, before I joined, to migrate and break down the original Python codebase into smaller, *microservice* Python and TypeScript (NodeJS) services. At the time I was assigned this problem, I was still in probation.

*Sidenote: why do people love microservice so much? I'm guessing the original codebase, while developed as a POC, was a god object. But instead of breaking it down as a module, the developers decided to break down Python service into Python and NodeJS. Adding a new languagae to the current solution seems like a logical decision for them. I didn't question, though, and I accepted the fact as is. Maybe I should have questioned them why they were breaking down the God object Python in the first place? I never saw Archtecture Record Decision (ARC) documents, and I don't even consider looking for it.*

The application has backends in Python and NextJS with [pdf-lib](https://pdf-lib.js.org/) (there is a Rust service, but that is irrelevant), and a frontend in [react-pdf-viewer](https://www.embedpdf.com/react-pdf-viewer/). In my probation, I only have experience in backend (Java), and it's been a long time I haven't done Python, and this job was the first time I worked in TypeScript (at least I know Javascript a little bit). Worst, I know nothing about frontend, except HTML-CSS I learnt at school (this had a significant effect later on). Looking at React syntax seems like reading Egyptian hieroglyphs.

The new TypesScript service can do Overwrite mode and Side by Side mode well, but it cannot do Annotation mode. Why, I don't know. That's when I was assigned the task. Lesson learnt early in probation: don't accept a ticket without knowing why, especially when the previous team postponed the ticket. There was a chance I was given a task that can't be solved, or unusally hard to solve. Accepting tasks without knowing how hard it is is a horrible thing to do, now that I look back. There was a reason this was a stinking task that original developers didn't want to do. But I thought, if this was done in Python in the first place, then it could be done in Typescript, right?

### Task began - resolve on the backend TypeScript side
I worked on NextJS with [pdf-lib](https://pdf-lib.js.org/)

I search Google how to implement Annotation mode for TypeScript and pdf-lib. No result. It's a bit harder than it looked. Maybe that's why no one has done this before. *This isn't the first time I was sent to do something no one wanted to do in my probation*.

Asking AI (my personal, *free* Copilot account) doesn't help. (First time I was forced to use AI on the job, when all Google search failed, by the way).

I looked how my colleagues implemented Overwrite mode and Side by Side mode, and, I checked out [pdf-lib Github](https://github.com/hopding/pdf-lib) source code. I cross referenced the library source code versus my colleague's function calls. I was hoping there might be an existing function (or at least easy to be found solution) in the library so I could use. No result, but I was following the right direction.

**Then I found the solution**. Instead of looking on Google, I looked on [pdf-lib Github pull requests](https://github.com/hopding/pdf-lib/pulls). Instead of trying to look on Google (which often points to StackOverflows and Github references), I looked directly on Github itself. There were a few hundred pull requests at that time (I remember only about 120), closed and being open alike. I only needed to find all one by one, and if I could filter the right keyword, I could copy and and paste the solution.

Done. But there was a problem. The icon on frontend looks horrible. It looks something like this.
![Sample Annotation after fix](assets/images/But-its-not-my-fault/First-post/Sample-PDF-image-annotation-after-fix.png)

The function works correctly: clicking the icon shows the translated text. It's just it looks ugly, and nothing like when you use the original Python backend.
Since the feature broke when we switched the backend from Python to NextJS, the responsibility seemed to come from backend.

I presented my current "solution" and the problem I was having. I was given extra time to deal with this.

### Further investigation - frontend

Next: I read our frontend code, and I even checked out frontend library [react-pdf-viewer source code](https://github.com/react-pdf-viewer/react-pdf-viewer). I could not understand a thing. *I have never seen Javascript and HTML elements together in a React code*. I had no idea how React works, and spending 4 - 6 hours learning React from scratch is a risk that might lead nowhere. After all, the prolem happened after I changed backend system from our Python service to Typescript service. I should investigate backend system, and leave the frontend alone.

Asking the only frontend member in Vietnam office didn't go anywhere. He was not available. I was on my own regarding React matter.

At least, I found the annotation SVG icon. But it's written in code, and I could not display it to confirm that's how the ugly the icon looked on frontend side, or, the error came from the frontend side.

### Put off investigation on frontend. Back to backend.
Next, I dissected the PDF itself. I tried to editt the result PDF using Visual Code. I asked Copilot for help, but I could not find anything useful. 

Next, I wanted to know how PDF is structured. I managed to find (PDF open specification from Adobe itself)[https://www.adobe.com/accessibility/pdf.html]. It's a 700 pages PDF book, detailing how a PDF is to be like. I wanted to understand the meaning of the PDF structured that I looked at in Visual Code, *especially the annotation part*. Nothing serious. But I found something important: how annotation works, specified by Adobe. The annoatation is only like 50 pages. I cross referenced PDF annotation document with how my TypeScript handled annotation. Everything checked out. I did everything right, *by Adobe PDF specification*!. So where did I go wrong? Why did the previous Python god object work, and my TypeScript didn't? Why did my icon look so ugly? Function wise, it's not a big deal, but since end user won't accept an ugly icon, I have to investigate.

Next, I diseected the Python god object. It does everything that all of our microservices were doing: extract text, translate, find the correct area, handle Overwrite, Side by Side, and Annotation mode. It calls a function from a Python library (PyMuPDF)[https://pymupdf.readthedocs.io/en/latest/].
*insert PyMuPDF function here.*
I checked out [PyMuPDF source code](https://github.com/pymupdf/pymupdf). I wanted to know how it works. The Python library leaded me to another [C library mupdf](https://github.com/ArtifexSoftware/mupdf/tree/master).


Looking around a bit, I found this bit of code:

```
static const char *icon_note =
	"0 0 8 1 re\n"
	"0 2 8 1 re\n"
	"0 4 8 1 re\n"
	"0 6 8 1 re\n"
	"f\n";
```

**Found it**! This is the annotation symbol - the sticky note in the PDF. *I didn't forget everything after years of game programming*. From just my reading, I guessed the code looks like 3 lines on a colour background
But reading source code isn't enough. I confirmed I was correct when I threw this bit of code in Copilot and asked what it meant in the library. *I didn't mention my suspicion that this icon might look like a sticky note*.

Here is Copilot answer (copy from Copilot history)

![Sample PDF](assets/images/But-its-not-my-fault/First-post/annotation-from-C-library.png)

**Confirmed!** The reason why changing backend system caused the bug is that the annotation symbol (the sticky note) was drawn from the C library in the backend. How that backend image override the faulty ugly image in frontend, that's not my concern. But now that I can confirm the error comes from frontend (or rather, the ugly symbol was from the SVG in frontend library).

I could have asked the only frontend developer in Vietnam office for help (we have engineering office in Japan, Viet Nam, and the US). He didn't directly help me, but I remember, he wrote a CSS, which I copied the format, threw the prompt in Copilot to ask me to draw the sticky notes above in CSS, and got the result.

```
.rpv-core__annotation-text-icon {
    color: yellow;
    background-color: yellow;
    width: 40px;
    height: 40px;
    position: relative;
    border-radius: 4px;  /* Slight rounded edges for a sticky note look */
    box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);  /* Soft shadow for depth */
}

/* Adding horizontal lines */
.rpv-core__annotation-text-icon::before {
    content: "";
    position: absolute;
    top: 30%;
    left: 10%;
    width: 80%;
    height: 2px;
    background-color: black;
    box-shadow: 
        0px 6px black,  /* Second line */
        0px 12px black,  /* Third line */
        0px 18px black;  /* Fourth line */
}
```


Frankly, it's just an yellow rectangle, with 3 black lines on it to resemble a sticky note. It doesn't look too beautiful, but I'm not a graphic designer. Looks good enough for me! The yellow rectagle was a bit too big, so I scaled it down a little bit. 

Final commit: my annotation using TypeScript at the very beginning, and the CSS at the very end.
Knowing some React could have saved me a lot of debugging time, *but if only I had known React*.


Lessons from the series.

| What I did | What I could have done better |
| ----------- | -------------------------------------------------------------------------------------------------------------------- |
| I didn't know why this task was necessary | I should not have accepted a task without understanding why it was postponed.<br>Why was the original Python god object being replaced<br> by the new TypeScript service? |
| I used personal free Copilot for advice | I could have asked if our company had any AI service to use.<br> This was the first time I used AI on a project.<br> Heck, a few months earlier, when I was interviewing candidates for CMC Global,<br>I disdained when candidates asked permission to use ChatGPT<br> during their codes interviews.<br> Later on, I learnt that our company had Gemini Pro for employees. |
| I checked out library source code | This was new. I can read the source code and the requirement specs. When documentation fails, read the PRs. Then PR fails, read the source code.<br> Maybe because in the past, I used to work for big, big corporations<br> and we rarely used external, open source library<br> (most libraries were either framework or internal).<br> Libraries aren't perfect.<br> Also, I wonder if the best AI (Claude Sonnet, Claude Opus) can fix these similar bugs?<br> In my case, the error happens in the frontend library.<br> But what if developers have to check out multiple levels of library?<br> Can the AI be persistent enough to do that?<br> With enough tokens, I think yes,<br> but I think the token cost would be painful.<br> At the time of this writing (August 2026),<br> I have seen cases where I give the documentation link to Claude Sonnet,<br> and the code result is still broken. |
| I searched for similar problems I had on Github| This was new.<br> At the time of this bug (May 2025),<br> I never thought Google search index could leave such gaps.<br> There are searchable results that do not show up on Google results.<br> Now I learn if we know where the library comes from,<br> we can search if someone else has ever encountered our problem,<br> not on Google, but on Github.<br> Will AI agents be smart enough to do this too?<br> Maybe yes, maybe no|
| I read the Adobe PDF specification | Good to know a new technique.<br> In this case, reading the specification only confirmed<br> that my solution on the backend was doing as intended.<br> Sometimes you have to read the specification.<br> Not a fun thing to do, especially when you are on deadline|
| I asked the only frontend engineer for help | I don't know how this could be better.<br> If my colleage slacked off with a valid reason<br> (family member in traffic accident),<br> then the best I can do is to report the situation to my manager,<br> and ask for extra resource to work on, *if the timeline allows*. |
| I commited the solution,<br> knowing that the Annotation icon<br> was drawn by CSS and it's not perfect | Might as well commit.<br> I explained to my manager about the cause of the bug and my solution.<br> I think the drawing was good enough for my manager, too.<br> An imperfect feature is better than no feature in this case.|

And from that point, I embraced a new era, of using AI in coding. It's far from perfect, but it's better when Google doesn't help. And AI can visualize your code quite well.