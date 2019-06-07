---
layout: post
author: Jonathan Frappier
title: System logs on host are stored on non-persistent storage
permalink: /:title/
modified: 2014-04-24
category: articles
tags: [esxi, vmware, vsphere, vcenter, logs, system logs, persistent]
share: true
---
<img src="/images/fulls/system_logs_on_host_non-persistent_storage.png" class="fit image">
I've seen this come up in more and more situations lately, especially as people move towards SD/USB media for ESXi installation so I thought I'd write up quick how-to for changing it.  The error you receive is System logs on host are stored on non-persistent storage.  This happens when there is no VMFS partition available during installation which causes log files to be written to RAM disk.  When you log into vCenter, you would see a warning on your host, and the above message on the summary tab.

Depending on your host configuration, you can chose to create a folder on a local datastore to write logs to, or perhaps another shared VMFS volume where all servers can write logs.  My preference is regardless of this choice is to send all server logs to some sort of central syslog server.

To fix the error, do the following steps:
<ul>
	<li>Log into the vSphere web client</li>
	<li>Click on Hosts and Clusters and expand your datacenter/clusters until you see the host you want to change</li>
	<li>Click on the Manage tab, then Settings, then Advanced System Settings</li>
	<li>Scroll down until you find Syslog.global.logDir</li>
	<li>Click on Syslog.global.logDir and click the pencil icon to edit the value</li>
	<li>Change this value to the location of where you want to store the logs.  For example:  [esxihostname-local]/esxilogs where esxihostgname-local is the name of the datastore and exilogs is the name of the folder.
<ul>
	<li>What is in brackets should match the name you see in the Storage view.  For example if the datastore name was homersimpson and a folder name bart it would look like [homersimpson]/bart</li>
</ul>
</li>
</ul>
You can read up on this further in the <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2032823" target="_blank">VMware KB 2032823</a> and more information on configuring syslog <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2003322" target="_blank">here</a>.

&nbsp;