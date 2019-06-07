---
layout: post
author: Jonathan Frappier
title: Getting to know ViPR SRM
permalink: /:title/
modified: 2015-04-09
category: articles
tags: [emc, vipr, vipr srm]
share: true
---
<em>**Disclaimer: I am an EMC employee, this post was not sponsored or in any way required by my employer, it is my experience getting to know this particular product.**</em>

One of the upcoming tools I will be working with is ViPR SRM. ViPR SRM is a storage management tool that allows for monitoring the end-to-end health of your environment. I know what you're thinking, "C'mon now Frapp that sounds awfully marketingy" and you're right - it does, BUT let me give you an example of why some of the tools in ViPR SRM interest me.

<img src="/images/fulls/network-is-fine-300x300.jpg" class="fit image">

Have you ever went over to a friends cube to chat and they say the app it ain't no good? The reports are slow, the app keeps crashing, and the chicken taste like wood. Okay, but seriously how many times has someone walked over and said "my application is slow/down/broken" with no further detail, leaving it up to you to isolate what is going on? It has happened to me often. Worse is when you are the personal responsible for storage and someone else responsible for networking does the Jedi hand wave and says the network is fine, it must be storage.

That is where ViPR SRM comes it, it can show you the relation from virtual machine, through the hypervisor, datastore, data path to the storage array hosting the virtual machine. Further, for heterogeneous it supports multiple types of applications, operating systems, hypervisors and storage arrays. Of course it supports more EMC products, since it is an EMC product but you don't necessarily have to run an EMC array to leverage ViPR SRM.

Below are some of the systems supported by ViPR SRM, an updated list can always be found at <a href="https://store.emc.com/us/Product-Family/EMC-ViPR-Products/EMC-ViPR-SRM/p/EMC-ViPR-SRM?showPrice=true&amp;isProdActive=true&amp;productClassCode=System#Specifications" target="_blank">emc.com</a>.

<img src="/images/fulls/vipr-srm.jpg" class="fit image">

While getting ready for the installation, know that you can deploy as either a pre-packed vApp or install the application on 64-bit versions of RedHat, CentOS, SUSE, or Windows; during my post I will be deploying the vApp version which includes 4 virtual machines. The 4 virtual machines each have unique roles as a typical multi-tier application would - there is a web front end for UI and reporting, database backend for storing data, and collector for, well, collecting data. In large environments with multiple arrays you may deploy multiple collectors.

<img src="/images/fulls/vipr-srm-components.png" class="fit image">

In my next few blog posts I'll be reviewing the installation of ViPR SRM, and review some of the dashboards and how they might help you in the day to day monitoring, and troubleshooting of your environment. If you'd like to learn along with me check out the ViPR SRM <a href="https://community.emc.com/docs/DOC-34286" target="_blank">free e-Learning on ECN</a>.