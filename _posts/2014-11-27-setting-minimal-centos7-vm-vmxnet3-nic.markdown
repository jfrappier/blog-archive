---
layout: post
author: Jonathan Frappier
title: Setting up a minimal CentOS7 VM with VMXNET3 NIC
permalink: /:title/
modified: 2014-11-27
category: articles
tags: [vmware, vm, centos7, vmxnet3]
share: true
---
Now that CentOS7 is out, time to make sure I can setup my virtual machines with the VMXNET3 vmnic.  <a title="Help – I installed CentOS with a VMXNET3 NIC and can’t install Perl or VMware Tools!" href="/help-installed-centos-vmxnet3-nic-cant-install-perl-vmware-tools/" target="_blank">As I documented in my previous post, CentOS 6.x using the VMXNET3</a> driver requires VMware Tools, VMware Tools needs Perl, Perl is not included in the minimal ISO so I need network access to get Perl to install VMware Tools to get network access.  That order of operations doesn't work very well.

Also, as of CentOS7, <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2073803" target="_blank">VMware now recommends Open Virtual Machine Tools</a> so you would not be installing <a title="Help – I installed CentOS with a VMXNET3 NIC and can’t install Perl or VMware Tools!" href="/help-installed-centos-vmxnet3-nic-cant-install-perl-vmware-tools/" target="_blank">VMware Tools as I pointed out in my CentOS 6.x post on VMXNET3.</a>  Good news, though, VMXNET3 drivers in CentOS7 do not need VMware Tools but there are seemingly some new steps to get networking working.  So, lets get started; now obviously we don't want to install the OS every single time you need it, so my assumption here is that the use case if for your initial template build.  With that assumption out of the way I am going to create a new virtual machine in the vSphere Web Client with the following settings:
<ul>
	<li>VM hardware version 10 (since now we can edit them in the C# client)</li>
	<li>Guest OS Family - Linux</li>
	<li>Guest OS Version - CentOS 4/5/6/7 (64-bit)</li>
	<li>Virtual Hardware
<ul>
	<li>1 vCPU</li>
	<li>1GB memory</li>
	<li>New hard disk - thin provisioned</li>
	<li>1x vmnic - VMXNET3 connected</li>
</ul>
</li>
</ul>
Once the new machine is created, power it on and connect to the console, install the OS as you normally would - notice when you get to the Installation Summary screen the network says Not Connected; click on it and you'll see that i seems to recognize the VMXNET3 controller.  I am not going to set this adapter to "ON" right now, I am going to leave it "OFF" to show you how to bring it up on the command line.  Finish the and reboot once completed.  Log in and run ifconfig...egads command not found?  What is Linux going all Microsoft on us and changing things for the sake of changing it!!  Well if you tried to do a yum or ping anything right now, you'd not have network access as you might expect. So where to go from here?

Well it appears there is no more /etc/udev/rules.d/70-persistent-net.rules file any more, so lets have a look at /etc/sysconfig/network-scripts.  Hmm where is my ifcfg-eth0 file?

<img src="/images/fulls/centos7-ifcfg-file.png" class="fit image">

That has been replaced now, notice the ifcfg-eno16777984 file, that is what we want (though not sure where the numbering comes from) - open it in vi so we can have a look.  Yup looks just like the old ifcfg-eth0 file, lets get to work.  Change BOOTPROTO from dhcp to none and add the following with valid information for your network; IPADDR, NETMASK, GATEWAY, DNS1.  Here is what my file looks like now:

<img src="/images/fulls/centos7-ifcfg-static.png" class="fit image">

Now that you are all set, [esc] :wq [enter] to save it and service network restart - now ping 8.8.8.8...wait what - I STILL can't ping?  What is wrong?  Apparently in CentOS7 restarting the network service is not enough, we need to bring the actual interface up.  If you do an ls in /etc/sysconfig/network-scripts you'll notice the ifup command - always been there, I never used it before but this is what you'll use to bring up your interface, something like
<pre>ifup ifcfg-eno16777984</pre>
Now, here you can see our network is up

<img src="/images/fulls/centos7-ping-vmxnet3.png" class="fit image">

But... this will not be persistent over network service or virtual machine restarts so you'll need also edit the ifcfg-eno######## file and change ONBOOT to yes, now you can restart networking or your virtual machine and maintain networking

More information about the changes can be found in the <a href="http://wiki.centos.org/FAQ/CentOS7" target="_blank">CentOS7 FAQ</a>.