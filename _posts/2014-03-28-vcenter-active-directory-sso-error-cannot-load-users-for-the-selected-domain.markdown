---
layout: post
author: Jonathan Frappier
title: vCenter Active Directory SSO Error - Cannot load users for the selected domain
permalink: /:title/
modified: 2014-03-28
category: articles
tags: [vmware, vsphere, vcenter, sso, active directory, vcsa]
share: true
---
During a recent vCenter deployment using the VCSA I ran into an error I hadn't run into before with the VCSA (or vCenter/SSO on Windows for that matter).  After an error free install and setup wizard, I logged in to vCenter as administrator@vsphere.local to set my roles and assign my AD groups permission.  However I noticed that there was no identity source for my Active Directory domain, no problem add it in, boom now hop on over to my vCenter permissions tab and get people vCentering.  This is where I ran into errors.  When trying to search for a user I received a pop up that said
<blockquote>Cannot load users for the selected domain</blockquote>
Before I ran the setup wizard, I had SSH'd to the VCSA did some pings and digs to make sure the network bits were flowing properly and everything seemed fine.  I could ping and dig both local and remote  AD resources so I was confident that was all working fine.  Easy fix I assumed, so I headed over to the global KB search tool known as Google and was lead to this KB, http://kb.vmware.com/kb/2033742 which suggested checking DNS, time synchronization and joining and re-joining to the domain.  I manually re-checked DNS records were present, that the AD join process had worked correctly and the account was still enabled.

Looking through the /storage/log/vmware/sso/ssoAdminServer.log I saw several exceptions with the following error (stripped excess text)
<blockquote>Failed to establish server connection</blockquote>
I searched the KB again for this error, but all I found was problems related to accent characters which wasn't my thing.  At this point it was worth while to open a support case.  I wanted to make sure I had all my tier 1 support boxes ticked so I rebooted, verified I could ping/dig records, went back to the Identity Source page removed and re-added the domain and went to look up an AD group to get the error message and... all my users were listed.  I've not quite tracked down what was wrong, but if you are getting this error, and you know your DNS was square try just re-adding the sdentity source.