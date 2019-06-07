---
layout: post
author: Jonathan Frappier
title: Using vSphere 6.5 APIs with Ansible
permalink: /:title/
modified: 2017-05-26
category: ansible
tags: [vmware, vcsa, ansible, api]
share: true
related: [
    "vRealize CloudClient Export and Import Blueprints",
	"So you want to be a full stack engineer? Advice on mapping out your career." 
]
---
With the release of vSphere 6.5 comes a new set of REST APIs for interacting with various vSphere components such as the vCenter Server Appliance (VCSA) and vCenter components such as VMs, or clusters. Kyle Ruddy has a great post on getting started with the [Sphere APIs](https://blogs.vmware.com/code/2017/02/02/getting-started-vsphere-automation-sdk-rest). After reading his post, I wondered if I could use the vSphere APIs from an Ansible playbook via the URI module. The remainder of this post is really just smashing together Kyle's post and the [Ansible URI module documentation](https://docs.ansible.com/ansible/uri_module.html) - huge thank you to both Kyle and the folks from Ansible for the great documentation.

I learned from Kyle's post the first thing I need to do is authenticate in order to call any of the APIs. That was fairly straight forward thanks to the Ansible docs and examples.

~~~~
   - name: vcenter login
      uri:
        url: https://cloudvc.student.lab/rest/com/vmware/cis/session
        force_basic_auth: yes
        method: POST
        user: administrator@vsphere.local
        password: P@ssw0rd
        status_code: 200
        validate_certs: no
      register: login
~~~~

A quick review of that code block: I provided a name for the task (I personally like doing that so I can see each play run), used the uri module with the options you see below it - URL, method, etc. Finally I registered the results in a variable called login. I will use login in the subsequent plays to authenticate. Status_code is optional, since I am still learning how to do this I left it in to see what I might do with it, I am reasonbly confident you could remove it. Validate_certs set to no is required if you are using self signed certs (another side note, that didn't seem to work with some of the community VMware modules in 2.0 - curious if I can get those working now).

As an example, since this was simple, I  wanted to see if I could disable SSH access on the VCSA. The /appliance/access/ssh API uses a PUT method, so in our play below you'll notice that instead of method: POST I'll use method: PUT

~~~~
    - name: disable ssh
      uri:
        url: https://cloudvc.student.lab/rest/appliance/access/ssh
        force_basic_auth: yes
        method: PUT
        body_format: json
        body: "{{ lookup('file','sshoff.json') }}"
        validate_certs: no
        headers:
          Cookie: check the example playbook below, done fighting with markdown for today :)
~~~~

Very similar to the vCenter login play, what is differnet here is I am putting the necessary JSON into a file called sshoff.json in the same directory as the Ansible playbook and defined the body_format as json. At the bottom of the play notice the headers: Cookie - login is what I registered in the login play and passing that back vCenter. The contents of the sshoff.json file were pulled from the API explorer (https://vcenter.fqdn/apiexplorer).

Before running the play I used Postman to verify that ssh returned a value of true, ran the play with a value of false in the json file, and verified that after successfully running the playbook, SSH was now disabled on VCSA!

This is just a basic example, I will be looking into more now that I have the basic structure down. You can find an [example playbook here](https://github.com/jfrappier/ansible-test-playbooks/blob/master/v65-api-example-disable-ssh-uri.yml) Hope this helps!