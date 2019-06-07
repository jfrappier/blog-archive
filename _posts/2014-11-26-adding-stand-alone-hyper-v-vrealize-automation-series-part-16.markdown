---
layout: post
author: Jonathan Frappier
title: Adding Stand Alone Hyper-V – vRealize Automation Series Part 16
permalink: /:title/
modified: 2014-11-26
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac, hyper-v]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Creating Entitlements - vRealize Automation Series Part 15", 
    "Add vCenter Endpoint - vRealize Automation Series Part 8"
]
---
A question came up on Twitter recently about how to add a stand-alone Hyper-V server as an endpoint.  What I can gather from the documentation is that you need to have an agent deployed for Hyper-V but the directions were otherwise unclear so this is an attempt to document the required steps.  First, the assumption is you have a Windows Server with the Hyper-V role at a minimum available (if you are running as a virtual machine; <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2044876" target="_blank">make sure the OS type is set to Hyper-V</a>).

First, we need to install an agent for Hyper-V.  For my lab I am doing this on my existing IaaS server
<ul>
	<li>Log into the IaaS server as your vCAC / vRA service account</li>
	<li>Download and run the IaaS installer from http://vcacappurl:5480/i as administrator</li>
	<li>When you get to the Installation Type page; select Proxy Agents</li>
</ul>
<img src="/images/fulls/vra-add-hyperv-agent.png" class="fit image">
<ul>
	<li>Follow the wizard until you get to the Install Proxy Agent screen; enter the following information
<ul>
	<li>Agent type:  HyperV</li>
	<li>Agent Name:  HyperV</li>
	<li>Manager / Model Manager Host Name:  Your IaaS server if you followed along in my vRA Home Lab series</li>
	<li>An administrative user for your HyperV server; then click the Add, and Next button</li>
</ul>
</li>
</ul>
<img src="/images/fulls/vra-agent-configuration.png" class="fit image">
<ul>
	<li>Click the install button and wait for the installation to finish</li>
</ul>
Once the installation finishes, we now need to add the agent to vRA.  To do this, log into vRA as the iaasadmin user and perform the following:
<ul>
	<li>Navigate to Infrastructure &gt;&gt; Endpoints &gt;&gt; Agents</li>
	<li>Provide the FQDN for the Compute resource - your HyperV server, select the HyperV agent from the the pull down and click OK</li>
	<li>Navigate to  Infrastructure &gt;&gt; Compute Resources &gt;&gt; Compute Resources; you should see your Hyper-V server listed as "OK"</li>
</ul>
<img src="/images/fulls/vra-hyperv-compute-resource.png" class="fit image">

If you click on the small magnifying glass icon, you should see information about your server - I can confirm these are the specs for my Hyper-V virtual machine:

<img src="/images/fulls/hyper-v-resrouces.png" class="fit image">
<ul>
	<li>Hover over your Hyper-V server and select New Reservation</li>
	<li>Fill in the reservation information and click the OK button; you should now see your Hyper-V server listed</li>
</ul>
This is just adding Hyper-V as a compute resource, I assume will still need virtual machines in Hyper-V (or templates?) to create blueprints from and entitlements for that blueprint - I'll try to get on that ASAP.