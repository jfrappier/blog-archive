---
layout: post
author: Jonathan Frappier
title: Cisco WLAN Controller not passing traffic - resolved - but not sure why
permalink: /:title/
modified: 2013-01-08
category: articles
tags: [cisco, wlan, wireless, controller]
share: true
---
I ran into a strange problem recently, a Cisco WLAN controller 5508 with 1142N APs (not sure the model and controller matter entirely as I found the fix on a support forum thread for a 4000 series) would allow clients to connect, get an IP address but NOT pass any traffic other than ICMP.  I thought maybe the problem was Windows firewall related but disabled it still appeared.  I thought maybe a driver problem but tried several revs of the driver, and it also happened with different model cards.  A temporary work around was to disable, then re-enable the wireless card.

DHCP is handled by a Windows 2008 server, not the access points or WLAN manager, and again - the client was actually DHCPing an address (as I type that I wonder if there is a problem with the DHCP server now, but it didn't happen to wired clients or on a temporary access point we brought in...).  There was a thought it was a DHCP problem since ping worked, but I could not access network resources via IP which ruled DNS out.  Yet another test was to isolate the WLAN controller and APs on to a separate switch.  This also eliminated what appears to be a known problem with 2960 switches where APs cannot register with the controller (which wasn't our problem but worth isolating anyways).  I also removed all but 1 of the APs, but the problem persisted.

Now had I listened to my own Troubleshooting 101 post, I would have opened a support ticket, but this particular company let the support lapse and did not want to renew it.  This also meant I did not have access to download the latest software for the controller or APs.  So for those wondering, thats why there was no all into Cisco TAC on this issue.

What lead me to the fix that ultimately "fixed" the problem was an error I found in the logs "APF−1−REGISTER_IPADD_ON_MSCB_FAILED: Could not RegisterIP Add on MSCB. MSCB still in init state."  Now I am happy this is fixed, but I am not happy with what the "fix" was yet because I haven't found good documentation that explains why this fixed our problem.  I had to enable DHCP Addr. Assignment in the advanced section of the WLAN config, according to the documentation from Cisco:

<em>DHCP Addr. Assignment Required setting, which disallows client static IP addresses. If DHCP Addr. Assignment Required is selected, clients must obtain an IP address via DHCP. Any client with a static IP address is not be allowed on the network. The controller monitors DHCP traffic because it acts as a DHCP proxy for the clients.</em>

<em></em>Thats good and all, but my clients WERE DHCPing  addresses just fine and APs were broadcasting SSIDs just fine.  Oh and by the way this was all working swell through October, for several months actually, and just started to have problems in November.  If anyone has a better description/document that more deeply defines the DHCP Addr. Assignement Required option I would love to read it.