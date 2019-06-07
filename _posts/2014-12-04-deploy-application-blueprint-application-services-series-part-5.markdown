---
layout: post
author: Jonathan Frappier
title: Deploy an Application Blueprint - Application Services Series Part 5
permalink: /:title/
modified: 2014-12-04
category: application services
tags: [vmware, vsphere, vcenter, vra, apps, app services]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Enabling Disaster Recovery with Ansible", 
    "DevOps IRL Boston VMUG Usercon with @szelechoski"
]
---

In part 4 we published an application blueprint through Application Serivces, that is pretty awesome but we still really haven't done anything just yet.  I mean its all just about working but the real hard part is creating the application blueprints.  Just for fun, lets create a generic blueprint and run a deployment.  While logged into Application Services go to Applications and click on the green + (plus) button to create a new application.
<ul>
	<li>Name the application and select a business group, if you've followed along my various home lab series you would select StarWars here since it is the only business group we gave permission to in vRealize Automation.</li>
	<li>Click save, click Create Application Version then click Save</li>
	<li>Now you are able to create a blueprint; click Create Blueprint</li>
	<li>Drag the logical template to the design pane, again if you're following along with me this would be the CentOS 64 logical template</li>
</ul>
<img src="/images/fulls/apps-app-design.png" class="fit image">
<ul>
	<li>Now all this would do is create a virtual machine like you could do through vRealize Automation or vSphere; here however we also have several preconfigured services we can drag into our logical template to install applications.</li>
	<li>Let's do a typical single node web and database server</li>
	<li>Drag Apache, vFabric RabbitMQ and and vFabric Postgres into the logical template, it should look something like this:</li>
</ul>
<img src="/images/fulls/apps-app-services-added.png" class="fit image">
Now one of the hardest parts about automating something is now all the dependencies.  In this scenario I happen to know a few things are missing, not because I am a genius but because I went through several iterations of this blueprint before getting it to work.  This, however also allows me to demo some other features of Application Services.  In my CentOS template, SELinux is enabled - now I could convert my template to a virtual machine, disable it, clean up the virtual machine machine again and convert it back to a template.  It's what I would have done not 6-8 months ago.  Now, however, I'll simply use the tools available to me, tools like Application Services or <a title="Ansible Playbooks – A more advanced example" href="/ansible-playbooks-advanced-example/">Ansible</a> to put the virtual machine into the state I want it:
<ul>
	<li>From the Application Components page, drag two "script" items into the logical template</li>
	<li>Edit the first script by clicking on it; name it (no spaces), click on Actions, click "Click here to Edit," copy the following into the window and click the reboot checkbox</li>
</ul>
#!/bin/bash
# set SELinux disabled
cp /etc/selinux/config /etc/selinux/config.bak
sed -i s/SELINUX=permissive/SELINUX=disabled/g /etc/selinux/config
<ul>
	<li>SELinux will now be disabled upon reboot.</li>
	<li>We also have to tweak the EPEL install to allow it to pull data properly (seems to be a known issues right now).  Rather than letting the EPEL package install as part of the services we used earlier, we can also do that in a script and configure the options we need for it to work.</li>
	<li>Edit the 2nd script as you did before but copy the following into the window</li>
</ul>
#!/bin/bash
# install EPEL
yum -y install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
sed -i "s/mirrorlist=https/mirrorlist=http/" /etc/yum.repos.d/epel.repo
<ul>
	<li>Click the OK button, you should now see something like this:</li>
</ul>
<img src="/images/fulls/apps-blueprint-configd.png" class="fit image">
<ul>
	<li>Now click the deploy button, name the deployment, and select the business group</li>
	<li>Click Map Details, ensure all details match what you have setup, and click Next</li>
	<li>Provide a name to your virtual machine and edit CPU and memory as needed (and to match your vRA blueprint limits) - click Next</li>
	<li>Review the deployment blueprint and click Next</li>
	<li>Click the deploy  button (you could also publish to vRA here as we did in part 4, but I'm just demonstrating the deployment)</li>
	<li>The deployment will start</li>
</ul>
Now at one point I wasn't sure it was working, I could see Application Services say it was working (system was under 80-90% load consistently) however I wanted to see what vSphere was doing.  As you an see in the two screenshots below, the virtual machines are being deployed as you might expect (they are from two different deployments so yes the dates are different)

Via the web client..
<img src="/images/fulls/apps-clone-in-vsphere-web.png" class="fit image">
Via the C# client
<img src="/images/fulls/apps-clone-in-vsphere-client.png" class="fit image">

In addition, you can zoom in on the Execution Plan pane to see what step the deployment is currently on

<img src="/images/fulls/apps-provisioning.png" class="fit image">

This process took quite a while in my lab, but it I am pretty resource bound now.  Now, as I mentioned this is an iterative processes, good chance it may have failed in your environment, review errors and run the deployment again.  After working through any specific environment issues you should be able to successfully deploy the application components.

<img src="/images/fulls/apps-successful.png" class="fit image">