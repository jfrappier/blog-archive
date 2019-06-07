---
layout: post
author: Jonathan Frappier
title: VMware Workstation Home Lab Setup Part 4 - Domain Controller setup
permalink: /:title/
modified: 2014-11-04
category: articles
tags: [home lab, windows, AD, DC, active directory, domain controller]
share: true
---

Alight, so far we have built our Windows template in VMware Workstation that we will use for various home lab purposes, cloned it and got the first clone ready to be a domain controller.  Given the limited resources in the lab, I'm not sure I want to tackle PKI at this time, though maybe I'll try a lightweight opensource project at some point.  Anyways back to why you are here, configuring Active Directory;
<ul>
	<li>The last thing to do before promoting the server to a DC is to give it a static IP address, after all we don't want that changing (even if we are using DNS for everything).  Bring up the Start menu</li>
	<li>Click on Control Panel &gt;&gt; Network and Sharing Center &gt;&gt; Change Adapter settings</li>
	<li>Right click on Ethernet0 and select properties</li>
	<li>Double click on Internet Protocol Version 4 (TCP /IPv4)</li>
	<li>Change Obtain an IP address automatically to use the following and enter the IP information for your network.  In my case I will set it to 192.168.6.5 with a subnet mask of 255.255.255.0 and a default gateway of 192.168.6.2 for my NAT'd VMware Workstation network.  If it is not already, set the Preferred DNS server to 127.0.0.1</li>
	<li>Server Manager should still be open from the previous post - if not open it.</li>
	<li>Click on AD DS in the left navigation menu.  You should have a yellow bar that says Configuration required... click the Yellow Triangle in the upper 1/3 of the window</li>
</ul>
<img src="/images/fulls/windows-configure-ad.png" class="fit image">
<ul>
	<li>Click on Promote this server to a domain controller</li>
	<li>Select the Add a new forest radio button</li>
	<li>Specify your root domain name.  If you are included to pay for SSL certificates use a valid TLD that you own as there are very few providers offering certificates for private domains such as .local.  I am going with all self signed certificates in my lab (for now) so I've chosen vxprt.local (.lan has troubles with OSX...at least it used to) and click Next</li>
	<li>On the Domain Controller Options page, you can change the functional levels if you think you'd ever need to introduce and older domain controller, its unlikely so you should only need to add the DSRM password, then click Next</li>
	<li>On the remaining steps, just click next (or review information provided if you like)</li>
	<li>On the Prerequisites Check page, click Install.  The VM will reboot.</li>
	<li>Log in with the domain administrator password you set</li>
	<li>Open the Start menu and click on Administrative Tools &gt;&gt; DNS</li>
	<li>Expand your DC &gt;&gt; Forward Lookup Zones and click on the zone for your domain (e.g. vxprt.local)</li>
	<li>Verify that your server appears with an A record for the IP previously set.</li>
	<li>Right click on Reverse Lookup Zone and click on New Zone</li>
	<li>Click Next, accepting defaults until you get to the Reverse Lookup Zone Name page</li>
	<li>Type in the first 3 octets of the IP subnet you are using, so for example I would type in 192.168.6, this will help generate the appropriate zone name, click Next two more times and click Finish.  You now have a reverse lookup zone so hosts can be resolved by name and IP address.</li>
	<li>Go back to your forward lookup zone for your domain and double click on the A record for your DC</li>
	<li>Check the Update associate pointer PTR record and click ok; this will create a record in the reverse zone you just created</li>
	<li>The last step is to set a DNS forwarder since this server will server as the primary DNS server for all other servers.</li>
	<li>Right click on your server, just under DNS and select properties</li>
	<li>Click on the Forwarders tab and click the Edit... button</li>
	<li>Remove any local addresses from the list by highlighting it and selecting delete</li>
	<li>I will use the public Google DNS servers, but you could also use something like OpenDNS.</li>
	<li>Click where it says "Click here to add and IP address..." and enter 8.8.8.8 and 8.8.4.4 - those should resolve to google-public-dns-a and b; click OK and OK again, then close DNS Manager</li>
	<li>Open IE and verify you can get to the intenret, you should be all set!</li>
</ul>
So far we have setup our Windows template VM, created a Linked Clone and made it into a Domain Controller and NTP server, next we can get into setting up our virtual ESXi hosts.