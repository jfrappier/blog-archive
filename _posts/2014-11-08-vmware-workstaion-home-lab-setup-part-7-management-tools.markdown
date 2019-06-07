---
layout: post
author: Jonathan Frappier
title: VMware Workstation Home Lab Setup Part 7 – Management Tools
permalink: /:title/
modified: 2014-11-08
category: articles
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
---
So now we've got two ESXi hosts and our domain controller running in the home lab, it's almost time to setup vCenter however, in a real world scenario you would need a way to get vCenter onto the ESXi hosts (because of course you are virtualizing vCenter).  Up until now what we have done through the DCUI would have been at a keyboard and mouse or virtual KVM (such as Cisco UCS or HP iLO) and we cannot create virtual machines via the DCUI.  So, what tools are available to manage our ESXi hosts to start creating virtual machines?

First, typically most people would start with the Windows vSphere Client.  This client can connect directly to an ESXi host and start creating virtual machines, such as a Windows virtual machine for vCenter or <a title="Installing the vCenter Server Appliance 5.5.0b #VCSA" href="http://www.virtxpert.com/installing-vcenter-server-appliance-5-5-0b/">importing virtual appliances such as the vCenter Server Appliance (VCSA)</a> which we will use in our lab set later on.

You can download the Windows vSphere client from the ESXi getting started page by navigating the IP address of one of your ESXi hosts (like was done in the last post at https://192.168.6.11).  If you are running Windows 8.x you will need to <a href="http://www.microsoft.com/en-us/download/details.aspx?id=15468" target="_blank">download a J++ package</a> and install it before proceeding with the Windows vSphere Client, <a href="http://itechthereforeiam.com/2013/12/quick-post-trouble-installing-vsphere-on-windows-8-1/" target="_blank">thanks to my friend Matthew Brender for reminding me</a> of that gotcha which also means you will need to turn on the .NET Framework 3.5.  To do so, open the Start menu, go to Control Panel &gt;&gt; Programs and Features and click Turn Windows features on or off.  Tick the .Net Framework 3.5 box and click OK

<img src="/images/fulls/enable-dotnet35-windows81.png" class="fit image">

Once .NET 3.5 is enabled and J++ is installed, download the vSphere Client, run the installation wizard and log in to the IP address/host name of your ESXi host as root.

<img src="/images/fulls/vsphere-client.png" class="fit image">

Another option is to install <a href="http://blogs.vmware.com/PowerCLI/2014/03/new-release-vsphere-powercli-5-5-r2.html" target="_blank">PowerCLI</a>, which is based on PowerShell and <a href="http://www.slideshare.net/JonathanFrappier/providence-vmug-powercli?ref=https://www.linkedin.com/in/jonathanfrappier" target="_blank">very powerful with many options to manage both ESXi hosts, virtual machines</a> and more.  Again download and run through the PowerCLI installation wizard, it will install a few additional componenets as part of the install.  Once installed launch PowerShell as administrator (even if you are logged in as an administrator) and run
<pre>Set-ExecutionPolicy RemoteSigned</pre>
You should now be able to launch PowerCLI and run
<pre>Connect-VIServer 192.168.6.11</pre>
You will be prompted for a username and password and are then connected and can run cmdlets in PowerCLI to import or create new virtual machines.

<img src="/images/fulls/powershell-powercli.png" class="fit image">

Since we are using VMware Workstation, we can also use that to connect to the ESXi hosts (or a vCenter server later) as well.  In VMware Workstation, click on File &gt;&gt; Connect to server.  Enter the IP, username (root) and password to log in.  You can now create virtual machines on your virtual ESXi host running in VMware Workstation from VMware Workstation!

<img src="/images/fulls/esxi-vmware-workstation.png" class="fit image">

In the next post, we will use the vSphere Web Client and PowerCLI to import the vCenter Server Appliance for our home lab which will provide (obviously) vCenter, Single Sign-On (SSO) and the vSphere Web Client which is where we will do most of the work through this series.