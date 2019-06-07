---
layout: post
author: Jonathan Frappier
title: Back to basics - Configuring the ESXi management interface via DCUI
permalink: /:title/
modified: 2015-02-04
category: articles
tags: [vmware, vsphere, vcenter, home lab, basics]
share: true
---
[display-posts category="back to basics" display-posts posts_per_page="15"]

Configuring the ESXi management interface via the Direct Console User Interface (DCUI) is the first step, post installation, needed to make your ESXi host accessible (unless of course it obtains an address via DHCP). Once the management interface is setup and working you can then log into the server from the host based client or use tools such as PowerCLI to manage and configure the host.  In fact, since the basic features of ESXi are free, you could start virtualizing with just 1 host and the management interface configured.

<img src="/images/fulls/virtualize-all-the-things.png" class="fit image">

Once the install is complete and you have restarted your server, you will be at the DCUI.
<ul>
	<li>Press the F2 button on your keyboard and enter the password you set during installation (see, no copy and paste here which is why I start with something easy).</li>
</ul>
<img src="/images/fulls/esxi-6-dcui.png" class="fit image">
<ul>
	<li>Use the arrow key to “Configure Management Network” and press enter</li>
	<li>Select “Network Adapters” and hit enter; here you can choose which network interfaces you want configured as a vmkernel adapter to support management traffic.  In this example only have 1 NIC but you may  have many in a production environment (you can very easily run  small workload traffic over 1 interface, but you won’t have redundancy)</li>
</ul>
<img src="/images/fulls/esxi-6-select-network-adapters.png" class="fit image">
<ul>
	<li>Depending on your network configuration, you may need to go into “VLAN (optional)” to set your VLAN ID for this interface if you require all traffic to be tagged (for example in a UCS environment).  For simple configurations, this step is probably not needed.</li>
	<li>Next, use the arrow keys to go to “IP Configuration” and press enter.  You can set either a dynamic or static IP address.  If you go the dynamic route via a DHCP server I’d suggest using a reservation so your IP address is consistent.  Using DHCP here also adds some considerations for availability.  For example if your DHCP server is not available, your host won’t get an IP address.</li>
	<li>If you use IPv6, select “IPv6 Configuration” and configure as needed.</li>
	<li>Now, go to “DNS Configuration” and press enter.  Here you define your DNS servers as well as your host name.  If you opted for DHCP, these will be provided otherwise enter these as appropriate and make sure you host name matches the DNS record you created.</li>
	<li>Once finished, hit the ESC key.  Here you will be prompted to restart the management network (unless you didn’t make any changes, which if you are using DHCP is certainly possible)</li>
</ul>
<img src="/images/fulls/esxi-6-restart-management-network.png" class="fit image">

If you have not already done so, create the appropriate DNS records for your host(s). You should now be able to access the Welcome page and download the Windows client. You could also connect to the host using <a title="PowerCLI Hacks" href="http://www.virtxpert.com/powercli-hacks/">PowerCLI</a> at this point.

<img src="/images/fulls/esxi-6-download-client.png" class="fit image">

The basics are now in place for you to start creating virtual machines!