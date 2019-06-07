---
layout: post
author: Jonathan Frappier
title: Configure rsyslog in vCAC vRA appliance to send to a syslog server
permalink: /:title/
modified: 2014-09-25
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Deploy the vRealize Automation Appliance - vRealize Automation Series Part 2", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
One option not available in the vCloud Automation Center (vCAC) appliance VAMI is the ability to send logs to a syslog server, such as Splunk or LogInsight (does anyone know if this has been exposed in vRA 6.1?), thankfully since the appliance is built on linux, its just a matter of configuring rsyslog.  If you are using LogInsight you can use the <a href="https://solutionexchange.vmware.com/store/products/vcac-log-insight-content-pack#.VCQi0_ldXYE" target="_blank">LogInsight Content Pack</a>.  While I have LogInsight, I want to do this manually to send logs as if I were using a generic syslog server.  As you can see here, none of my vCAC logs are here (my appliance is named vcacapp)

<img src="/images/fulls/vcacapp-loginsight-search.jpg" class="fit image">

Here is how to configure the vCAC appliance to send logs:
<ul>
	<li>SSH to your vCAC / vRA appliance</li>
	<li>Type vi /etc/rsyslog.conf</li>
	<li>At the end of the file enter</li>
</ul>
<pre>*.*    loginsight.fqdn.tld:port</pre>
<ul>
	<li>For example loginsight.test.lab:514</li>
	<li>Save the file and restart syslog by typing</li>
</ul>
<pre>service syslog restart</pre>
Now if you go back to LogInsight or whatever syslog server you are using you can see logs being collected.  Logging, like monitoring involves a bit of science instead of just dumping logs into your syslog server.  If you have LogInsight, check out their content packs as that would be the preferred option in my opinion.

<img src="/images/fulls/vcacapp-loginsight-search-setup.jpg" class="fit image">