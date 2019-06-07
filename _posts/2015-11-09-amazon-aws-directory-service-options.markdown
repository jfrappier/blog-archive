---
layout: post
author: Jonathan Frappier
title: Amazon AWS Directory Service Options
permalink: /:title/
modified: 2015-11-09
category: articles
tags: [aws, amazon, directory services, iam]
share: true
---
As part of my lab rebuild, and efforts to understand Amazon Web Services better, I am looking to setup Directory Services to use with my home lab, and potentially eliminate a virtual machine in my lab. It was pointed out on Google+, however, the dangers in doing this if I have no internet connection, or AWS is unavailable. In this post I am going to take a look at options from AWS and take into account potential pitfalls, like the one pointed out to me. Please note that I am still learning about AWS, so if I misinterprested something from the features available please let me know. Always learning and happy to have my mistakes corrected.

In my last <a href="/quick-how-to-amazon-identity-and-access-management-iam/">AWS post I looked at IAM to create an manage AWS users</a>, and set permissions rather than using your root account, go ahead and log into your AWS Console, hopefully as an IAM user with specific privileges.

There are two options available from Amazon. First is to setup the Simple AD which is a Microsoft Windows Active Directory Compatible LDAP. You can use this for instances running in Amazon however it is run in isolation and only available to instances in AWS. That won't eliminate my need to run a local AD without having a VPN connection to AWS.

Another option is to setup the AD Connector to extend your Windows Active Directory to Amazon. This is useful for hybrid cloud deployments to maintain common user accounts across services. One potential pitfall of AD Connector is you need to have INBOUND AD ports open on your firewall from the Amazon IP range, I don't see this flying in organizations that understand security. Again this could be mitigated by the use of a VPN to Amazon, however that is not a position I am in. <strong>**Update: It was pointed out by Raphael Soeiro de Faria ( a former co-worker), on LinkedIn, that there is also a direct connection option with AWS if you did not want to use a VPN. Thanks for the update Raphael!**</strong>

Setting up Simple AD is straight forward, select Directory Service from the AWS Console &gt;&gt; Get Started &gt;&gt; Create Simple AD. You'll be asked for items you come to expect such as a DNS name, NetBIOS name and in the case of Amazon Directory Services, a "large" or "small" directory. You will also setup AD in two different subnets for availability purposes (you would/should always have two local DCs, same goes in the cloud brah!)

<img src="/images/fulls/amazon-directory-service.png" class="fit image">

Also, Directory Services, unlike IAM, carries a cost of either $36.50 per month for a "small" Simple AD or AD Connector, and $109.50 per month for large. If you are an existing Workspaces, WorkDocs, or WorkMail customer, Directory Services is free.

In either case, I'll keep running my 1vCPU 1GB RAM Windows DC in my lab. If you are going to be investing in AWS services, Directory Services though, this is certainly something worth looking into. If you are just running a lab, more than likely IAM will work just fine to continue testing AWS.