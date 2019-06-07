---
layout: post
author: Jonathan Frappier
title: So you want to be a full stack engineer? Advice on mapping out your career.
permalink: /:title/
modified: 2013-05-31
category: articles
tags: [devops, career, full stack engineer]
share: true
---
<img src="/images/fulls/fuller-stack.png" class="fit image"> The ability to transform your career are critical in the technology industry, even more so of late as roles transform from a limited scope such as a VMware engineer to the DevOps or Full Stack engineer roles. Making this transformation is not new, many readers of my blog are likely involved with some form of virtualization. This technology was just taking off between 2005-2010. If you were in a technology role before this time, virtualization was a skill you needed to add to your repertoire. For example, I started out as a desktop administrator tasked with creating images for systems and supporting users. From there, I moved into roles more focused on network and server technology, followed by brief a stint in management positions. I finally settled in to virtualization and cloud related work over the last several years. However times are a-changing once again and, except for a select few companies, having knowledge or specialty in just one area won’t be enough.

So how exactly do you move from one type of role to another. In this post, I’ll try to explain how I have looked at this and attempted to add new skills.

A great, if not totally required question to start with is, Do you see yourself staying at your current company or will need to (or planning to) make a change? Why this question, simple really – if you have a good manager and co-workers who are supportive of one another and you have interesting projects at work then your transformation can start within the scope of the technology your organization uses. Essentially they are your map (we’ll look at this more in a moment). If, on the other hand, are looking to leave or feel the need to leave in order to advance your career, you will need to research what technologies you want to work or will make you most marketable. Let’s take a look assuming you are going to stay at your current company.

Currently you are a virtualization engineer at your company. The scope of your role likely includes at least some understanding of storage, networking, and monitoring. You also probably at least have a working knowledge of the operating systems running on your virtualized environment whether that be Windows or Linux. You may also have some outside knowledge of the applications running on those virtual machines; for example a web application or client/server application. You may not be aware of the tools your application or release team uses, or specific development tools used. If you were to visualize this, maybe it looks something like this today:

<img src="/images/fulls/today.png" class="fit image">

The area in the green box is what you are responsible for today, but you at least have knowledge of how and what type of storage is presented to your environment (traditional like a VNX/XtremIO, or local storage aggregated by a software solution like ScaleIO, other HCI solutions like SimpliVity, or a combination of both). This is clearly a small, narrow view of the technology your company uses, but by working with other groups, you can literally expand the map of your “stack” to see where your gaps might be. Here is an example, assuming that virtualization lives towards the middle of the stack.

<img src="/images/fulls/fuller-stack.png" class="fit image">

Now you can start see where the opportunities are to add a new skill.  You can start to research some of the specific technologies used in each category by asking questions about each:

<img src="/images/fulls/questions.png" class="fit image">

As you can see, some questions may even cross different disciplines – package requirements for the application are certainly relevant to the operating system, as well as how they are deployed and configured. As these questions get answered, you will find out which tools you need to learn in order to support the “full stack” of your application. Here is another example with those questions, and more answered and some problems unearthed!

<img src="/images/fulls/tools.png" class="fit image">

Not only are you becoming more familiar with you application team and their tools, you may potential uncover problems along the way! What if the SLA for the database platform is less than the application itself if the application relies on the database? What if the Nagios monitoring intervals do not fall within your SLA? A 5 minute check interval may seem reasonable, but a single outage that you are not notified about for 5 minutes already puts you at 99.99% for the entire month, and with manual intervention required that could easily add more time to the outage. Not only have you identified opportunities for your own personal growth, you have positioned yourself to help other teams within the organization (my preferred way to learn is by doing!), and for those of you stuck in siloed organizations, maybe…just maybe this the hole the silo needs to break by supporting each other!

My one tip here, do not get overwhelemd – YES there is A LOT to learn but you can easily start with projects that align through the stack as we uncovered in this scenario, or simply start “thinking in code” – take your everyday tasks and start scripting them. Pick a single, common task that you do and work on that script. Today it may just be hacked together for examples from others, but work on understanding how it works and improve upon it. Eventually this script could evolve into something you make available to others in your company so this particular repetitive task is now automated and can be completed by co-workers. I joined Scott Lowe to discuss this on his podcast which you can find on his website.

While this is only a quick run down, I hope it provides you with some ideas on how you can go about mapping out your next evolution.

Still want more? Check out my podcast with Scott Lowe on <a href="https://itunes.apple.com/us/podcast/the-full-stack-journey/id1073172158?mt=2&i=369510786">The Full Stack Journey</a> and Eric Shanks <a href="http://theithollow.com/2016/05/31/wanna-full-stack-engineer/">excellent take on some of the hurdles you may face.</a>