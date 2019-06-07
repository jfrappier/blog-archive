---
layout: post
author: Jonathan Frappier
title: Installing Nagios Log Server
permalink: /:title/
modified: 2015-01-22
category: articles
tags: [nagios, nagios log server, installation]
share: true
---
When I think of syslog servers, I tend to think of VMware Log Insight and Splunk on the commercial side, and SyslogNG or an <a href="http://everythingshouldbevirtual.com/highly-available-elk-elasticsearch-logstash-kibana-setup" target="_blank">ELK solution like the one Larry Smith has blogged about</a> in the past. I've never thought of Nagios; turns out <a href="http://library.nagios.com/library/products/nagios-log-server" target="_blank">they have a logging solution of their own</a>, and it leverages the ELK stack. For many deployments, Nagios Log Server will fall into the commercial category, there is a free version which supports a single instance of Log Server running and a maximum of 500MB logged in a day (according to http://logfilemonitoring.com/ which appears to be affiliated with Nagios.com). However, for SMBs who may only have a few servers, or to support a specific application, Nagios Log Server may do the trick. Only one way to find out right? Let's get it installed!

Nagios delivers this either as an OVF (both 32-bit and 64-bit) running on CentOS or as a package which you can install on your own server. For purposes of getting this into my lab environment I am going to <a href="http://library.nagios.com/library/products/nagios-log-server/downloads" target="_blank">download</a> the 64-bit OVF package. Once downloaded you deploy as you would in Workstation (for my home lab) or the vSphere Web Client. Once the OVF deployment finishes, power on the virtual machine; if you're watching the console you should see the boot screen. By default it is set to DHCP and a default root password of nagiosls.

<img src="/images/fulls/nagios-log-server-console-sm.png" class="fit image">

Navigate to the IP address in the lease, if you do not have a DHCP server on your network, you can log into the console and configure /etc/sysconfig/network-scripts/ifcfg-eth0 appropriately for your network. Once you are on the web page you will be given the option to do a new install or add a new instance, enter your license key (I'll be starting with the trial to see if this converts into a free single instance/500MB per day solution), and your admin credentials.

<img src="/images/fulls/nagios-log-server-install-page.png" class="fit image">

When you have all of the information filled in; click the blue Finish Installation button. When the installation completes, you will be ready to log in, log in with the credentials you set in on the installation screen - you're now on the Log Server Overview page (which by the way nice job Nagios folks - heck of a lot better than I expected based on my past experience with Nagios core).

<img src="/images/fulls/nagios-log-server-overview-page.png" class="fit image">

From here you can explore the guides provided such as the Windows source or Linux source guides. You can also see we already have some logs from the graph in the lower left - these are the logs from the Nagios Log Server CentOS virtual machine. Click on the Dashboards link in the top navigation menu and explore some of the screens - as you can see you have a very friendly search interface to look for logs.

<img src="/images/fulls/nagios-log-server-dashboard.png" class="fit image">

Nagios has made it very easy to deploy their appliance; in my next post I will look at adding log files from a linux virtual machine, then an ESXi host.