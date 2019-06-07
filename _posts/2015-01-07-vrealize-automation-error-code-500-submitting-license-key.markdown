---
layout: post
author: Jonathan Frappier
title: vRealize Automation Error Code 500 when submitting license key
permalink: /:title/
modified: 2015-01-07
category: articles
tags: [vmware, vsphere, vcenter, vra, vcac]
share: true
---
Chalk this up in the "useful error messages" column. When you attempt to enter a license key in the vRealize Automation appliance you receive "Error code: 500."

<img src="/images/fulls/error500.png" class="fit image">

Now when I saw this I immediately thought "internal server error," however in the case of vRA it may simply be an expired or invalid license key. Before extensive troubleshooting validate that your license key is correct, and it has not expired.

<img src="/images/fulls/license-success.png" class="fit image">