---
layout: post
author: Jonathan Frappier
title: vRealize CloudClient Export and Import Blueprints
permalink: /:title/
modified: 2017-01-16
category: devops
tags: [vmware, cloud, vrealize automation]
share: true
related: [
    "Where is VMwares multicloud API gateway?",
	"So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors"
]
---
When vRealize Automation 7 first came out, one of the features I was very excited about was the ability to export, and import blueprints as YAML files. Being able to share, collaborate, or document how blueprints were setup seemed liked a no brainer. In order to to do this, you need to use the vRealize CloudClient available from the [Developer Center](https://developercenter.vmware.com/tool/cloudclient/4.1.0).

I am not sure if the CloudClient is technically in "beta" or in a "fling" state but one thing it does not do particularly well right now is provide error messages if a command is typed incorrectly. There are already some great detailed posts available from [Josh Coen](https://twitter.com/joshcoen) over at [ValcoLabs}(http://www.valcolabs.com/2016/05/23/blueprints-as-code-in-vrealize-automation-7-part-1-exporting-blueprints/), or from [Ryan Kelly](https://twitter.com/vmtocloud) at [VMtoCloud](http://www.vmtocloud.com/how-to-import-vra7-blueprints-as-code/). This post will serve more as a cheatsheet with commands you will find useful under certain scenarios. Note that the CloudClient does not allow you to "muck" with any vRealize Automation settings that you could not already do in the vRealize Automation UI. Also, roles are honored just as they are in the UI, a person who is only a user in a business group cannot perform actions that a tenant or IaaS administrator could. They help is reasonably good, simply type help for a list of all commands, or type help before any command such as help vra login to get a list of required, and optional flags. I have come across a few scenarios where something was marked as required that wasn't, and somethings that were marked as not required but were. If you type a command and it quickly goes back to the prompt you may have the syntax incorrect.

With that, let's dive into a scenario where you want to create a blueprint and publish it. I am also planning a "Common Administrative Tasks" posts as well - in some cases there may be cross over, for example in your organization you may have different teams creating blueprints, and then publishing them, but I am going to group those together. My assumption here is that you have downloaded, and extracted the CloudClient to a folder accessible to you.

Switch to the bin folder for the CloudClient and run either cloudclient.bat for Windows operating systems, and cloudclient.sh for Linux based operating systems. The remainder of this post will be done on Windows, if you know of any bugs/options that are different on Linux please let me know. If you see anything wrapped in {} that is some value you need to provide, such as a username. Do not include the {} in the actual command.

**Logging In**
Logging into the CloudClient is a two step process, there is a set of flags in the first command to connect to the IaaS Manager all in a single command but there is not an option to pass the domain (or its not documented), and as such have been unable to get it to work as a single command.

This command will allow you to type the name of your vRealize Automation appliance, username, and password.

<code>vra login userpass --tenant {tenant-name}

Optionally, you can pass the server, username, and password as one command.

<code>vra login userpass --server {server} --tenant {tenant} --user {username} --password {password}

Once logged in you will see the following output:

vRA 7.1 login: [ACTIVE]

IaaS Model Manager login: [INACTIVE]

Notice that IaaS Manager is INACTIVE, if you need to utilize commands related to the IaaS functionality, you will also need to log into the IaaS Manager with the following command:

<code>vra login iaas --server {iaas-server-vip} --domain {domain} --user {user} --password {password}

This command, unlike the vra login command, requires all of the listed flags - it will not prompt you for any of the values and just return you to the CloudClient> prompt.

You should now see:
vRA 7.1 login: [ACTIVE]
IaaS Model Manager login: [ACTIVE]

If you wish to check the status of your login at anytime, you can use:

vra login isauthenticated all

This will return with a status of ACTIVE, or INACTIVE for both the vRA login, and IaaS Model Manager.

**Working with Blueprints**
Now that you are logged into the cloud client, one of the use cases I was really excited about was the ability to export existing blueprints as YAML files. What could you do with that file you ask? A few thoughts I had were to keep the various blueprints used check into source control like [Git (enter shameless Commitmas plug)](https://www.youtube.com/playlist?list=PL2rC-8e38bUXloBOYChAl0EcbbuVjbE3t). Not only are all of the configuration settings backed up, but now other people in your organization can collaborate on the configuration, or new blueprints. Another use case might be for documentation such as disaster recovery, or audit purposes such as SOX, HIPAA, or PCI; you can easily hand over the configuration of all virtual machines deployed from vRealize Automation.

To export, and import a catalog item we will be working with several different objects just as you would in the UI - blueprints, catalog items, and services. It's worth noting that you must be logged into the CloudClient as a user who would have access to blueprints, and catalog items in the UI. The CloudClient isn't some magic hacking tool to get access. First, list the available blueprints:

<code>vra blueprint list

Make note of the ID of the blueprint you wish to export.

To export the blueprint type:

<code>vra blueprint detail --id {id} --export {export-path\filename.yml}

Navigate to the directory you exported the file to and open in your favorite editor. You can see all of the information related to your blueprint. For example, maybe this is a template used by dev and qa, and you want to retain many of the settings for the production version but add more resources. You could edit the CPU Default/Max/Min to meet your production requirements, or change the transport zone used by the virtual machine.'

Be aware that the id, name, and description in the YAML file will be overridden when we import the new file. I change these to match the values I will use in the import command because OCD. Make some change to your new file and save it as new-blueprint.yml (or whatever filename you want really). To import the bluerprint type:

<code>vra blueprint add vsphere --name {name-of-blueprint} --id {id-of-blueprint} --description "{Some description}" --inputfile {path-to\new-blueprint.yml}

Your blueprint should now be available in the design tab. Also, make note of the description - with a multi-word value you will need to enclose it in quotes otherwise the CloudClient will parse the second word as a separate flag/switch. With the blueprint imported (and hopefully published, if not publish it via the UI or re-import the YAML file with a state of PUBLISHED), you might want to add it to an existing Service with some set of entitlements (we'll cover creating services in a future post). You will need, like we did with the blueprints, a list of Services, and Catalog Items. Type:

<code>vra catalogitem unassigned list

Make note of the id of the item you just imported. Now list Services:

<code>vra service list

Locate the id of the Service you want to add the unpublished Catalog Item to and run:

<code>vra catalogitem service assign --id {id-of-catalogitem} --serviceid {id-of-service}

Navigate back to the vRealize Automation UI, log in as a user that would have access to the service you just updated with the new catalog item, and it should be available.

With just a few commands to remember, you could easily list all of your blueprints, export, update them, and import them to create new blueprints and catalog items.

Cheetsheet<br>
<code>vra blueprint list<br>
<code>vra blueprint detail --id {blueprint-id} --export {export-path\filename.yml}<br>
<code>vra blueprint add vsphere --name {name-of-blueprint} --id {id-of-blueprint} --description "{Some description}" --inputfile {path-to\new-blueprint.yml}<br>
<code>vra catalogitem unassigned list<br>
<code>vra service list<br>
<code>vra catalogitem service assign --id {id-of-service} --serviceid {id-of-catalogitem}<br>

This should give you some familiarity with the basics, but what if you have hundreds of catalog items? Have no fear, [James Bowling](https://twitter.com/vsential) has a great post with a [script he wrote to export in bulk](https://vsential.com/2016/02/26/export-vrealize-automation-7-blueprints). In my next post I will look to cover some basic administrative tasks that you can perform quickly from the CloudClient.