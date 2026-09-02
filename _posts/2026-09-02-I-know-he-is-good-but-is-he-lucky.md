---
title: "I know he's a good engineer, but is he lucky?"
date: 2026-09-02 00:00:00 +0700
categories: [EN-But it's not my fault]
tags: [EN-But it's not my fault]
---

Often attributed to Napoleon Bonaparte, the original quote looks like this:
- I know he's a good general, but is he lucky?
- I'd rather have lucky generals than good ones.
- In war, luck is half in everything.

![Napoleon crossing the Alps](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/David_-_Napoleon_crossing_the_Alps_-_Malmaison2.jpg/960px-David_-_Napoleon_crossing_the_Alps_-_Malmaison2.jpg?utm_source=en.wikipedia.org&utm_campaign=imageinfo&utm_content=thumbnail)

I was incredibly lucky. More luck than talent.

*Date: December, 2025<br>*
*Context: Our company manages technical engineering drawings in PDFs for our customers. In Vietnam, there is a customer unwilling to use our services, saying that their drawings are in DWG format (for their AutoCAD software), and they don't have the manpower to convert 60,000 drawings to PDFs (some customers have more than 100,000 DWG drawings). This is the biggest blocker. AutoCAD can convert each DWG file to PDF, but there is no way to convert multiple files, let alone 100,000 files. It will have to be done manually one-by-one. Our company doesn't have manpower to do the same. Is there anyway to convert 100,000 DWG files to PDFs?*

As I did my research, I began to unravel: AutoCAD is the most popular software in engineering and construction. They work on a specific file format, DWG. When users want to (literally) print the drawings to paper, they can convert the files to PDF. AutoCAD actively supports converting to PDF *per DWG file*, but no one will click through 100,000 files to do that.

Worse, DWG is proprietary and closed source. There are numerous open source options out there to convert DWG file to PDF, but their accuracy is questionable. There are also paid solutions to convert DWG to AutoCAD, but these solutions had to be tested first. 

Then I got some good news: in my company, there were already 2 separate solutions for this problem. One came from our office branch in Chicago, USA, and one was from my colleague in Ho Chi Minh, Vietnam. My manager asked me to evaluate these 2 solutions.

### Solution 1: from Chicago, USA.

The maker of AutoCAD, AutoDesk, actually has an HTTPs API to convert DWG drawings into PDF. Our US colleague wrote a Python script that uploads DWG files to the API to convert them to PDFs. I created an AutoDesk account, then tested my colleague's Python scripts. But there were a few issues:
- You need to set regions in the AutoDesk API. I looked up and realised that only regions in the US were available. But many of our customers were Japanese, and compliance dictates that data customer stays in Japan. Even when our Japanese customers opens any factory in Vietnam, customers still want to control on who can receive their strict confidential engineering drawings. This is the biggest problem, and for this problem, this solution is not applicable for (Japanese customers) in Vietnam.
- We need a backend to access AutoDesk API. This may incur additional costs. Not the biggest problem, so far.

Evaluation: given a few sample drawings, the result PDFs looked very identical to the original DWG files.

### Solution 2: open source solution, from my colleague in Vietnam
My colleague took this challenge before I did. He ran an open source [LibreDWG](https://en.wikipedia.org/wiki/LibreDWG) on a VM on Google Cloud. I did some research and I knew: DWG was proprietary and closed source. There is no way an open source solution can reliably convert DWG to PDF. And even if it can do right now, in the future, if AutoCAD changes their DWG format, the solution has to be updated, too.

Evaluation: given a few sample drawings, the result PDFs missed a lot of details. There is no way a mechanical engineer will ever accept losing so much details. And our company manages PDF drawings, and there is no way our company can work on such defected PDfs. My colleague's solution didn't work, but I was grateful: he explored a path I didn't want to go.

### My solution

I did some further research. I knew that AutoCAD only works on Windows. Somehow I learnt a critical fact: AutoCAD supports CLI. You can use the terminal to call AutoCAD! Thus, I can invoke AutoCAD to convert the DWG drawings to PDF. Now I can have: 
- Native support, so accuracy is guaranteed.
- Script, so I can have automation.

*How did I even look for AutoCAD CLI in the first place? Maybe I used Windows at home? Maybe in the past, on Windows, I have tried to open Notepad++, Chrome and other applications using CMD, and now I was trying to do the same? Maybe because my company provided everyone with Macbook M3, M4, and thus, my colleague was used to using Mac for a long time, and thus didn't consider Windows? Until this point, I still don't know how I considered using AutoCAD CLI on Windows. In any way, I got lucky, and I found the solution in the simplest way possible.*

Either way, there is a way. I submitted my solution, quoted official sources from AutoDesk that supported my solution. I thought to myself: since AutoCAD runs on Windows, the solution had to run either on Windows VM backend. I suspected AutoDesk might not appreaciate that I was running AutoCAD on my company's backend to serve *other* customers, for financial purpose. I read through AutoCAD's 80 pages Terms of Use (manually, all 80 pages, with AI only at the end to confirm by the way). While I could not host AutoCAD on company backend, I could, however, run the solution on *client* side, on client's Windows.

Then there was my sale's counter argument: we could not use AutoDesk for commercial purposes. I looked up AutoCAD's terms of use and read all of that. I flipped through AutoCAD's 80 pages Terms of Use, again, for confirmation. Nothing said I was not allowed to allow our customers to use our script on customers' Windows machine. After all, it served our customers: to convert all drawings to PDF for customer's needs. Heck, I could write a blog post telling customers how to convert all their DWG drawings (100,000) to PDFs, and I wouldn't break any of AutoDesk's Terms of Use.

Implementing the solution was simple: I wrote a Batch script on Windows that called AutoCAD with the necessary parameters. I recursively looped through every folder and subfolder, and feed any DWG file to AutoCAD. Some drawings may look better if the parameters were different, but the technical drawings in result PDF were still valid, and our company AI system could still read well. For any exception case, the file's name and path were written down in a log file, so clients can export manually (and we could investigate later). There were other concerns: it worked on my machine, would it work on client machine? What if user lacked permission? What if user drive didn't contain enough space? I tried every precaution I could think of before shipping the script. Frankly, there can be always improvement, but in this case, if there were any exception, we could deal with that later. Right now it's more important to ship to serve the immediate potential customers.

Evaluation: given a lot of drawings, on multiple layers of subfolders, the script reliably converted all DWG drawings to PDFs. Heck, my sale guy asked for further support DXF file (another file format from AutoCAD). My solution worked beautifully. Plus, 
- No data leaves the client machine, or the country.
- The solution relies on client's subscription to AutoCAD. It doesn't run on my company machine, and we do not break any commercial SaaS restriction.
- We incur 0 cloud cost.

*Requirements can be extended or even changed. That's quite normal when you try to provide custom solutions to your users.*

But the effect was huge. Immediately, we had new ways to convince new big customers to try out our company's new products: Panasonic Energy, HMC Polymers, MFEC. The head of our company in South East Asia complemented me, and asked sales members to refer to the tool (running on client machine) if future clients had the same problem.

I got lucky. My colleagues in Chicago office and in HCM office explored paths I could have explored. And partly, I think, I was lucky because of very small factors that seemed irrelevant: I used a Windows at home, I first learnt programming by learning Windows Batch script and CMD, and I even tried to work without a mouse before, to open Notepad++ and Chrome using just CMD and my keyboard, with no mouse.