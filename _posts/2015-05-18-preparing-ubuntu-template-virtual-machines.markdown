---
layout: post
author: Jonathan Frappier
title: Preparing Ubuntu template virtual machines
permalink: /:title/
modified: 2015-05-18
category: articles
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6]
share: true
---
Bob Plankers has a great post over at <a href="https://lonesysadmin.net/2013/03/26/preparing-linux-template-vms/" target="_blank">lonelysysadmin.net for preparing CentOS based virtual machines for being a template</a>. As I've started working with Ubuntu more I decided to take that list and Ubuntu-ize it (mostly from proding by <a href="http://twitter.com/szelechoski" target="_blank">Sarah Zelechoski</a> - one of the smartest people I've ever had the privilege to work with...so many thank you's). Anyways here is that guide... Ubuntu-ized.

<strong>Stop logging services (<a href="http://manpages.ubuntu.com/manpages/utopic/man8/auditd.8.html" target="_blank">auditd</a> and <a href="http://manpages.ubuntu.com/manpages/lucid/man8/rsyslogd.8.html" target="_blank">rsyslog</a>):</strong>
<pre>service auditd stop
service rsyslog stop</pre>
<strong><a href="https://help.ubuntu.com/community/Lubuntu/Documentation/RemoveOldKernels" target="_blank">Check for, and remove old kernels</a></strong>

Check your current kernel by running
<pre>uname -r</pre>
Then run
<pre>dpkg -l | grep linux-image-</pre>
If additional images are listed, remove them by running
<pre>apt-get autoremove linux-image-#.##.#</pre>
You can remove multiple images all on the same line just by listing them one after another.

<strong><a href="https://help.ubuntu.com/community/AptGet/Howto" target="_blank">Clean out apt-get</a></strong>
<pre>apt-get clean</pre>
<strong>Force the <a href="http://manpages.ubuntu.com/manpages/jaunty/man8/logrotate.8.html" target="_blank">logs to rotate</a> &amp; remove old logs we don’t need</strong>
<pre>logrotate –f /etc/logrotate.conf
find /var/log -name "*.gz" -type f -delete</pre>
<strong>Truncate the audit logs (and other logs we want to keep placeholders for)</strong>
<pre>cat /dev/null &gt; /var/log/audit/audit.log
cat /dev/null &gt; /var/log/wtmp
cat /dev/null &gt; /var/log/lastlog</pre>
<strong>Remove the udev persistent device rules</strong>

Well, saved a step here - there are rules which exclude creating files that match MAC addresses for VMware, Hyper-V, KVM, Xen, and virtualbox (see /lib/udev/rules.d/75-persistent-net-generator.rules). So long as your MAC matches this, nothing to clean up. Otherwise
<pre>rm -f /etc/udev/rules.d/70-persistent-net.rules</pre>
It will be recreated on the next boot, so any time you power on this VM (updates maybe?) you'll need to delete this file again so it is not saved in the template.

<strong>Remove the traces of the template MAC address and UUIDs.</strong>

Here is another step you shouldn't need to do, however you may want to check /etc/network/interfaces to verify

<strong>Clean /tmp out</strong>
<pre>rm -rf /tmp/*
rm -rf /var/tmp/*</pre>
<strong>Remove the SSH host keys</strong>
<pre>rm –rf /etc/ssh/*key*
rm –rf ~/.ssh/authorized_keys</pre>
<strong>Update network config</strong>

If you have set /etc/network/interfaces, make sure to reset for cloning purposes. For example as I wrote this it had a static IP address which I changed to DHCP before shutting down and converting to a template.

<b>Remove hostname</b>

If you have named your virtual machine anything other than localhost, and want the template to spin up with a generic name, versus say "ubuntu-template" remove entry from /etc/hostname
<pre>cat /dev/null &gt; /etc/hostname</pre>
<strong><a href="http://askubuntu.com/questions/191999/how-to-clear-bash-history-completely" target="_blank">Remove the user’s shell history</a></strong>

If you have switched to root at any point, do this as root and individual user accounts
<pre>history -w
history -c</pre>
That should about do it, depending on where this template is going, make sure any ISOs attached to the CD-ROM or networks for the NIC's are adjusted properly. While many of the steps were the same there were a few differences to be aware of. Anything else you like to clean out? Comment below please!