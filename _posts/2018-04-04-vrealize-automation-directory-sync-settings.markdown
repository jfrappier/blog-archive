---
layout: post
author: Jonathan Frappier
title: vRealize Automation Directory Sync Settings
permalink: /:title/
modified: 2017-10-27
category: advice
tags: [vmware,vrealize automation,vrealize,vra]
share: true
related: [
    "Step 1 of building a DevOps Culture",
	"Using vSphere 6.5 APIs with Ansible" 
]
---
I was setting up a new vRealize Automation 7.3 environment, that process feels so smooth now compared to the 6.x days. One of my first steps once the install is complete is to integrate a directory service for user authentication. I was cruising along all swell but was unable to authenticate with my Active Directory users. No obvious errors in the vIDM logs, no errors on the Domain Controller, no errors when syncing the directory.

The error I was receiving was: Your username or password is incorrect. 

I checked the normal cultrips like DNS and NTP, reset the account password I was testing with but still could not log in. Local user accounts in the vsphere.local domain I created during the install worked as expected.

As it turns out, there are two separate base DN fields that come into play. The first is when you are setting up the initial sync and providing a user account to connect to the directory service.

<img src="/images/fulls/vra-sync-settings.png" class="fit image">

The base DN field here supports the authentication process when logging in. Since you specify the full bind DN to connect to the directory, you could - as I did, have a typo in this base DN field and everything will still connect properly.

The second base DN field is under Sync Settings >> Users (assuming the directory is already created, if you are going through the wizard to add a new directory one it will be configured there).

<img src="/images/fulls/vra-sync-users-settings.png" class="fit image">

This DN path, assuming it is spelled correctly will sync the user accounts even if the DN listed with the bind account is incorrect. Once I corrected the base DN, I was able to log in as expected.

