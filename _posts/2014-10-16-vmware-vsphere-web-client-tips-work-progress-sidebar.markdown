---
layout: post
author: Jonathan Frappier
title: VMware vSphere Web Client Tips – Work in Progress sidebar
permalink: /:title/
modified: 2014-10-16
category: vsphere
tags: [vmware, vsphere, vcenter, web client, tips]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
In the Windows vSphere Client, when a wizard comes up, you need to finish or cancel that current task - not so in the Web Client!  When start a wizard in the vSphere Web Client you can click outside of the wizard window back into the vSphere Web Client and your wizard window will disappear.  Let's take a look at an example, below is a screenshot of my lab, highlighted in red is the Work in Progress sidebar:

<img src="/images/fulls/vsphere-web-client-lab.png" class="fit image">

Now I can start a task, in this case the <strong>New Virtual Machine</strong> wizard

<img src="/images/fulls/vsphere-web-client-new-virutal-machine.png" class="fit image">

Now, in the Windows C# classic vSphere client I would need to either finish, or cancel this task if I wanted to do something else.  In the web client, however I can simply click off the wizard back into main UI.  As you can see here, my New Virtual Machine wizard has been minimized into the Work in Progress side bar.

<img src="/images/fulls/vsphere-web-client-new-virutal-machine-work-in-progress.png" class="fit image">

Instead of having to restart the the wizard, I can minimize it to the Work in Progress sidebar, then click on the link in the work in progress window to bring it back, right where I left off.