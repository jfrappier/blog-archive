---
layout: post
author: Jonathan Frappier
title: vRealize CloudClient Data Collection
permalink: /:title/
modified: 2017-01-16
category: vra
tags: [vmware, cloud, vrealize automation]
share: true
related: [
    "vRealize CloudClient Export and Import Blueprints",
	"So you want to be a full stack engineer? Advice on mapping out your career." 
]
---
In my last post, I looked at how you can use the vRealize Automation CloudClient to export, and import blueprints as YAML files, and edit that file to create a new blueprint. In this post I want to look at some basic administrative tasks you can perform from the CloudClient.

A quick login recap, there are two commands to log in:<br>
<code>vra login userpass --server {server} --tenant {tenant} --user {username} --password {password}</code><br>
<code>vra login iaas --server {iaas-server-vip} --domain {domain} --user {user} --password {password}</code><br>

You should now see:
vRA 7.1 login: [ACTIVE]
IaaS Model Manager login: [ACTIVE]

If you wish to check the status of your login at anytime, you can use:

<code>vra login isauthenticated all</code>

Now lets dig into some examples of what you can do with the CloudClient. The first task that comes to mind is running the data collection processes. These processes will ensure vRealize Automation has the most recent information from vCenter, and NSX. When you add a new template to vCenter, for example, you would have to wait for the next inventory data collection process to run before you could use the template in the converged blueprint designer (by default ever 24 hours). If you want it available sooner, you would need to run the data collection manually. In order to do this you will need to know the name or ID of the compute resource, if you were in the GUI you would hover over the compute resource and and click on Data Collection. Then run the desired process. With the CloudClient, 2 commands:

<code>vra computeresource list</code>

Now with the ID of the compute resource, you can run:

<code>vra computeresource datacollection start --name {resource-name} --type {type}

Possible types include: inventory, state, and performance. If you ran the inventory data collection, any new templates, or snapshots would be available in vRealize Automation.

What about NSX? If I am using NSX I can pull security groups into vRealize Automation as well. Maybe you have created a new security group you wish to use, like the template you would have to wait for the next scheduled run. With the CloudClient, know the ID or name of the compute resource, you would run:

<code> vra computeresource datacollection start --name {resource-name} --waitforcompletion yes

This was a trick one, huge thanks to Jon Harris for helping me with that one. Its not obvious, so glad he was able to help me out. Hopefully VMware can add this by name in a future version.

You now have a very quick, and easy way to run the data collection process against your vRealize Automation compute resources.