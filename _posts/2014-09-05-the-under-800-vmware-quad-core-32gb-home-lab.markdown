---
layout: post
author: Jonathan Frappier
title: The Under $800 VMware Quad-Core 32GB Home Lab
permalink: /:title/
modified: 2014-09-05
category: lab
tags: [home lab, 32gb]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "VMware Workstation Home Lab Setup Part 1 - Windows VM", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
**Updated 4/20/2016** The under $800 home lab is now under $700!
Because I am a geek, and my needs change, and this is what I like to do I often check out new hardware, cost features etc.  One of the things I wish I did on my <a title="8-Core, 32GB RAM, 360GB Flash, 3TB, Dual-NIC Home Lab Part List" href="http://www.virtxpert.com/8-core-32gb-ram-360gb-flash-2tb-dual-nic-home-lab-part-list/">8-core lab</a> was go for a smaller form factor case.  Pricing, as always is subject to change.  I actually prefer to buy most of my hardware on Amazon because I seem to have an easier time returning items that are defective but I'll link to NewEgg.  You may be able to find a part or two for a bit less somewhere else which is always good.

Here is a run down on the parts for this VMware home lab build which should be capable of booting nested 64-bit VMs or as a stand alone ESXi host running other VMs since it is cheap (you can get almost 2 of these for the price of my 8-core build).  

<a href="http://goo.gl/jPkUMC" target="_blank">According to AMD</a>, all Vishera series processors have the VT/RVI feature to allow you to boot 64-bit VMs in a nested hypervisor (http://goo.gl/jPkUMC) I'll assume you will boot from USB, and that you have plenty of them from conferences past to keep my price under $800 :)


<li>Case: $40 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16811146075&ignorebbr=1" target="_blank">NZXT Source 210 S210-001 Black “Aluminum Brush / Plastic” ATX Mid Tower Computer Case</a></li>
<li>CPU: $100 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16819113286&ignorebbr=1" target="_blank">AMD FX-6300 Vishera 6-Core 3.5 GHz (4.1 GHz Turbo) Socket AM3+ 95W FD6300WMHKBOX Desktop Processor</a></li>
<li>MB: $75 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16813157364&ignorebbr=1" target="_blank">ASRock 970 PRO3 R2.0 AM3+ AMD 970 6 x SATA 6Gb/s USB 3.0 ATX AMD Motherboard with UEFI BIOS</a></li>
<li>RAM: $120 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16820231557&ignorebbr=1" target="_blank">G.SKILL Ares Series 32GB (4 x 8GB) 240-Pin DDR3 SDRAM DDR3 1333 (PC3 10666) Desktop Memory Model F3-1333C9Q-32GAO</a></li>
<li>PS: $50 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16817139027&ignorebbr=1" target="_blank">CORSAIR CX series CX500 500W ATX12V v2.3 80 PLUS BRONZE Certified Active PFC Power Supply</a></li>
<li>SSD: $90 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16820167333&ignorebbr=1" target="_blank">Intel 535 Series 2.5" 240GB SATA III MLC Internal Solid State Drive (SSD) SSDSC2BW240H6R5</a></li>
<li>HDD: $107 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16822236344&ignorebbr=1" target="_blank">WD Red 3TB NAS Hard Disk Drive - 5400 RPM Class SATA 6Gb/s 64MB Cache 3.5 Inch - WD30EFRX</a></li>
<li>NIC: $105 <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16833310504&ignorebbr=1" target="_blank">Refurbished:   HP NC364T 10/ 100/ 1000Mbps PCI-Express Network Adapter</a></li>

That is a grand total $687 US (as of 4/20/2016 pricing). Additionally there is currently $40 in mail in rebates ($20 each on the PSU and MB) so in the short term that is $647!

