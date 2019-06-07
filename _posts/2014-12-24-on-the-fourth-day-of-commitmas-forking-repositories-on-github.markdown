---
layout: post
author: Jonathan Frappier
title: On the fourth day of Commitmas - Forking repositories on GitHub
permalink: /:title/
modified: 2014-12-24
category: articles
tags: [git, github, commitmas]
share: true
---

Yesterday Tim Jabut forked my Ansible test repo on GitHub to help me get markdown working and I merged that back into my repository. Today it is time to learn how to fork a repository on GitHub myself. If you take a look at a repository on GitHub, you'll see a Fork button on the upper right corner of the page:

<img src="/images/fulls/git-fork.png" class="fit image">

Click the Fork button, after a few moments the page will return - notice the difference?

<img src="/images/fulls/git-forked.png" class="fit image">

Yup, I now have a copy of this repository in my account. Switch over to your console and clone the repository locally. For example
<pre>git clone git@github.com:jfrappier/12-days-of-commitmas.git</pre>
I can now work on these files locally, for example here is the 12 Days of Commitmas (2014) opened in Markpad (choco install markpad)

<img src="/images/fulls/markpad-commitmas.png" class="fit image">

With a few changes made, I commit my file back to my repository. Do you remember what commands we need to do that?

<pre>git add .

git commit

git push</pre>

And here we are, files are in <em>*my* </em>repository. Next we will look at issuing a pull request so the owner of the repository can merge my changes into their repository.

<img src="/images/fulls/git-forked-commit.png" class="fit image">