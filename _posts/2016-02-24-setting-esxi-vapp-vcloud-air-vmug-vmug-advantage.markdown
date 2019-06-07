---
layout: post
author: Jonathan Frappier
title: Setting Up ESXi vApp in vCloud Air
permalink: /:title/
modified: 2016-02-24
category: articles
tags: [vmware, vcloud air, esxi, vsphere 6, vmug, vmug advantage]
share: true
---
<em>**Disclaimer: My VMUG Advantage subscription was provided courtesy of the Boston VMUG. This post was not paid for, nor reviewed prior to publishing. This is simply my opinion after using the service. Additionally, I am an EMC employee**</em>

In my last post, I setup a VDC and looked at what the cost would be to run different vApp configurations</a>. It seems that I can run a small vSphere environment for roughly $1 a day, well under the $50 a month credit you receive from the VMUG Advantage (and obviously much less than the cost of buying home lab equipment, although lets admit that is also some of the fun). Since vCloud Air does not provide an ESXi template, I am going to build one using the vCloud Director GUI (which is where you would build any custom vApp).

Log into your vCloud Air account, and access your VDC and create a new virtual machine; when prompted select create from scratch which will launch vCloud Director.

<img src="/images/fulls/vcloud-director-ui.png" class="fit image">

Click the create new vApp to launch the wizard. Here you can provide information about your vApp such as the name, the data center you wish to place the vApp in. Another excellent option for ensuring you do not go over your vCloud Air credit is to configure the runtime lease for something other than "Never Expires" so you do not accidentally leave everything running when not in use. For example you can set the runtime lease to 12 or 18 hours so that it will automatically be stopped to save yourself a few billing hours while you are sleeping in you forget to shutdown.

<img src="/images/fulls/create-vapp.png" class="fit image">

Click next, on the Add Virtual Machine step click New Virtual Machine; unlike you've come to see in current versions of vCenter there is not ESXi OS type, here select other 64-bit and check the box to expose hardware virtualization. Since this is an ESXi VM you'll need at least 2 vCPU and I am adding 12GB of RAM. For the drive, this is your choice as you get up to 30GB of storage for free, so whether you use 1GB as I typically do for my virtual lab ESXi hosts, or 30 GBs the cost stays the same.

<img src="/images/fulls/create-vm.png" class="fit image">

Since this is a vApp, you can add several different VM's into the vApp as you would expect. I'm not sure what the vCloud Director/vCloud Air best practice is but for now I am going to keep a single ESXi host in this vApp. Click Ok and next, select the desired storage tier from vCloud Air (SSD accelerated obviously being the faster option). Click Next and select the network you want to create (I didn't get into this yet, but you can create different networks in vCloud Air, the default routed network was created for you when you created the VDC). I didn't make any changes to the system networking, so click though here and click Finish.

Now, if you navigate to VMs in vCloud Director you can see the VM that was created with the vApp.

<img src="/images/fulls/vcd-vm.png" class="fit image">

Here the VM can be managed, for example adding different drives or mounting an ISO. Select the VM, click the gear icon, and select properties. Here you can see you could add additional network interfaces or drives.

<img src="/images/fulls/edit-vm.png" class="fit image">

Click cancel, unless there are some changes you want to make. In order to install ESXi you will need to upload an ISO into your catalog. In order to do this, you'll need to follow <a href="http://kb.vmware.com/kb/2110191">KB 2110191 to upload the ISO to vCloud Air using the OVFtool</a>. For example
<pre>ovftool.exe --sourceType="ISO" "<i>path\mysoftware</i>" "vcloud://<i>vcloudairusername</i>:
<i>password</i>@<i>vcloudairendpoint</i>?org=<i>myorg</i>&amp;vdc=<i>myvdc</i>&amp;catalog=<i>mycatalogname</i>&amp;media=<i>mymedianame</i>.iso"</pre>
You can get your vCloud Air endpoint from the vCloud Air GUI by clicking on the blue link button

<img src="/images/fulls/endpoint.png" class="fit image">

You need only the FQDN, not the https:// or subfolders. For example here you can see an example I used

<img src="/images/fulls/ovftool.png" class="fit image">

Once complete, you can see the ISO in your catalog under Media &amp; Other

<img src="/images/fulls/catalog.png" class="fit image">

Navigate back to My Cloud &gt;&gt; VMs; right click the VM &gt;&gt; Insert CD/DVD from catalog, select the ISO and click insert. Power on the VM and click the console to install ESXi.

<img src="/images/fulls/esxi-console.png" class="fit image">

Please note that I have not done anything to provide IP addresses via DHCP in my environment yet, so for now I can do so statically, however if you have followed along you should now have ESXi installed. In my next post, I'll look setting up networking and a DC before moving on to the VCSA.