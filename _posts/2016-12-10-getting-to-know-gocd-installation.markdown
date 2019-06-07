---
layout: post
author: Jonathan Frappier
title: Getting To Know GoCD - Part 1 Installation
permalink: /:title/
modified: 2016-12-10
category: devops
tags: [containers, vms, devops]
share: true
related: [
    "Where is VMwares multicloud API gateway?",
	"So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors"
]
---
DevOps is a hot topic, and a hot job title. What I have found is in many cases when someone approaches you with a "DevOps" job, what they are really looking for is a release engineer. I have been approached multiple times over the last few months and there as the assoicated laundry list of job requirements: scripting in python or powershell, knowledge of VMware and/or AWS. Somehwere in the middle, generally only on a bullet point was something related to Continuous Integration (CI) or Continuous Deployment (CD).

I started to test my theory that these companies were really looking for a release engineer by telling them I had most of what they were looking for. When I would point out I didn't know Python, or AWS it wasn't a problem - "oh we can teach you that" they would say. But as soon as I said I didn't have experience with CI/CD it was an instant deal break, even when I had every other thing they were looking for (and some of those jobs had A LOT of requirements).

With that in mind I started looking for a CI/CD platform to use, the obvious choices being Jenkins or Travis, but it was another that I came across on Twitter that I decided to start with - https://www.go.cd/. GoCD is opensource, so free to download and install, but what attracted me as I was trying to figure out which to learn was the amazing documentation they provided. I am just through the installation now but so far its some of the best I have come across - commercial or open source. As part of Commitmas https://github.com/commitmas/commitmas-3-return-of-commitmas I have been working on a GoCD role for team Ansible, if you are interested in checking out GoCD you can get the server role and agent playbook (note still a WIP) from my fork of the commitmas-ansible repo, just grab the ansible-gocd branchhttps://github.com/jfrappier/commitmas-ansible/tree/ansible-gocd. As always PRs welcome!

For now run ansible-playbook gocdsrv.yml from the root of the repo to install the server components, and /roles/ansible-gocd/tasks/gocdagnt-full-playbook-example.yml to install the agent. Adjust become_user and the hosts to fit your inventory file as needed. Additionally edit /roles/ansible-gocd/files/go-agent to match the IP of your GoCD server. I should have this cleaned up in the next couple of days so both the agent and server can be run as roles.

Here you can see the GoCD server UI after a fresh run, with the agent.
<img src="/images/fulls/go-install-part1.jpg" class="fit image">