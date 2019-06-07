---
layout: post
author: Jonathan Frappier
title: VMware Application Services Linux deployment hangs at agent_bootstrap node setup
permalink: /:title/
modified: 2014-12-01
category: articles
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Deploy an Application Blueprint - Application Services Series Part 5", 
    "DevOps IRL Boston VMUG Usercon with @szelechoski"
]
---
When deploying an Application Services blueprint, you notice that the workflow does not move past the 2nd step in the provisioning process - agent_bootstrap node setup, however the previous step which renames the virtual machines appears to work fine.  In this scenario you have also successfully installed the AppD agent in the vSphere template.

<img src="/images/fulls/application-services-hangs-agent_bootstrap.png" class="fit image">

If you log into the virtual machine, you can see that the vmware_appdirector_agent should be set to on, however when checking the running services (for example by running ps -ef | grep vmware_appdirector_agent) you do not see any processes running.

When working with vSphere templates that are used in Application Services / Application Director blueprints there are a few things to be aware of.  After the agent installation initially, you shutdown the virtual machine, however making changes to the template after the initial shutdown requires additional steps to be performed.   Not only do you need to remove the /etc/udev/rules/70-persistent-net.rules file and make sure the vmnic MAC is not in /etc/sysconfig/network-scripts/ifcfg-eth0 but you also need to run
<pre>/opt/vmware-appdirector/agent-boostrap/agent_reset.sh</pre>
This reset the agent configuration and allows it to start properly after being cloned.  Once the agent_reset script has been run, shutdown the host and convert it back to a template.  You should now be able to run your Application Services / Application Director blueprint (or at least get past that step :) )

<img src="/images/fulls/application-services-completes-agent_bootstrap.png" class="fit image">