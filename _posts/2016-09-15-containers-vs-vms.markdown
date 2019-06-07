---
layout: post
author: Jonathan Frappier
title: Why are we still talking about containers vs VMs
permalink: /:title/
modified: 2016-09-15
category: devops
tags: [containers, vms, devops]
share: true
related: [
    "Where is VMwares multicloud API gateway?",
	"So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors"
]
---
A few years ago when EVERYONE was going to move to the cloud, I remember all the people arguing over Opex vs CapEx. I pointed then that the discussion was mostly irrelevant, it came down to business requirements and what the business wanted. It's why we, as technologist exist - to do what is right for our organization, not to pick tools because they are the new hotness (anyone burned by HashiCorp dropping Otto already?).

Well, here we are, a couple of years later and the argument has shifted over the years from Opex v Capex, private vs public cloud, 3 tier architecture v hyperconverged and more recently containers v VMs. I was recently asked what my preference is, and you know what - I honestly just don't care personally. For me, it’s not about one over the other - it’s about what is the right choice for the business.

Now, I know what you might be thinking after that last sentence. If containers are smaller, or faster to deploy, then isn't that better for the business? And why chose the word business while I'm hating on you? Isn't it all about the application? Yes...and no.

The application is the most important citizen, but there are other aspects to consider above and beyond what that application is running on. Is there a help desk or operations team that needs to support it? Does current (and forecasted) tooling available to those teams support one over the other to meet their SLAs? Sure it would be great to spin up a container in a few seconds, but if I am retrofitting other systems to monitor and track the container does that put those SLAs at risk if an alert for example doesn't trigger properly? Finally, and seemingly the most obvious - was the application even written to "properly" support running in a container? If I am forcing an application in a container because I want to put Docker on my resume, is that really in the best interest of the business?

Before we leave, and you troll my post, I'm not saying VMs are better. In fact, if the business can, or has better tooling to support containers, if operations/helpdesk teams are staffed to support containers, and the application isn't being forced in because it’s the new hotness, then by all means use a container. But let’s stop with the "containers are better than VMs" or "VMs are better than containers" conversations please.