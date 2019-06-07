---
layout: post
author: Jonathan Frappier
title: Configuring CentOS to to send logs to Nagios Log Server
permalink: /:title/
modified: 2015-01-23
category: articles
tags: [nagios, syslog, nagios log server]
share: true
---
Now that Nagios Log Server is installed, it's time to get some log files in there. I got myself all fired up ready to comb through page after page of documentation to figure out how to set it up... then those nice folks over at Nagios did this...

<img src="/images/fulls/nagios-log-server-linux-setup.png" class="fit image">

That's right, if you click on Linux Source from the home screen, it gives you scripts to download and run to set it all up. They even pulled the IP address from the Nagios Log Server...it was like they wanted you to succeed in making this all work! It can't be that easy right? Let's try!

<img src="/images/fulls/linux-source-nagios-log-server-setup-linux.sh-script.png" class="fit image">

That was easy, no way there are actually logs showing up in Nagios Log Server though, right? Almost, SELinux was preventing log files from being shipped as you can see in the middle of the above screenshot. So...
<pre>cp /etc/selinux/config /etc/selinux/config.bak &amp;&amp; sed -i s/SELINUX=enabled/SELINUX=disabled/g /etc/selinux/config &amp;&amp; shutdown now -r</pre>
And BOOM goes the log file goodness after a reboot!

<img src="/images/fulls/remote-linux-source-nagios-log-server.png" class="fit image">

In probably less than 5 minutes, you can have a fully functional Nagios Log Server, based on ELK, deployed and receiving log files from a remote source - that is damn impressive. Of course in this example we haven't looked at which logs we are sending - maybe you only want specific log files being sent from Apache or Ansible for instance, but that is a finer art form that we can save for another blog post. Happy logging!