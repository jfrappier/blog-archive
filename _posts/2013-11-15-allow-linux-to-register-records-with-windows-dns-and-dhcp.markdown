---
layout: post
author: Jonathan Frappier
title: Allow Linux to register records with Windows DNS and DHCP
permalink: /:title/
modified: 2013-11-15
category: articles
tags: [windows, linux, dns, dhcp, register dns, windows dns]
share: true
---
The title is actually a bit misleading, its not actually the linux OS registering with DNS, but rather DHCP registering on behalf of the OS.  In order to make this work, there just a couple config files on the linux side you need to edit and a DHCP setting on the Windows server.  One common misconception I ran into often in trying to figure this out were people suggesting to set your DNS server to allow Secure and Non-Secure updates which I had done.  It wasn't until I realized that DHCP was actually handling the registration on behalf of the OS through the use of an authenticated domain account that I realized change the DNS setting wasn't necessary, after all its a secure user account.

So, on the linux OS you have to update two files:

In /etc/hosts add your hostname.domain followed by just the hostname (and wondering now if you need this specifically for this purpose, but I guess its a good habit anyway?)
<pre>127.0.0.1 hostname.domain hostname localhost.localdomain localhost</pre>
In /etc/sysconfig/network-scripts/ifcfg-eth# (where number is your interface number) on RHEL/CentOS add
<pre>DHCP_HOSTNAME=hostname</pre>
Or in Debian based OS' update /etc/dhcp3/dhclient.config
<pre>send host-name "hostname"</pre>
This last step for linux, depending on your OS, is what will send the hostname to the DHCP server.  If you dont set it, you may see an empty name in your DHCP server lease section.

Now, in AD create a service account based on your naming conventions (service_dhcp2dns, fred, larry, whatever works for you).  On your Windows DHCP server set the following

<img src="/images/fulls/dhcp.jpg" class="fit image">

Now, while still in DHCP Manager, right click on  IPv4 and select properties, go to Advanced and click Credentials and enter the user account you just created.  This is why you dont need to change the security settings within DNS, you are securely updating via the user of an authenticated user account.