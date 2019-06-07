---
layout: post
author: Jonathan Frappier
title: Where is VMwares multicloud API gateway?
permalink: /:title/
modified: 2016-09-13
category: vmware
tags: [vmware, api, api gateway, hybrid cloud]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors", 
    "Managing ESXi host DNS using Ansible 2.0"
]
---
At VMworld this year there was quite a bit of talk of multi-cloud or hybrid-cloud support from VMware. NSX, for example, being used to extend networks between on premises and public cloud providers such as AWS and Azure. Now don't get me wrong, this is great but in terms of building a true hybrid cloud I am forced to build all of my applications around vRealize Automation. Forcing organizations to customize their pipelines and workflows around vRA, and not a native cloud provider API forces organizations to make a choice - lock into VMware or walk away (or even worse, maintain two sets of workflows).

What I am hoping to see from VMware, is an "API gateway" which can be used to develop a common pipeline for automation and orchestration, regardless of cloud platform. So you might be thinking, why would VMware build such a thing? As the world moves towards transforming applications to modern, or cloud native formats this service would be far more valuable to organizations than vSphere licensing and could be the value add to existing licenses that keeps vSphere and vRealize Automation in play as companies start to consider how to leverage public cloud. From the people I talk to, having this common gateway would be far more important, and desireable than writing automation against propritary VMware APIs or tools. In my opinion, there will always be a place for workloads to run on premises AND in the public cloud. For most organizations it is not an all or nothing, however that descision may come down to one or the other if they would be forced into maintaining multiple automation pipelines to manage their environments - one for VMware/on-prem and one for public clouds.

One question that comes up to me, is vRA or even VMware Integrated OpenStack meant to be the model that is deployed to allow a common set of APIs? If I set AWS as an endpoint, I can still deploy to AWS through vRA. The problem with that is it assumes vRA is available, or deployed on premises and in the public cloud provider of my choice; which would be fairly expensive. I could deploy VIO and have some set of setup for EC2 compatible APIs, but even that only <a href="https://wiki.openstack.org/wiki/Nova/APIFeatureComparison">offers a subset</a> of what is available, and locks me in to other public cloud providers running and exposing those APIs or Amazon.

What is your thought? Can VMware become a true hybrid cloud broker by enabling automation to work on any platform while not requiring vRA or VIO? Would you rather pay for licensing to have a standard set of APIs for vSphere, AWS, and Azure?