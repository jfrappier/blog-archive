---
layout: post
author: Kanji Bates
title: Notes from a nested esxi 5 setup
permalink: /:title/
modified: 2012-10-08
category: articles
tags: [vmware, nested, esxi, nested esxi, dell]
share: true
---
<strong><em>Guest Post by Kanji B.</em></strong>

These are some notes from my nested lab setup on a Dell OptiPlex 790 from (4x i5-2400 @ 3.10GHz, w/16GBs, and it supports VT!), I hope that these can help others in the VMware community doing the same.

I ran into my first gotcha quite early thanks to Dell's love for bleeding edge NICs.  It's one of the few things that drives me nuts about Dell hardware, and I facepalmed as soon as ESXi threw up it's "No network adapters detected" error so off I went to research how - or even if! - I could inject drivers into the ESXi install, and fortunately stumbled on someone who had already done so on the OptiPlex 790:- <a href="http://bohemiangrove.co.uk/esxi-5-0-the-free-one/" target="_blank">http://bohemiangrove.co.uk/esxi-5-0-the-free-one/</a>

A short while later, I had a fresh ESXi install and began installing my nested ESXi, when I ran into the SAME problem! WTF?! The host ESXi had networking, so why wouldn't the guests?  Turns out that the default Adapter type for RHEL 6 (the Guest type which a few of the nesting guides suggest you base your ESXi guest on) is vmxnet3, and there's no vmxnet3 driver in ESXi 5.0 and installing VMware Tools to get it wasn't going to happen.

Poking around, I managed to fix it by using an E1000 adapter instead, and then noticed that virtuallyGhetto touched on this last month (<a href="http://www.virtuallyghetto.com/2012/09/nested-esxi-51-supports-vmxnet3-network.html" target="_blank">http://www.virtuallyghetto.com/2012/09/nested-esxi-51-supports-vmxnet3-network.html</a>) as they noticed that 5.1 fixes this very issue.  That solved, I took another stab at installing a nested ESXi, only to hit another showstopper when the installer didn't detect any local or remote drives to install on.  Poking around some more, I noticed that the SCSI Controller Type was set to VMware Paravirtual (not recommended for this guest OS), ugh, bitten by RHEL 6 defaults again...  For reference, if you set it to LSI Logic Parallel, ESXi sees the provisioned drive as local; or remote if set to LSI Logic SAS.

Ironically, if I had just gone with Windows 2008 R2 x64 (the default Guest type), I wouldn't have run into either issue, as VMware defaulted to a supported Adapter and SCSI Controller!