---
layout: post
author: Jonathan Frappier
title: Clean Up Active Directory Computer Objects w vRealize Automation Custom Properties and Build Profiles
permalink: /:title/
modified: 2015-11-16
category: articles
tags: [vmware, vra, custom properties, ad, active directory, objects]
share: true
---
Chalk this up to I should have paid more attention when RTFD (reading the documentation), but since I missed it tucked away in there, I thought others might have as well. vRealize Automation ships with several custom properties that you can use to delete Active Directory computer objects when destroying a virtual machine deployed through vRA. This is excellent for Windows shops who might otherwise have to build some other means of cleanup. One item to point out, however, is that this will delete the computer object immediately. If you have any type of retention period where you might have to restore these VMs, you would then also need to restore the AD object.

This also assumes you are joining Windows based VMs to AD when they are deployed through a customization spec, or doing it manually but that seems like it would be a pain (I hope that isn't your job to do!)

<strong>Steps to perform as a Fabric Administrator</strong>
<ul>
	<li>Log into vRA as a fabric admin to and navigate to Infrastructure &gt;&gt; Blueprints &gt;&gt; Build Profiles</li>
	<li>Create a new build profile and provide a name such as ADCleanUp</li>
	<li>In the Add from property set area, select ActiveDirectoryCleanupPlugin</li>
	<li>Click the pencil icon next to Plugin.AdMachineCleanup.UserName and enter the <a href="http://sigkillit.com/2013/06/12/delegate-adddelete-computer-objects-in-ad/">username for an account you have delegated rights to delete computer objects</a>, or be bad and user your domain admin user (that is bad, so don't, but if you do I told you so!)</li>
	<li>Click the pencil icon next to Plugin.AdMachineCleanup.Password, click the encrypted checkbox and enter the password for the account used in the previous step</li>
</ul>
<strong>Steps to perform as a Tenant Administrator</strong>
<ul>
	<li>Log into vRA as a tenant admin and edit the blueprint you want to assign the build profile to</li>
	<li>Click on the Properties tab and click the checkbox for ADCleanUp (or whatever you named yours)</li>
</ul>
Log in as a user who has access to the blueprint, deploy it, check AD, destroy it, and check AD again - poof - its gone!