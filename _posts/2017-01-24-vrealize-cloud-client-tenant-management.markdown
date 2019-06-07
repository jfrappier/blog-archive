---
layout: post
author: Jonathan Frappier
title: vRealize CloudClient Tenant Management
permalink: /:title/
modified: 2017-01-24
category: vra
tags: [vmware, cloud, vrealize automation]
share: true
related: [
    "vRealize CloudClient Export and Import Blueprints",
	"So you want to be a full stack engineer? Advice on mapping out your career." 
]
---
In this post, I want to look at how you can manage tenants using the vRealize CloudCloud. Checkout my past posts on [exporting and importing blueprints](http://jfrap.com/vrealize-cloud-client-export-import-blueprints/) and running [data collection processes](http://jfrap.com/vrealize-cloud-client-data-collection/). These two posts both cover how to login as well.

There are several tenant related commands available, the first to look at is

<code>vra tenant list</code>

Pretty straight forward, right? Before I did anything I would want to know what tenants exist in my vRealize Automation deployment. With this command I can see the ID, name, and URL name for each tenant. Now, for example, maybe I need to add a new tenant, running

<code>vra tenant add --name {tenant-name} --url {tenant-url}</code>

You will receive a return code of success. Now I could run the list command again to verify my new tenant is listed. If I were in the UI, I would likely add a directory source next, so I could configure my tenant, and IaaS admin users within this tenant. This command is a bit more involved due to the number of inputs, so lets take a look at each of the required inputs for this command to work:

- tenantname: name of the tenant you are updating
- name: name for the identity store
- url: the URL for the AD source, not the tenant URL, this is not obvious from the help
- groupbasedn: You guessed, a search base in DN format (e.g. dc=domain,dc=tld)
- domain: fully qualified domain name (e.g. domain.tld)
- userdn: the user/account to connect to the AD source in DN format (e.g. CN=user,DC=domain,DC=tld)
- password: password for the user/account connecting to the AD source
- type: AD or LDAP, in the case of vRA7 this will always be AD

To better understand how to format the data, you can run a list command against an existing tenant with a tenant configured directory source.

<code>vra tenant identitystore list --tenant {existing-tenant-with-AD-source}</code>

If you string this all together in a working command it would be:

<code>vra tenant identitystore add --tenantname {name-of-vra-tenant} --name {name-for-ad-source} --url {ldap://domain.tld:port#} --groupbasedn {DN-for-groups} --domain {domain.tld} --userdn {bind-account-for-AD} --password {password-for-account} --type AD

Pretty long command, but hopefully you dont need to do this to often. If you run the identitystore list command again against your new tenant, you should see the details listed.

Now that I have a directory source, I can add tenant, and IaaS administrators with the following command:

<code>vra tenant admin update --tenantname {tenant-name} --role {iaas_admin or tenant_admin} --action {add or remove} --users {comma-separated-list-of-users/groups}</code>

Run this command twice, once for IaaS admins, and once for Tenant admins. The tenant admin update command can also be used for approval admins, and service architects. You can now run the list command, or check the UI to see the tenant, and settings you have created.

<img src="/images/fulls/vra-cloudclient-new-tenant.png" class="fit image">

