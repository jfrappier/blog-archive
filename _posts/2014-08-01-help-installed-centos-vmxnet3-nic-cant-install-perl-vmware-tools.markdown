---
layout: post
author: Jonathan Frappier
title: Help - I installed CentOS with a VMXNET3 NIC and cant install Perl or VMware Tools
permalink: /:title/
modified: 2014-08-01
category: articles
tags: [vcsa, vmware, vsphere, vmxnet3, linux, vmware tools]
share: true
---
<img src="/images/fulls/tux-egg.jpeg" class="fit image">

I often (always?) prefer installing linux from the minimal CD's, nothing I need from the CD that I cannot get from yum or apt-get.  However what happens if you want to use the VMXNET3 driver with the minimal install?  In order to get the VMXNET3 drivers installed, you need VMware Tools.  In order to install VMware Tools you need Perl.  In order to get Perl you need internet access!  What's an admin to do? (Update, as Timothy Patterson pointed out this may be fixed in CentOS 7 - this was written on CentOS 6.2 and believe Perl is not included through at least 6.5)

Quite simple actually, add a 2nd NIC using the E1000 driver.  The E1000 driver will be recognized nativley by the OS without VMware tools.  Now you can configure this adapter (either DHCP or Staic by editing /etc/sysconfig/network-scripts/ifcfg-eth0 (since your VMXNET3 NIC won't be recongnized).  After a fresh install, you will need to at the very least activate the card.  Run:

vi /etc/sysconfig/network-scripts/ifcfg-eth0

Set

ONBOOT="yes"

Type :wq and press the enter key (this saves the file).  Now type

service network restart

Now if you run ifconfig you will see eth0 in addition to lo.  If you have DHCP you should also have an IP address with all the information you need.  If you need to provide a static address, you'll need a bit more info in the ifcfg-eth0 file.  Here is what mine looks like

<img src="/images/fulls/ifconfig.png" class="fit image">

Now you should be able to ping network resources.  Now run

yum install perl

You will be prompted to confirm the installation of several packages, twice (press y).  After a few minutes you should receive a Complete! message.

<img src="/images/fulls/perl-complete.png" class="fit image">

You can now start the VMware tools install by running

./vmware-install.pl

from the directory you extracted VMware tools - <a href="http://kb.vmware.com/kb/1018414" target="_blank">http://kb.vmware.com/kb/1018414</a>

<img src="/images/fulls/vmwaretools.png" class="fit image">

Reboot and you should see the VMXNET3 driver NIC listed in /etc/udev/rules.d/70-persistent-net.rules

<img src="/images/fulls/persistent70.png" class="fit image">

You can now make note of the MAC, in my case 00:50:56:bd:25:93, comment out the first SUBSYSTEM line, change the NAME="eth1" to "eth0"

Edit/etc/sysconfig/network-scripts/ifcfg-eth0 and change the HWADDR , disconnect the E1000 NIC in the vSphere client (web or otherwise) and reboot the VM.  CentOS seems to get a bit cranky about shutting down the interface when the MACs get changed (in theory you could stop networking all together before editing I guess - not tested).

When the VM reboots, you will have only your VMXNET3 adapater connected and have internet access.  Clone away!