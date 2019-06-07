---
layout: post
author: Jonathan Frappier
title: ViPR SRM Solution Packs for vCenter and XtremIO
permalink: /:title/
modified: 2015-04-16
category: articles
tags: [emc, xtremio, vipr, vipr srm]
share: true
---
*<em>*Disclaimer: I am an EMC employee, this post was not sponsored or in any way required by my employer, it is my experience getting to know this particular product.**</em>

In my last two posts I touched on what ViPR SRM can do, and the quick installation.

[display-posts category="ViPR SRM Series" display-posts posts_per_page="15"]

With the ViPR SRM installation out of the way, it's time to start adding Solution Packs. Solution Packs are use to connect to various systems, such as VMware vCenter, so ViPR SRM can collection information about virtual machines, ESXi hosts, datastores, and HBA's. Additionally, you connect ViPR SRM to your switches and storage for, quite literally, an end to end view of your environment.
<ul>
	<li>First, log into <strong>http://&lt;vipr srm IP or DNS&gt;:58080/APG</strong> and click on Administration (upper right corner)</li>
	<li>Once you are in the Administration interface, click on Centralized Management on the left navigation menu, a new window or tab will open</li>
	<li>In the new window, click on Solution Pack Center (back in the upper right corner)</li>
</ul>
<img src="/images/fulls/vipr-srm-solution-packs.jpg" class="fit image">
<ul>
	<li>In the search box in the upper right corner, type vCenter to filer the results, and click on VMware vCenter</li>
	<li>When the vCenter box opens, click on the install button.</li>
</ul>
<img src="/images/fulls/virp-srm-vcenter-install-pack.jpg" class="fit image">
<ul>
	<li>Follow the wizard and review the options; it's a basic wizard - next, next; if using PowerPath click Enable the Host PowerPath alerts for example and click next, next, next, next, and finally install. ViPR SRM will go through and install the selected components.</li>
</ul>
<img src="/images/fulls/vipr-srm-solutions-pack-vcenter-installed.jpg" class="fit image">
<ul>
	<li>Click OK. Repeat the above steps for your environment. At the very least, the Storage Compliance pack is useful. Here is the EMC XtremIO solution pack which I will be installing to show examples from.</li>
</ul>
<img src="/images/fulls/vipr-srm-solution-pack-xtremio.jpg" class="fit image">
<ul>
	<li>With the solution packs installed, we need to provide each some information. Expand Discovery Center in the left navigation menu, expand Devices Management and click on VMware vCenter</li>
	<li>Click on the Add new device... button and fill in the information to connect to vCenter. I suggest using dedicated accounts for external services, so for example here is my app_viprsrm user account which has admin privileges in vCenter. Click the test button to confirm the account has access, and then click OK. Repeat for multiple vCenters or the storage in your environment you added a pack for.</li>
</ul>
<a href="/images/fulls/info.png"><img class="alignleft  wp-image-3680" src="/images/fulls/info.png" alt="info" width="48" height="48" /></a>

Don't forget to click the Save button!

<img src="/images/fulls/save.png" class="fit image">

<img src="/images/fulls/vcenter-vipr-srm-creds.jpg" class="fit image">

Depending on your environment, you may also want to add your FC switches as well. Switch monitoring is done by adding a Solution pack for your switch, and connecting to it via SNMP. While logged in as admin go to http://&lt;viprsrm IP or DNS&gt;:58080/device-discovery, click Collectors, click New Collector, and Save. This will add an SNMP collector to the local VM. Once the collector is added click on Devices, New Device, and fill in the appropriate information.

<img src="/images/fulls/vipr-srm-snp-device-discovery.png" class="fit image">

With all switches added, click the check box next to it, and click the magnifying glass icon under actions; this will discover the switch.

ViPR SRM will now start collecting data, to expidite the process click on Scheduled Tasks (left navigation menu), check box for the "import-properties-default" task, and click the Run Now button. If you return to the User Interface (back in the Administration page, click User Interface) and go to Explore &gt;&gt; Hosts you should see your vCenter hosts as well as virtual machines.

<img src="/images/fulls/vipr-srm-vcenter-hosts.png" class="fit image">

If you navigate to Explore &gt;&gt; Storage you should also see the storage devices you added.

<img src="/images/fulls/vipr-srm-storage.png" class="fit image">

With the configuration out of the way, I can now start to explore my environment with the various reports available, which I will do in the next post!