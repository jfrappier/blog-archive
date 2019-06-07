---
layout: post
author: Jonathan Frappier
title: Creating Reservations - vRealize Automation Series Part 11
permalink: /:title/
modified: 2014-11-20
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---

So far we are doing pretty well, but we aren't quite ready to turn vRealize Automation / vCloud Automation Center lose yet, next we will create reservations so users can't consume all of our resources.  Wait wait wait....why would we create a reservation to do that you fool - reservations "reserve" something for us - <a href="http://www.amazon.com/VMware-vSphere-Resource-Management-Essentials-ebook/dp/B00IJYDNYO/ref=tmm_kin_swatch_0?_encoding=UTF8&amp;sr=&amp;qid=" target="_blank">haven't you ever read Jonathan Frappier's book VMware vSphere Resource Management Essentials</a>?  Why yes, I have its a lovely book but a reservation in vSphere is not the same as a reservation in vRealize Automation / vCloud Automation Center - in fact they are generally used for opposite reasons (can you tell I'm working overnights in some of these posts :) ? )

In vSphere a reservation does what I would expect a reservation to do, it "reserves" resources for me.  If I reserve a certain amount of memory for a virtual machine, I am guaranteeing that amount of memory for it.  In vRealize Automation / vCloud Automation center a reservation is "reserving" a certain amount of my resources for my tenant - but generally this is a subset of the resources available - essentially I am limiting what they can consume.  Now I could set my reservation to have all available resources in the cluster, but in a self service model I would advise against this unless you are dedicating physical resources to a specific business group; but then are you really sharing resources?  Remember the point here is to be efficient.

If you are not already, log into vRealize Automation / vCloud Automation Center as the iaasadmin user we created or if you have your own set of users, someone who has the fabric admin role for your tenant.
<ul>
	<li>Click on the Infrastructure tab &gt;&gt; Reservations &gt;&gt; Reservation Policies</li>
	<li>Click New Reservation Policy</li>
	<li>I am going to match my reservation policy names to my business groups; so WalkingDead and StarWars (Don't forget to click the green circle / check mark to save each entry)</li>
	<li>Click New Storage Reservation Policy</li>
	<li>Naming convention will match the above with a -Storage at the end so WalkingDead-Storage etc...</li>
	<li>Click on Reservations in the left navigation menu</li>
	<li>Hover over New Reservation &gt;&gt; Virtual and click on vSphere (vCenter)</li>
	<li>Select the cluster from the Compute pull down menu</li>
	<li>The name field will fill in automatically; I am going to edit this slightly to include my business group game so cl01-Res-StarWars and cl01-Res-WalkingDead</li>
	<li>Select the matching business group from the Business Group pull down</li>
	<li>Select the matching reservation policy</li>
	<li>The quota field is optional and can be used to limit the number of virtual machines the business group user can provision.  For fun lets set WalkingDead to 10 and leave it blank (unlimited) for StarWars</li>
	<li>Set the priority to 1.  The priority field is used if you are going to create multiple reservations for the same reservation policy.  For example if we had two, the second one we create would have a priority of 2.  When a user tries to provision a virtual machine from the catalog it will use the lowest priority unless it is not available any longer then move to alternate resources, for example maybe vCloud Air.  Below is an example from my system</li>
</ul>
<img src="/images/fulls/vra-reservation.png" class="fit image">
<ul>
	<li>Click on the Resources tab</li>
	<li>Here you can limit the amount of memory that could be consumed, notice the "reserved" column is currently 0 (zero) - set the reserved amount to 6</li>
	<li>Click on one (or more) of the datastores you want to be available; enter the amount to reserve and the priority.  Don't forget to click the green circle / check mark.  For example:</li>
</ul>
<img src="/images/fulls/vra-reservation-storage.png" class="fit image">
<ul>
	<li>Look at the various datastores; you can edit and make only certain datastores available to certain reservations by disabling the ones you do not want to be included</li>
	<li>Click on the Network tab; here you select which networks the virtual machines will attach to.  I am selecting vm which is the port group on my VDS for virtual machine traffic</li>
	<li>Click the OK button</li>
	<li>Repeat for other business groups</li>
</ul>
Notice anything different in the Reserved Memory column this time?  You got it, we reserved memory in the first reservation we created.  You can over commit your reservations, but be careful as you would with any overcommitment.  Now that we have reservations setup, time to create some templates and blueprints.