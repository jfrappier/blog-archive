---
layout: post
author: Jonathan Frappier
title: Quick start guide for Ansible Vault
permalink: /:title/
modified: 2017-10-27
category: advice
tags: [ansible, ansible-vault]
share: true
related: [
    "Step 1 of building a DevOps Culture",
	"Using vSphere 6.5 APIs with Ansible" 
]
---
Ansible Vault is a handy utility available with Ansible since 1.5. It allows you to encrypt, well really almost anything. The obvious use cases are SSH keys and passwords, but also internal information should also be encrypted - database names, IP ranges etc. tl;dr at the end of the post if you don't want the explanation/back story.

If you're just getting started with Ansible, the documentation for this utility can be quite overwhelming as it is more of a brain dump of everything it can do with no real examples of how to use what you have just encrypted unlike the great examples included in many of the modules. I had what I thought was a reasonably simple use case: I wanted to encrypt a password and use ansible-vault when running my playbook. Unfortunately none of the examples I was able to find covered such a basic use case. Many of the well thought blogs/gists I found on this covered using vault for inventory groups via a group_vars file. My use case was running against localhost and using the uri module to connect to a remote system.

After a bit of frustration, I was able to get this working in just a few steps. In case you are trying to just encrypt a password and use that string in your playbook, here you go:

First Create a vault. A vault is nothing more than a YAML file that stores the encrypted information. To create a vault just enter the command:

```
ansible-vault create vaultname.yml
```

vaultname.yml should be something relevant to you and stored in a safe location. Once created, it will launch your default editor. Simply type your password in the editor, save and exit. Your password is now encrypted!

To view the encrypted string to use in your playbook just view the file. cat vaultname.yml. You will see something like this:

```
$ANSIBLE_VAULT;1.1;AES256
6363376533336339393835303861306464353661383564650a353939313339326132653062356664
32643935323163623132306664333265656436666263616332303834376234353833313466623438
3031383262653236320a623933363563363730306538333430643736646665653162313832663532
34323964333037386366396438356664323064633265313733336534376232643462316631613937
3439
```

Pretty simple right? How and where to place this in my playbook proved to be the hard part. With a vault encrypted value, you have to lead the string with `!vault |` so the playbook knows to get this vaule from the vault. I had placed the string into my playbook, but was receiving errors that the decrypted password as not json serializable. What threw me for a loop is that it was clearly (maybe to a fault) decrypting the password. That finally lead me to a StackOverflow post which suggested putting the string in a task level variable and using that variable in place of my password (Thank you Konstantin from StackOverflow!). This did it.

I created the following variable:

```
 passvc: !vault |
   $ANSIBLE_VAULT;1.1;AES256
   32643935323163623132306664333265656436666263616332303834376234353833313466623438
   3031383262653236320a623933363563363730306538333430643736646665653162313832663532
   34323964333037386366396438356664323064633265313733336534376232643462316631613937
   3439
```

And used that in place of the clear text password.

```
password: '{{ passvc }}'
```

Now, to run the playbook I provide the vault-id, and vault password like this:

```
ansible-playbook playbook.yml --vault-id vault.yml --ask-vault-pass
```

That is it! There was so much information on Ansible Vault it had my going into rabbit holes I didn't need to follow for this use case.

tl;dr
1. `ansible-vault create vaultname.yml`
2. Enter password for the vault, and then the password to encrypt in the editor.
3. Save and close the editor
4. cat vaultname.yml and copy string
5. Setup variable leading with `!vault |` (example https://github.com/jfrappier/vSphere-6.5-API-Playbook-Examples/blob/master/new-datacenter-api.yml)
6. `ansible-playbook --vault-id vault.yml --ask-vault-pass`