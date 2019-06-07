---
layout: post
author: Jonathan Frappier
title: Home Lab - $1250 8-Core / 32GB / 750GB Flash / 2TB HDD 2015 Edition
permalink: /:title/
modified: 2015-05-26
category: articles
tags: [home lab, 8-core, amd, 2015]
share: true
---
<a href="/images/fulls/11-139-022-TS.jpg"><img class="alignleft  wp-image-3784" src="/images/fulls/11-139-022-TS.jpg" alt="11-139-022-TS" width="209" height="157" /></a>It was a bit over a year ago that I wrote about my 8-core home lab. I was asked if there were any updates to the build and I was curious to see how it stood up a year later. Happily for me, and anyone who has invested in this build, the same basic platform is still a solid option for your home lab. I have made a few tweaks below based on some new hardware being available. As I did last year, there was a focus on keeping cost down but having enough power to run a fully nested home lab.

With 32GB I have been able to run Windows 8.1 and VMware Workstation with 3x nested ESXi 5.5 hosts each with 8GB of RAM. One of those host runs the vCAC / vRA appliance, one runs the Application Services appliance, and the 3rd is used when provisioning virtual machines. In addition to the 3x nested hosts, I run a 5.5 VCSA at 4GB RAM, Windows 2012 R2 DC, Windows 2012 R2 vCAC / vRA IaaS with SQL Express on the same virtual machine, and CentOS 5.5 running Ansible in Workstation. With everything powered on I run at about 85% memory utilization and only push the CPU's <a href="https://www.youtube.com/watch?v=2IY1YRKb8Co&amp;list=PL2rC-8e38bUWVQyuBTvM30ud9QImcW1aO&amp;index=9" target="_blank">during provisioning processes</a>.

The hardware...

<strong><a href="http://www.newegg.com/Product/Product.aspx?item=N82E16819113285" target="_blank">CPU:  AMD FX8320</a></strong> - This is the exact same processor as last year. It is an 8-core AMD processor that fully supports nested ESXi and 64-bit virtual machines running on the nested ESXi hosts.

<strong><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16813157576" target="_blank">Motherboard: ASRock 990FX Extreme6</a></strong> - This is a new motherboard for 2015, versus the Asus I used last year (<a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16813131873&amp;nm_mc=AFC-C8Junction&amp;cm_mmc=AFC-C8Junction-_-na-_-na-_-na&amp;cm_sp=&amp;AID=10446076&amp;PID=6152376&amp;SID=ia5lml7kba00a5e400053" target="_blank">though that board is still available</a>). The reason for the change, the ASRock Extreme6 supports up to 64GB of memory where as the Asus only supported 32GB. Now, having said that this build still uses 32GB because the 16GB memory modules are $190 each, compared to 4x 8GB (32GB total) modules being $210 TOTAL. This board has onboard RAID and has 5 6Gbps SATA ports.

<a href="http://www.newegg.com/Product/Product.aspx?item=N82E16820231488" target="_blank"><strong>Memory: G.SKILL Ripjaw X Series</strong></a> - Similar memory to what was used last year, just not in a full kit so pick up 4 of these.

<a href="http://www.newegg.com/Product/Product.aspx?item=N82E16820148948" target="_blank"><strong>Flash: Crucial MX200</strong></a> - These were used instead of the Corsair Neutron drives I used last year, for no other reason than saving a few dollars to upgrade in other areas. The Neutron drives have been great for the last year, no problems to report so far. At $1250 you can pick up 3 of these if you like, or drop the price of your home lab.

<strong><a href="http://www.newegg.com/Product/Product.aspx?item=N82E16822178340" target="_blank">HDD: Seagate Hybrid 1TB</a></strong> - I again opted for the hybrid drives for bulk / lower tier storage. I run most of my lab off these drives, configured in a RAID0. I opted for 2 of these.

<a href="http://www.newegg.com/Product/Product.aspx?item=9SIA4U71UH9495" target="_blank"><strong>NIC: Intel Dual-port 82575</strong></a> - Because HCL, and wanted the possibility to install clean on baremetal. If you go the VMware Workstation route, you could skip this card potentially unless you would like more ports to get fancy with. You could again lower your cost here by going with a used card as I ended up doing last year like the <a href="http://www.amazon.com/HP-NC7170-network-adapter-383738-B21/dp/B0009MWAI4?tag=viglink127339-20" target="_blank">HP7170</a> from Amazon.

<a href="http://www.newegg.com/Product/Product.aspx?item=N82E16814127692" target="_blank"><strong>Video: MSI ...whatever</strong></a> - This is here because the motherboard doesn't have on-board video. Buy a card based on your needs, I went cheap here because i don't use the box for any sort of gaming. If you'll have other uses, obviously look at your requirements.

<a href="http://www.newegg.com/Product/Product.aspx?item=N82E16811139022" target="_blank"><strong>Case: Corsair Air 540</strong></a> - Case again is getting into personal preference area. The <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16811139027&amp;nm_mc=OTC-pr1c3grabb3r&amp;cm_mmc=OTC-pr1c3grabb3r-_-Electronics-_-Corsair-_-N82E16811139027&amp;scpid=7&amp;scid=scsho5459358" target="_blank">graphite 230T</a> I used last year is perfectly capable. The Air 540 has 4 internal 2.5" drive bays and 2 hot swap drive bays to support the 3x SSD and 2X HDD drives.

<a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16817182304" target="_blank"><strong>Power Supply: Rosewill RD600-M</strong></a> - This is the new version of the power supply used last year, which has been stable for me even through a faulty UPS.