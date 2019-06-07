---
layout: post
author: Jonathan Frappier
title: Replacing Windows AD and DNS with Synology
permalink: /:title/
modified: 2015-11-11
category: articles
tags: [synology, ldap, ad, windows, replace AD]
share: true
---
I had been looking for a cloud hosted method of replacing my lab domain controller and DNS. Since Amazon offers IAM for free, I had been hoping Directory Services would also be free but it wasn't. As I was turning back to my lab to refresh my Windows domain controller and update my Synology NAS I noticed something interesting; Synology offers both an LDAP and DNS server which can be run directly on the Synology NAS.

<em><strong>TL;DR</strong></em> if you are busy - the Synology Directory Service doesn't support Windows. If that's a deal breaker for you check out some of my other posts :)

Installing the packages is quite easy, log into your Synology and access Package Center &gt;&gt; Explore &gt;&gt; Utilities. There you will find the Synology Inc. Directory Server and DNS Server. Click the install button for both, when the installation completes you will have a new item in your Main Menu

<img src="/images/fulls/directory-server-synology.png" class="fit image">

Open Directory Server and check Enable LDAP Server; you will need to provide a FQDN such as ldap.domain.local and password; when finished click Apply and click Yes to configure client settings. You will now have your base and bind DN's available to add other machines.

In addition to the LDAP functionality you can also schedule directory service backups which is a nice add on. However note that the LDAP service is unavailable while performing the backup, so schedule it for a time you will not need it.

There is one major downfall, however, and suspect it might be a deal breaker for most home labs - the Synology Directory Service does not support Windows :(

Still, since it is installed I am going to look at using it for the vCenter server appliance, a post will follow if possible otherwise I will update this. I guess the computer gods really want me using a Windows DC!

You can learn more about the <a href="https://global.download.synology.com/download/Document/UserGuide/Packages/DirectoryServer/DirectoryServer_enu.pdf" target="_blank">Synology Directory Server in their user's guide</a>.