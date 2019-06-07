---
layout: post
author: Jonathan Frappier
title: PowerCLI Stop-VApp -Force
permalink: /:title/
modified: 2014-11-08
category: articles
tags: [vmware, vsphere, vcenter, powercli, stop-vapp]
share: true
---
So day to day I help maintain a bunch of vApps that run, but are also disposable.  When we are finished with them I just want them gone.  I am working on a PowerCLI script to help automate this process and saw the Stop-VApp cmdlet.  Comparing that to the Stop-VM cmdlet there was no -kill switch in Stop-VApp so I wasn't sure how to just "pull the plug" - after all I don't care about the vApp at this point.

Turns out the -Force switch will take care of "pulling the plug" and not gracefully shutting down each guest OS. The documentation was a bit misleading to me for that switch.  So, just a quick and dirty post - need to kill a vApp:
<pre>Stop-VApp -Force -vApp &lt;vAppName&gt;</pre>
Boom, it's powered off, no waiting for the guest OS!