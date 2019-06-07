---
layout: post
author: Jonathan Frappier
title: Monitoring the vCenter Server Appliance with Nagios
permalink: /:title/
modified: 2017-04-14
category: vra
tags: [vmware, vcsa]
share: true
related: [
    "vRealize CloudClient Export and Import Blueprints",
	"So you want to be a full stack engineer? Advice on mapping out your career." 
]
---
Wait, what? Why are you even doing that? Pretty sure that is just what went through Emad Younis' and Adam Eckerle's heads. Yes, this is an odd one, but I have a pretty specific use case in the lab environments I build for a "single pane of glass" view into the health of the lab environment. Why not use vRealize Operations? Great question - again in my narrow use case it isn't setup yet, and many components may not be setup yet BUT I still need to monitor things like services, NTP, and accessibility of services. So that is why, it's my story and I am sticking to it.

One more thing before we get going; I am just assuming this is majorly unsupported by VMware but if y'all want to make it supported I will be happy to help iron out the details.

So - what am I trying to accomplish: I need to monitor several linux based virtual machines for service availability and NTP offset, preferably without a client. My theory was I could use Nagios, and the check_by_ssh plugin to fill this need. Again most of these steps are likely not supported by VMware, so I wouldn't suggest doing this outside of a test/dev type environment. This post also assumes you have Nagios or Nagios XI up and running, so I won't be covering much of that here.

What we need on the VCSA is a user called Nagios, a key to allow the nagios use to SSH to the VCSA, the scripts (check files) you want to invoke via SSH.

1. SSH to your vCenter Server Appliance, and log in as root (presumably you haven't created any other users.
2. Type the following command.
<code>shell.set --enabled true</code>
This will allow shell access.
3. Type shell to switch to the shell
4. Create a user called nagios
<code>
useradd nagios
passwd somepassword
mkhomedir_helper nagios
su nagios
</code>
5. At this point you will be back at the appliance shell
6. Type the following command.
<code>shell.set --enabled true</code>
To allow shell for the Nagios user
7. Type shell to switch to the shell
8. When nagios attempts to SSH to the VCSA, it will expect to land in a normal linux shell
9. Type
<code>passwd -s</code>
You should be prompted for your password, and then the login shell you wish to use by default. Accept the default of /bin/bash

Finally you will need to copy the scripts to some directory on the VCSA in order for them to be run. I made a folder to match where Nagios keeps them by default, so /usr/local/nagios/libexec. Make sure the directories and files have r-x so the nagios user can execute them. You can test running them on the VCSA; if you can run them as nagios you should be all set.

You now have a user called nagios, with /bin/bash as the default shell, and the check files on the VCSA. The remainder of the setup is done on your Nagios server following [these directions](https://assets.nagios.com/downloads/nagiosxi/docs/Monitoring_Hosts_Using_SSH.pdf).

The only piece you need to add to your Nagios check is the -E argument; for example 
<code>$USER1$/check_by_ssh -H $HOSTADDRESS$ $ARG1$ $ARG2$ </code>
where ARG1 is "/usr/local/nagios/libexec/check_ntp_time -H 192.168.1.10 -c .20 -w .50"
and ARG2 is -E

Now I can monitor things like disk space, NTP offset (this one was critical for me) and processes.
<img src="/images/fulls/nagios-vcsa.png" class="fit image">
