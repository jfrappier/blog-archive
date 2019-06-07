---
layout: post
author: Jonathan Frappier
title: Setting Up the Synology DS1513+
permalink: /:title/
modified: 2014-11-14
category: vra
tags: [synology, setup]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
In order to provide shared storage to my home lab, I am going to use a Synology DS1513+.  In my lab I have my DS1513+ connected to a switch, which is connected to my home router, this allows me to use http://find.synology.com to start configuring my DS1513+.

<img src="/images/fulls/get-started.png" class="fit image">

My Synolog is configured with 2x 120GB SSD Corsair Neutron drives and 3x 2TB Seagate SATA drives.  On the https://find.synology.com page, click on the Connect button to get started.
<ul>
	<li>Log in as admin with no password</li>
	<li>Click on the Main Menu button in the upper left corner and start Control Panel</li>
	<li>The Synology used DHCP to find an address on your network so we could connect and set it up.  We do not want DHCP to continue providing the address, especially since we will be using this for ESXi host storage (at least I will)</li>
	<li>In Control Panel click on Network &gt;&gt; Network Interface, selected the connected port and click the Edit button</li>
	<li>With the networking configuration done, time to start configuring storage!</li>
</ul>
My Corsair drives do not seem to be compatible with Synology SSD cache, I don't have the option to create it even though I should have enough memory for at least a portion of the SSDs to be used as cache.  In any case, give what I had for parts I'll just use the 2x SSDs as an all flash volume for my hosts and the 3x SATA drives as another.
<ul>
	<li>Chose manual configuration, enter an IP address outside the scope of your DHCP server (or home router) and click the OK button</li>
	<li>Click on the Main Menu button in the upper left corner and start Storage Manager</li>
	<li>When storage manager opens click on volumes (depending on your SSDs you could poke around and see if you can do SSD cache or not)</li>
</ul>
If your Synology ships with drives already, it likely had a volume created which is now unavailable because you removed two of the drives.  In that scenario remove any existing volumes.  If it was ordered with no drives, then I believe as older models did for me you can just create the new volumes and do not need to delete anything.

<img src="/images/fulls/synology-storage-manager.png" class="fit image">
<ul>
	<li>Click on the Volume menu and then click the create button</li>
	<li>For general purpose use I put my trust in Synology SHR volumes, in my case here I want a bit more control and am not so concerned over data loss since its just a lab.  I am going to chose Custom in the wizard to select my own RAID type</li>
	<li>Chose either single or multiple volume on RAID (I've selected single)</li>
	<li>Select the 3x 2TB drives, click OK when prompted about erasing the disk</li>
	<li>On the RAID selection screen, chose the RAID type you are most comfortable with given what you are running...for me - RAID0 across all 3 drives</li>
	<li>In most cases chose yes to check the disks, these shipped with the Synology and are new so I've selected No here for times sake</li>
	<li>Click Apply - your volume will be created</li>
	<li>If like me you still have drives in your Synology to use, repeat for the remaining drives.  Once the volume is created for the SSD, click on the SSD Trim button to enable.</li>
</ul>
And there you have it, Synology volumes are created.  Up next, iSCSi or NFS? (Hint I passed the Chris Wahl NFS Ninja training at the Boston VMUG)