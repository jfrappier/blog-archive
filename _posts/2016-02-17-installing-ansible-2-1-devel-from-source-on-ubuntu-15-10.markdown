---
layout: post
author: Jonathan Frappier
title: Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors
permalink: /:title/
modified: 2016-02-17
category: ansible
tags: [ansible, ansible devel, ansible 2.1, ubuntu, ubuntu 15.10]
share: true
---
There were some new features recently added to the development branch on Ansible I wanted to use for testing, from what I could gather reading the source for one particular feature, adding validate_certs to the VMware modules, required Python 2.7.9 (I'm no Python programmer so don't take my word for it). So I had two options, install Python and dependencies from source on Ubuntu 14.04 since it only has Python 2.7.6 available via the apt repos, or hop on up to 15.10 which has Python 2.7.10.

I installed 15.10, and for the most part the Ansible documentation is still accurate, however there is at least one additional step (I say at least because I was able to install all the required Python packages, but still have not successfully tested some of the modules I wanted to work on). When installing Python, Python-Dev, and Python-Setuptools, you need to also install g++ otherwise you will get a pycrypto error when trying to install additional Python modules via pip. Just to point out this is not an Ansible problem, we haven't even installed Ansible yet, just something extra on Ubuntu 15.10 you will need to do if you want to install anything via pip.

So, a very quick run down:

1:
<pre>sudo apt-get install python python-dev python-setuptools g++</pre>
2:
<pre>sudo easy_install pip</pre>
3:
<pre>sudo pip install paramiko PyYAML Jinja2 httplib2 six pyvmomi==5.5.0-2014.1.1</pre>
Note that pyVmomi is only needed if you plan on using the VMware modules, otherwise that can be removed (you will need valid certs for the VMware modules, so testing with self signed certs does not work). You should now be ready to clone Ansible on Ubuntu 15.10, however this is a development branch, and as such there seems to still be bugs. For example the vmware_dns_config module which worked on Ansible 2.0, now fails with a "certificate_verify_failed" on 2.1, even though I have specified validate_certs: false. In hindsight this probably should have been 2 posts, 1 about installing Python on Ubuntu 15.10, and the other installing Ansible but oh well it's the thing I'm currently working on :)