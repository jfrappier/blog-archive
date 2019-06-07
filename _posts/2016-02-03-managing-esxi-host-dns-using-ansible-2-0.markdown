---
layout: post
author: Jonathan Frappier
title: Managing ESXi host DNS using Ansible 2.0
permalink: /:title/
modified: 2016-02-03
category: ansible
tags: [ansible, vmware, vmware modules, dns, configuration management]
share: true
---
With the release on Ansible 2.0 came several new VMware modules that leverage pyVmomi, one of which was <a href="http://docs.ansible.com/ansible/vmware_dns_config_module.html" target="_blank">vmware_dns_config</a>. This modules runs directly against ESXi hosts, so if you have small lab with no vCenter (because reasons?) you can still use this module to ensure that the host(s) have the desired DNS servers. 

As with the previous <a href="http://professionalvmware.com/vbrownbag-devops-series/" target="_blank">"vsphere" modules which I demoed last year on the #vBrownBag DevOps series</a>, this tasks runs locally but leverage the previously mentioned pyVmomi instead of pySphere.

As of this writing, the Ansible documentation lists vSphere 5.5 as the only supported version. I am currently running the <a href="http://www.virtxpert.com/vmware-vcenter-server-appliance-vcsa-installation-resources/" target="_blank">vCenter Server Appliance (VCSA) 6</a> in my lab, along with ESXi 6 hosts, though be aware YMMV.

Before you get started, I had a bit of head banging going on trying to determine how to setup Ansible properly. I had been using pyVmomi 5.5.0 as well as 6.0.0 without much success, Larry Smith in his testing found that 5.5.0-2014.1.1 seems to work (best), so when you install make sure you run
<pre>sudo pip install pyvmomi==5.5.0-2014.1.1</pre>
Now, once that is working, just a <a href="https://github.com/jfrappier/ansible-test-playbooks/blob/master/vmware_esxi_dns.yml" target="_blank">nice basic playbook</a> in order to change DNS which you can find in my <a href="https://github.com/jfrappier/ansible-test-playbooks" target="_blank">test playbook repo on GitHub</a>. Here is a "before screenshot with one of my hosts having the incorrect DNS server:

<img src="/images/fulls/bad-dns.png" class="fit image">

Now, after running the playbook you can see that it has been corrected.

<img src="/images/fulls/good-dns.png" class="fit image">