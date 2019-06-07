---
layout: post
author: Jonathan Frappier
title: Installing the EMC Virtual VNX (vVNX) Community Edition
permalink: /:title/
modified: 2015-07-08
category: articles
tags: [vmware, vsphere, vcenter, emc, vnx, vvnx]
share: true
---
*<em>*Disclaimer: I am an EMC employee, this post was not sponsored or in any way required by my employer, it is my experience getting to know this particular product.**</em>

The virtual VNX was announced at EMC World in May of 2015, and is a free to use, software only version of the VNXe. <a href="http://www.emc.com/products-solutions/trial-software-download/vvnx.htm" target="_blank">More details on the vVNX can be found at the ECN</a>.

A few requirements for the vVNX: you'll need to provide the virtual machine with 2 vCPU and 12GB of RAM (I'll test that as well) and you can present up to 4TB of storage. This is a great lab solution as well as a way to test upgrades or changes to your production VNX arrays (I know I never had budget to buy additional arrays for testing). Before you get started, <a href="http://www.emc.com/products-solutions/trial-software-download/vvnx.htm" target="_blank">download the vVNX bits</a> (http://www.emc.com/products-solutions/trial-software-download/vvnx.htm) which is provided as an OVA.

1.  Once the OVA has been downloaded, log into vCenter and start the Deploy OVF Template wizard. In the image below, you can see I am deploying to an existing vApp but you could do this to an individual ESXi host or cluster as well.

<img src="/images/fulls/1.deploy-ova.png" class="fit image">

2.  Once you select the OVA package, you'll be prompted to accept the extra configuration options; click the check box, click Next, and accept the EULA

<img src="/images/fulls/2.accept-config.png" class="fit image">

3.  In section 2 of the wizard, you will name your virtual VNX and select the appropriate resources such as storage, and configure networks.

4.  The network setup (2c) includes 3 interfaces; 1 for management and 2 for data. Select the appropriate port groups and click next. As you can see below I select two different port groups for this.

<img src="/images/fulls/3.network-setup.png" class="fit image">

5.  In step 2d you will set the system name, as well as expand the IPv4 section to provide the management IP information as pictured below, of course that is valid in your environment. If using DHCP, leave these values blank

<img src="/images/fulls/4-mgmt-network.png" class="fit image">

6. Click next, check the box to Power on after deployment and click Finish. After the vVNX deploys, it will boot and start its initial configuration which can take some time. You can watch the setup process via the console to know when it has completed, for me I started the OVA deployment at 1:25p and it was completed within about 20 minutes (I also wasn't paying very close attention)

7.  With the Virtual VNX powered on, add more virtual disks to the VM; this would mimic having physical drives in your array. The number of drives you add will depend on your use case. I've added 4x 250GB drives. Do not remove the 22, 30, or 32GB drives that were already created from the VM deployment.

8.  Navigate to the IP address you set during installation and log in as as admin with a default password of Password123# to start the Unisphere Configuration Wizard (note in may take several minutes after the initial setup for the login page to be available)

<img src="/images/fulls/5.unisphere-config-wiz.png" class="fit image">

9.  The first 2 steps are pretty straight forward; accept the EULA, and enter a new password (keep the service password box checked)

10.  When you get to the license screen, you will need to switch gears for a minute and navigate to https://www.emc.com/auth/elmeval.htm and enter the System UUID in the configuration wizard

<img src="/images/fulls/6.uuid_.jpg" class="fit image">

11. Download the license key file that is generated, and upload it in Unisphere using the Install License File button

<img src="/images/fulls/7.download.jpg" class="fit image">

12.  You should see a message that the License file installed successfully

<img src="/images/fulls/8.lic-file-installed.png" class="fit image">

13.  Next, select the appropriate DNS and NTP configuration for your network

14.  Create your initial storage pool, you can create others later as well. I have opted to create one called CloudStorage and added the 4x 250GB drives I added previously and assigned a tier.

<img src="/images/fulls/9.storage-pool.png" class="fit image">

15.  Next, configure your iSCSI interfaces, for example ETH0

<img src="/images/fulls/10.iscsi_.png" class="fit image">

16.  The remaining steps are optional. If you would like to use NFS, you can do so in step 9 and replication in step 10. Click Finish to complete the wizard.

<img src="/images/fulls/11.finish.png" class="fit image">

At this point, if you have not done so already you will need to configure a vmkernel port on your ESXi hosts on the same network as the storage interface(s). Once that is done, you should be able to go to Storage &gt;&gt; LUN and create a new LUN. For example here you can see the ESXi host I added to the vVNX while creating a LUN.

<img src="/images/fulls/12.add-lun.png" class="fit image">

In my next post, I will be seeing if I can add the vVNX to a ViPR Virtual Array...stay tuned.

<strong>*Update: It appears the vVNX ships with a version of SMI-S that is not currently supported by ViPR. While it will initially discover the vVNX it fails on subsequent discovers for both file and block**</strong>