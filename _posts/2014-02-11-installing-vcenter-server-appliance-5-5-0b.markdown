---
layout: post
author: Jonathan Frappier
title: Installing the vCenter Server Appliance 5.5.0b
permalink: /:title/
modified: 2014-02-11
category: vcsa
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 5.5]
share: true
---
I've been using the vCenter Server Appliance, AKA VCSA, in testing for several months now and ready to start using it for every day use.  These are the installation steps to get a fresh vCenter Server Appliance installed using the embedded PostgreSQL database and Active Directory for SSO.

Quick tip before you start, if you have hosts with different CPUs and will need to enable EVC, setup your VCSA on the host with the lower CPU, this might have to save you from doing some <a title="Enabling EVC when vCenter is a virtual machine" href="/enabling-evc-vcenter-virtual-machine/">VCSA trickery later to enable EVC</a>.
<ul>
	<li>Download via your my.vmware.com account, if you are not licensed you can register and download an evaluation <a href="https://my.vmware.com/group/vmware/evalcenter?p=vcops" target="_blank">here</a>.</li>
	<li>Create DNS records for your ESXi hosts and for your vCenter appliance</li>
	<li>Create a service account for for the SSO service to access Active Directory/LDAP and document the password</li>
	<li>Import the OVA you downloaded to an ESXi host using the C# client
<ul>
	<li>Click<span style="color: #0000ff;"><strong> File</strong> </span>&gt;&gt; <span style="color: #0000ff;"><strong>Deploy OVF Template</strong></span></li>
	<li>Select the OVA file and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>Review the template details and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>Name the VM, typically I match this to the DNS entry, for example <span style="color: #800000;"><em>vc01</em> </span>or <em><span style="color: #800000;">corp-vc01</span></em> for example and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>Select the datastore you wish to deploy to and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>Chose the disk format, either <span style="color: #800000;"><em>Thick Provision Lazy Zeroed</em></span>, <span style="color: #800000;"><em>Thick Provision Eager Zeroed</em></span> or <span style="color: #800000;"><em>Thin Provision</em></span></li>
	<li>Click <span style="color: #0000ff;"><strong>Finish</strong></span></li>
	<li>When the deployment finishes, click <span style="color: #0000ff;"><strong>Close</strong></span></li>
</ul>
</li>
	<li>At this point, you may want to check the resource configuration for the VM.  By default it deploys with 2 vCPU and 8GB of RAM</li>
	<li>Click on the newly created virtual machine and click the <span style="color: #0000ff;"><strong>Power on virutal machine</strong></span> link then click on the console tab to watch the VM boot</li>
	<li>By default, the VCSA will boot with DHCP enabled so you can log into a web configuration page to setup the VM.</li>
</ul>

<img src="/images/fulls/vcsa-console.png" class="fit image">

<ul>
	<li>Navigate to the URL shown in your console, for example https://192.168.1.50:5480</li>
	<li>Log in with the default username and password of root / vmware, check the box to <span style="color: #800000;"><em>Accept license agreement</em> </span>and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>Here is a point I consider a bit lazy, you are prompted to cancel the setup wizard to set the IP address and hostname, not sure why this wasn't included as part of the wizard but click <span style="color: #0000ff;"><strong>Cancel</strong></span></li>
	<li>Click on the <span style="color: #0000ff;"><strong>Network</strong> </span>tab and then click <strong><span style="color: #0000ff;">Address</span> </strong>sub-tab
<ul>
	<li>In the pull down menu for<span style="color: #800000;"><em> IPv4 Address Type</em></span> select <span style="color: #0000ff;"><strong>Static</strong></span></li>
	<li>Fill in the <span style="color: #800000;"><em>host name</em></span> field with the name you setup in DNS in the second step.</li>
	<li>Fill in the <em><span style="color: #800000;">IPv4 Address</span></em> and <em><span style="color: #800000;">Netmask</span> </em>fields with the appropriate values</li>
	<li>Click <span style="color: #0000ff;"><strong>Save Settings</strong></span></li>
</ul>
</li>
	<li>At this point you will lose access to the web page your originally navigated to since you changed the IP address.  Reconnect to https://new.ip.add.ress:5480 or https://f.q.d.n:5480 and log back in with root / vmware</li>
	<li>Click on the Admin tab
<ul>
	<li>Enter vmware for the current password and a new password you wish to use and document the password</li>
	<li>In the Email for expiration warning enter your email address so you will be notified of the root password expiration (the VCSA will use the SMTP settings we configure once vCenter is setup)</li>
	<li>Click <span style="color: #0000ff;"><strong>Submit</strong></span></li>
	<li>Typically I like to log out and back in with the new password to make sure its working as expected</li>
</ul>
</li>
	<li>Once logged back in, you should be on the <span style="color: #800000;"><em>vCenter Server</em></span> tab and <span style="color: #800000;"><em>Summary</em> </span>sub-tab</li>
	<li>In the <span style="color: #800000;"><em>Utilities</em> </span>table, click the <span style="color: #0000ff;"><strong>Launch</strong> </span>button next to <span style="color: #800000;"><em>Setup wizard</em></span></li>
</ul>
I will proceed here as if this is a new vCenter and SSO installation, using the embedded PostgreSQL database with a working Active Directory / LDAP
<ul>
	<li>Select the <span style="color: #800000;"><em>Set custom configuration</em></span> radio button and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>On the <em><span style="color: #800000;">Database Settings</span></em> page, keep <span style="color: #800000;"><em>embedded</em> </span>and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
	<li>On the SSO Settings page:
<ul>
	<li>Keep <span style="color: #800000;"><em>embedded</em> </span>for SSO deployment type</li>
	<li>Enter a password for the administrator@vsphere.local account, document and click <span style="color: #0000ff;"><strong>Next</strong></span></li>
</ul>
</li>
	<li>On the Active Directory Settings page click the Active Directory Enabled check box, enter the domain and the service account you created previously.</li>
</ul>

<img src="/images/fulls/sso.jpg" class="fit image">

<ul>
	<li>At this point it will test your AD credentials and proceed if they were validated</li>
	<li>On the review configuration page click <span style="color: #0000ff;"><strong>Start</strong></span></li>
</ul>
The configuration will start and notify you when certain components finish.  The SSO configuration was by far the longest but using these steps all configured as expected.  When you click close, you will return to the Summary sub-tab and see that all services you just configured are running.  You should now be able to log into https://f.q.d.n:9443 with the administrator@vsphere.local account and password configured during the setup wizard to configure Active Directory permissions.

A few final steps you should take is to verify the Java settings per <a href="http://www.gabesvirtualworld.com/cant-establish-connection-server-7331/" target="_blank">this post from</a> <a href="http://twitter.com/gabvirtualworld" target="_blank">Gabrie van Zanten</a> to ensure you have access to the remove console via the Web Client and make sure you have either disabled or setup <a title="Need SMTP for your home lab? Why not run it on your Domain Controller?" href="http://www.virtxpert.com/need-smtp-home-lab-run-domain-controller/">SMTP </a>to notify you about <a title="How to disable the vCenter Server Appliance Password Expiration #VCSA" href="http://www.virtxpert.com/disable-vcenter-server-appliance-password-expiration-vcsa/">password expiration</a>.

<strong>Summary</strong>

The first couple of times I deployed the VCSA I ran into <a title="VMware vCenter Server 5.5 Appliance (VCSA) Bugs" href="http://www.virtxpert.com/vmware-vcenter-server-5-5-appliance-vcsa-bugs/">odd errors and performance problems</a>, however 5.5.0b deployed without a problem and performance out of the box is fantastic.  The VCSA is something I am likely to use for many deployments.

&nbsp;