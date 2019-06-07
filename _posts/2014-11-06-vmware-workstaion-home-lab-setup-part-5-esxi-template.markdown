---
layout: post
author: Jonathan Frappier
title: VMware Workstation Home Lab Setup Part 5 – ESXi Template
permalink: /:title/
modified: 2014-11-06
category: articles
tags: [vmware, home lab, esxi, nested esxi]
share: true
---
With the Windows template set and our first VM working, its time to make an ESXi template we will use in the home lab.  I mean it is small - I typically install with only a 1GB OS drive but why not use the features in VMware Workstation, so I am going to setup a clone.  Setting up the ESXi VM is pretty much the same as what we did in part 1, so I am not going to rehash that (hopefully you learned something in part 1) but I'll note some tips here none the less.

First, when creating the disk use only 1024MB, ESXi will install fine for our use it just won't have any place for logs but that is okay, we will fix that later.  Also, while VMware Workstation should take care of this when you select ESX as your VM type, check to make sure Virutalize Intel VT-x/EPT or AMD-V/RVI is enabled in the processor section in Virtual Machine Settings.   Finally, mount your ESXi ISO in your CD/DVD drive and power on the VM.

While it has been documented pretty well, I will walk through the ESXi install steps here for completeness.  Also, once you click into the console, you will lose control of your mouse since ESXi doesn't have VMware Tools installed, press CTL-ALT to return it to your computer
<ul>
	<li>Boot the VM with the ESXi CD mounted</li>
	<li>After a few moments the Welcome to the VMware ESXi 5.5.0 Installation screen will appear, press the enter button to continue</li>
	<li>Press F11 to accept the EULA (tip, if you are on a laptop or keyboard with extra features on the F keys, you may have to hold down the FN or function key</li>
	<li>The only drive available should be the 1GB drive create during the VM setup, ensure it is highlighted in yellow and press the enter button</li>
	<li>Select your keyboard layout and press Enter</li>
	<li>Type in the root password, arrow down to enter it again and press enter</li>
	<li>After  a few moments, the Confirm Install page will appear, press F11 to install</li>
	<li>Once the install completes, press Enter one last time to restart the server</li>
</ul>
Now that we have the base ESXi install done, it is time to install a couple of extra Flings into our ESXi template - <a href="https://labs.vmware.com/flings/vmware-tools-for-nested-esxi" target="_blank">VMware Tools for ESXi</a> and the <a href="https://labs.vmware.com/flings/esxi-mac-learning-dvfilter" target="_blank">ESXi Mac Learning dvFilter</a>.  In order to install these, we need to log into the console of our ESXi virtual machine in VMware Workstation; click into the console and press the F2 button to get started.
<ul>
	<li>Log in as root and the password you set previously</li>
	<li>Arrow down to Troubleshooting Options and press enter</li>
	<li>Arrow down to enable SSH and press enter; the SSH status in the right/gray side of the screen should change from Disabled to Enabled</li>
</ul>
<img src="/images/fulls/esxi-dcui-enable-ssh.png" class="fit image">
<ul>
	<li>Now press the ESX button until you return to the main screen with the IP address ESXi pulled from DHCP</li>
	<li>Open your favorite SSH client and connect to that IP address</li>
	<li>Log in as root</li>
	<li>I am assuming you have internet access here, so it is actually quite easy to install these components, copy and paste the commands below into you SSH session.  If you do not have internet access, you will first need to download the VIBs, upload them to the ESXi virtual machine and install from that location (just edit http path in the below commands and replace with your file system path</li>
</ul>
To install VMware Tools:
<pre>esxcli software vib install -v http://download3.vmware.com/software/vmw-tools/esxi_tools_for_guests/esx-tools-for-esxi-9.7.1-0.0.00000.i386.vib -f</pre>
To install Mac Learning dvFilter
<pre>esxcli software vib install -v http://download3.vmware.com/software/vmw-tools/esxi-mac-learning-dvfilter/vmware-esx-dvfilter-maclearn-1.0.vib -f</pre>
You should receive a message that the VIBs were installed like the image below

<img src="/images/fulls/vib-install.png" class="fit image">

Note the dvFilter is installed here for testing purposes, since my home lab is built on Windows and VMware Workstation, typically  you would install this on your physical ESXi hosts running ESXi virtual machines.  Now that we have done that we "could" power down the ESXi VM we just built and start using it as a template, however we would need to manually reset ESXi every time we cloned it (and if you are doing this in a FC storage based environment you will need to do that anyways) but why would we want to do that.  The following steps are courtesy of <a href="http://twitter.com/lamw" target="_blank">William Lam</a> and <a href="http://www.virtuallyghetto.com" target="_blank">virutallyghetto.com</a>, check out his site and the <a href="http://www.virtuallyghetto.com/2013/12/how-to-properly-clone-nested-esxi-vm.html" target="_blank">blog post for full details on resetting the virtual machine</a>.  Since we do not have any VMFS datastores, there are only two steps we need to do before cloning, then once the ESXi virtual machine is clone you will need to log in and set networking information before we join them to vCenter (to be installed shortly).
<ul>
	<li>From the SSH session, run the following from William's article:</li>
</ul>
<pre>esxcli system settings advanced set -o /Net/FollowHardwareMac -i 1</pre>
<ul>
	<li>Next, remove /system/uuid from esx.conf.  Open it in vi, arrow down until you find the /system/uuid line and press dd on your keyboard, then press esc : wq &lt;enter&gt;</li>
</ul>
<pre>vi /etc/vmware/esx.conf</pre>
<ul>
	<li>Log out of the SSH client</li>
	<li>Return to the ESXi DCUI and disable SSH</li>
	<li>Press ESC twice to return to the main DCUI page and press the F12 button, enter the root password and then press F2 to shut down the VM</li>
</ul>
Once the virtual machine powers off take a snapshot to use for future cloning.  You should have two VMs in your template folder, along with your running Windows linked clone which is current a domain controller.  You are now ready to start cloning your nested ESXi virtual machines.

As a side note, and not required, you may also want to install the simple web client, an open source(?) simple client so you can manage the ESXi host without the DCUI or vSphere Client.  You can <a href="https://github.com/weikinhuang/esxi-simple-web" target="_blank">find this project on GitHub</a>.