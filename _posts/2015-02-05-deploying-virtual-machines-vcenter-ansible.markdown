---
layout: post
author: Jonathan Frappier
title: Deploying Virtual Machines to vCenter with Ansible
permalink: /:title/
modified: 2015-02-05
category: ansible
tags: [ansible, vcenter, vmware, deploy, deploy vm, create vm, ansible playbook]
share: true
---
During the #vDM30in30 challenge I started playing with <a href="http://www.virtxpert.com/tag/ansible/" target="_blank">Ansible</a> to get to know it a bit better. One of the things I was curious about is the ability for Ansible to provision virtual machines directly to vCenter. After all if I am using Ansible to manage the configuration of my servers, it would certainly be nice to have a <a href="http://www.virtxpert.com/tag/playbook/" target="_blank">playbook</a> that also deploys my virtual machine, rather than another provisioning tool.

If you don't already have Ansible up and running, you can <a title="Installing Ansible via Git" href="http://www.virtxpert.com/installing-ansible-via-git/" target="_blank">get the dependencies installed and Ansible project cloned from GitHub pretty easily with a vanilla CentOS box</a>. Once Ansible is running and you have a <a href="http://www.virtxpert.com/tag/playbook/" target="_blank">playbook</a> or two under your belt you can take a read at the <a href="http://docs.ansible.com/vsphere_guest_module.html#examples" target="_blank">Ansible vsphere_guest docs</a>. This particular module is a bit lacking, I hope to add some updates soon and see if they get accepted.

As they mention in the docs, you will need to have pysphere installed; PySphere being a Python API to support interaction with vSphere/vCenter. PySphere is easy to install if you were able to follow along with my <a title="Installing Ansible via Git" href="http://www.virtxpert.com/installing-ansible-via-git/" target="_blank">Installing Ansible from Git</a> post; simply run
<pre>sudo pip install -U pysphere</pre>
With PySphere installed, you are now ready to create your playbook. The playbook example in the Ansible docs seemed to be missing something. Recall from my other posts you typically specify a host (linux box, not vSphere host) in your inventory file and direct the playbook that that host or groups of hosts, for example:
<pre>
---
- hosts: db
  remote_user: virtxpert
  sudo: yes
  tasks:  
  - name: update mysql-libs-latest 
    yum: pkg=mysql-libs state=latest</pre>
But, as you can see in the document ion, you specify the vSphere/ESXi host and vCenter server in the vsphere_guest task. So after a few iterations of trying to get that to work, I came across <a href="https://github.com/romeotheriault/ansible-vsphere_guest" target="_blank">this page on GitHub</a>. The hosts section is required for the playbook, but in this case the Ansible host will handle the work, instead of a remote host as you might typically see in a playbook to say configure a web server. So adding the following:
<pre>- hosts: 127.0.0.1
  connection: local
  user: root
  sudo: false
  gather_facts: false
  serial: 1

  tasks:
  - vsphere_guest:
      vcenter_hostname: vxprt-vc01.vxprt.local
      username: administrator@vxprt.local
      password: ********
      guest: testansible
      state: powered_on
      vm_disk:
        disk1:
        size_gb: 1
        type: thin
        datastore: vxprt-esxi01-gold-local
      vm_nic:
        nic1:
        type: vmxnet3
        network: vm
        network_type: standard
      vm_hardware:
        memory_mb: 1024
        num_cpus: 1
        osid: centos64Guest
        scsi: paravirtual
      esxi:
        datacenter: dc01
        hostname: vxprt-esxi01.vxprt.local
</pre>
With this playbook format in place, I am now able to run the playbook and have the virtual machine created in vCenter.

<img src="/images/fulls/playbook-ansible-provision-vm-vcenter.png" class="fit image">
<img src="/images/fulls/vcenter-vm-provisioned-ansible.png" class="fit image">

Up next, play around with <a href="http://docs.ansible.com/playbooks_prompts.html" target="_blank">vars_prompt</a> in place of some of the item names so when the playbook is run, it will prompt the user for input and using Ansible to clone an existing vSphere template.