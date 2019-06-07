---
layout: post
author: Jonathan Frappier
title: Installing VMware vCenter Server Appliance 6.0
permalink: /:title/
modified: 2013-05-31
category: vcsa
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6]
share: true
---
<img src="/images/fulls/vcsa-6-web-client.png" class="fit image"> Generally, installing virtual appliances has been pretty straight forward – import an OVA and enter the necessary details in the deployment wizard, or access the virtual appliances management interface (such as those typically on port 5480 from VMware). However, as of the Release Candidate for VMware vSphere 6.0, the vCenter Server Appliance (VCSA) installation takes a much different approach than what you’ve been used to.

A few vCenter Server Appliance prerequisites

First, it should be noted that you can only install the vCenter Server Appliance (VCSA) from Windows. I was first turned onto the VCSA because I was at an all OSX/Linux shop so it made sense to use something we were accustomed to using already. For now, you’ll need a Windows box to at least get the appliance deployed;  then you can punt (please note also this is based on Release Candidate (RC) code and could change in the final release).

You CAN deploy the VCSA 6.0 to both ESXi 5.5 or 6.0 host. If you currently have a 5.5 environment you can deploy the VCSA without upgrading your hosts, but if you did not take  the plunge into 5.5 you’ll have to bring at least one host online running 5.5. or 6.0.

Finally, before getting started, you MUST create DNS records before running the installer. I was struggling with the new installer because I’ve just been used to doing my DNS records after I deployed the VCSA, but before running the setup through the management interface. However with a little help from Emad Younis (@Emad_Younis) I was able to point me in the right direction. With 6.0 all of the configuration is done from the initial setup wizard. When it’s finished installing, vCenter is ready to run.

The installation wizard will NOT give you an error if this does not exist, instead it will fail during the installation!

As you can see here I have my forward and reverse DNS records ready to go on .9

<img src="/images/fulls/vwmare-vcsa-dns.png" class="fit image">

Installing the vCenter Server Appliance

As with the older versions of the VCSA, it all starts with a download; however in this case you will be downloading an ISO image. Once the ISO image is downloaded either mount the ISO on your Windows box or extract the ISOs into a folder (as seen here).

<img src="/images/fulls/vmware-vcsa-iso-extracted.png" class="fit image">

Now that you have access to the files, drill down into the vcsa folder, there you will find the VMware-ClientIntegrationPlugin-6.0.0. Install this application on your Windows box (double click, Next, Accept/Next, Next, Install, Finish). Once the plugin finishes installing, back up one folder level and open the index file. As you can see here I am on Windows Server 2012, thus at least IE10 however opening the index in IE10 gives me a warning that I need to upgrade to at least IE10 or 11, so yea I’m going with Chrome. As with any plugin, you must enable it in Chrome. Click on the puzzle piece with the red x, then click Always allow and refresh the page and click the Allow button.

<img src="/images/fulls/vmware-vcsa-chrome-enable-plugin.png" class="fit image">

Newer versions of Chrome/Client Integration Plugin may prompt you like this; click Remember my choice and then the launch Application button

You should now see the vCenter icon along with a large Install and Upgrade button, click on the Install button. You will get a UI very similar to what you would get deploying a virtual appliance.

<img src="/images/fulls/chrome-vcsa.png class="fit image">

1.  After carefully reading the license agreement, printing it for your records, and having it signed by an attorney, click the I accept… check box and click Next.

2.  Now you can chose to deploy to your target server. Specify your ESXi host (5.5 or above!), username and password. Note the requirements at the bottom of the page; if you are using a vDS the port group must be ephemeral.

<img src="/images/fulls/vcsa-ephemeral.png" class="fit image">

Now click Next.

If you are using self signed/untrusted certificates click Yes when prompted.

3.  The next step is to name your appliance. In my case, like I have created in DNS, my appliance name will be vxprt-vc02.vxprt.local. Provide the OS root user password and click Next

4.  On the deployment type you can chose to install an embedded Platform Services Controller (which includes Single Sign-On in vSphere 6.0), just the the PSC, or just vCenter. You can have multiple Platform Services Controllers, and they can be different types. For example you could do a stand-alone PSC and have an embedded one with the VCSA. When the installer says “embedded” it really just means the components will be installed on the same virtual appliance as vCenter. I’ll be doing embedded here. Click Next

5.  Chose whether you have an existing SSO domain or you will be creating a new one. I will do this install as a new SSO domain type deployment. Enter the administrator password, and domain. To stay consistent with what I know about SSO, I’ll enter vsphere.local and Default-First-Site for the site name. Click Next.

<img src="/images/fulls/vcsa-6-new-sso.png" class="fit image">

6.  Select the appliance size that supports your environment, including the new “tiny” deployment for up to 10 hosts. Click Next

7.  Select the datastore you will to install to, and whether to THIN PROVISION the vmdk (no VMware, I’m not calling it “Thin Disk Mode” – THIN PROVISION!). Click Next

8.  If you’re an Oracle shop, you have a choice on step 8, otherwise just click Next.

9.  Chose a network (this will be based on the host you deployed to), and how to assign IP information including the host name – This MUST match DNS. I’ll select static as that is what I would want to do for this type of server. Finally enter the NTP server and click next (I’ve also enabled SSH so I can connect directly to the virtual machine.

<img src="/images/fulls/vmware-vcsa-6-step-9-network.png" class="fit image">

10.  Review the settings you’ve enter, make sure your IP information and host name are all correct and click Finish. The installation of vCenter and the VCSA will start. You’ll even see it installing packages, that’s right this is a ground up build, not just a bunch of packages pre-installed on a virtual machine!

<img src="/images/fulls/vmware-vcsa-6-installation-progress.png" class="fit image">


Once the installation is complete, you can connect to https://fqdn/vsphere-client (no more 9443! One less question on the VCP6 I guess :) ).

<img src="/images/fulls/vcsa-6-complete.png" class="fit image">

Log in as the user@domain you configured during the installation.

<img src="/images/fulls/vcsa-6-web-client.png" class="fit image">

That’s it, your vCenter Server Appliance is now installed, of course, now comes the fun part like creating your clusters, setting up HA, Admission Control, DRS, and other great features enabled by vCenter!