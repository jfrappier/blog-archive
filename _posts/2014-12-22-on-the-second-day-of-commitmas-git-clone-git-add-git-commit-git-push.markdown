---
layout: post
author: Jonathan Frappier
title: On the second day of Commitmas - git clone, git add, git commit, git push
permalink: /:title/
modified: 2014-12-22
category: articles
tags: [git, github, commitmas]
share: true
---

Day two of commitmas, I have my Windows computer setup and SSH keys added to my GitHub account. Time to clone my existing repository to make a few edits. First, I created a directory on my computer called 'git' where I'll save all my work; cd to that directory and run git init

Now, log into GitHub and find the repository you wish to clone, in the lower right corner of the screen you will see the Git clone URL for the repository. In my case I want the SSH URL so click on SSH.

<img src="/images/fulls/git-clone.png" class="fit image">

Now in your console type git clone git@github.com:jfrappier/ansible-test-playbooks.git or your repository; when prompted enter the passphrase for your SSH key. You should now have your files on your local machine.

<img src="/images/fulls/git-clone-local.png" class="fit image">

Edit a file, for example the README file. Once the file has been edited and saved, we want to get the file back into the repository. To see what files have been changed, run git status. As you can see here my README file was modified.

<img src="/images/fulls/git-status.png" class="fit image">

As you can also see in the screen show we need to run git add, for example git add README or git add . - with the file added you now commit your changes by running git commit (this launches vi so press i to go to insert mode, enter a note then press esc :wq enter). Finally, git push to put the file back into GitHub. As you can see here I just updated my README file.

<img src="/images/fulls/git-push.png" class="fit image">

Tomorrow we will get into forking!