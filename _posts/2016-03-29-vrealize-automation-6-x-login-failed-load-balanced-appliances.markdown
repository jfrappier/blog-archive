---
layout: post
author: Jonathan Frappier
title: vRealize Automation 6.x Login Failed with Load Balanced Appliances
permalink: /:title/
modified: 2016-03-29
category: articles
tags: [vmware, vsphere, vcenter, vra, vcac, load balanced]
share: true
---
Last week the team I work on ran into a problem with an installation of vRealize Automation 6.2, this installation has 2 appliances behind an NSX load balancer. After an ESXi host failure, one of the appliances did not come back online – when we tried to login we received the generic “Login failed. Please contact your System Administrator and report error code” completely-bs-string-that-means-nothing. Generally when we have seen this it was related to NTP, with the appliances and domain controllers being out of sync, however that wasn’t the case here.

<img src="/images/fulls/vra-error-login-failed.png" class="fit image">

We were unable to log into the default tenant as administrator@vsphere.local, or the additional tenant as any user who should have had access. Looking at the management interface of the appliance, there were several services that would not start, and an error in vRA Settings >> Messaging.

<img src="/images/fulls/vra-rabbitmq-error.png" class="fit image">

The Rabbitmq process was in a “NOT Connected” state and an error “Timeout contacting cluster nodes” for vRA02. When vRA02 was brought back online we were still unable to login with the same generic error.

The fix, in this case was to reset Rabbitmq on vRA01:
<ul>
Log into the 1st vRA appliance management interface
Navigate to vRA Settings >> Messaging and click Reset Rabbitmq
Reset RabbitMQ on vRA02, then rejoin it the cluster
</ul>

Log into the 2nd vRA appliance management interface
<ul>
Navigate to vRA Settings >> Messaging and click Reset Rabbitmq
Navigate to vRA Settings >> Cluster
Enter the 1st vRA appliance as the leading cluster node, the root password, and click join cluster
</ul>

<img src="/images/fulls/joined-to-cluster.png" class="fit image">

You should now be able to log into vRA