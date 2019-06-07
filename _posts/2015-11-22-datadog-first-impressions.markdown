---
layout: post
author: Jonathan Frappier
title: DataDog First Impressions
permalink: /:title/
modified: 2015-11-22
category: articles
tags: [data dog, datadog, monitoring, first impression]
share: true
---
After a sad first experience with another "Software as a Service" monitoring vendor which I can't sign up and get access to without first talking to a sales person, and because they don't publish their pricing, I decided to give DataDog a try. DataDog is free for up to 5 hosts, sufficient maybe for very small SMBs or for a specific, small application instance or $15 per host billed annually (or $18 a month billed monthly). <a href="https://www.datadoghq.com/pricing/#" target="_blank">You can sign up for DataDog free or pro</a> on their website, but for enterprsie support you'll have to talk to the dreaded sales person.

Once you sign up, you will need to select the services you use (though this doesn't seem to relate to the agents available); for example there are options for AWS, Docker, Google Cloud Platform, and vSphere (via Windows vCenter Agent, so it appears no VCSA), as well as other common applications such as Apache, Tomcat, PagerDuty, Microsoft IIS, and MicrosoftSQL (props to the DataDog team for including Microsoft technologies - they aren't the cool hipster things to use but damn stable and underrated in today's startupie/devopsie world).

<img src="/images/fulls/datadog.png" class="fit image">

As you can see above, I have selected AWS, IIS, vSphere, and SQL since these are things I have access to in my lab. Once complete, select your agent, I'll start with Ubuntu since that is what my Ansible control machine is running on. Once selected, you will get a command to run to pull down the agent.

<img src="/images/fulls/agent-datadog.png" class="fit image">

The command/directions provided will change based on your operating system, as you might expect. You will need admin/sudo access for the agent installation, which will proceed when you enter the command provided.

<img src="/images/fulls/datadog-install-ubuntu.png" class="fit image">

The agent will complete and connect to DataDog.

<img src="/images/fulls/data-dog-starting.png" class="fit image">

Once you have finished select OS types on the DataDog wizard, click next. You will see information collected by DataDog, right off the bat it shows that the machine I added has NTP issues; makes sense since I never setup NTP!

<img src="/images/fulls/data-dog-alert.png" class="fit image">

In addition to Events (which you are looking at above), you can also see
<ul>
	<li>Dashboards, which you will need to create</li>
	<li>Infrastructure lists or maps</li>
	<li>List of triggered monitors, and the ability to create new monitors</li>
	<li>Integration for the various systems I listed earlier</li>
</ul>
Time to let the agent do some work, I'll check back in on DataDog in a day or so to see how its doing collecting information as well as look at how easy (or not) it is to create new monitors (currently a primary complaint with Nagios and Sensu).