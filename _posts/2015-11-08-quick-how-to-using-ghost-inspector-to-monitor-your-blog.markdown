---
layout: post
author: Jonathan Frappier
title: Quick How To - Using Ghost Inspector to monitor your blog
permalink: /:title/
modified: 2015-11-08
category: articles
tags: [monitor, monitoring, ghost inspector, blog]
share: true
---
Ghost Inspector is a tool for generating and continuously monitoring web sites and web applications, since it is free for up to 100 tests, I thought I would give it a try to ensure certain elements of my blog are working as expected. You can sign up for your free account at <a href="http://ghostinspector.com" target="_blank">ghostinspector.com</a>

The first step is to install the recorder and create a new suite of tests. I have installed the Chrome extension and created a suite called www.virtxpert.com

<img src="/images/fulls/ghostinspector.png" class="fit image">

Once your inspector is installed, click on the ghost icon, log in and you can being recording your test. Since I currently run ads on my site, I want to ensure that the ads are displaying correctly. To do this, you use what is called an assertion. Start the recording and open your the web page you wish to test, i this case this blog. Click on the now green ghost icon, select Make Assertions, and click on the image you want to ensure is appearing.

Once complete, Ghost inspector will run through the test you just recorded. As you can see here the images for my various banner ads were found when the page was loaded.

<img src="/images/fulls/ghostinspector-check-web-graphics.png" class="fit image">

In addition to the test, a video is taken and screenshot of the site you are testing. Another great feature of Ghost Inspector is the ability to schedule these tests, rather than manually running them. Click on Settings &gt;&gt; Schedule and select an option that fits your needs, I have opted for daily. You can create up to 100 of these tests in the free tier to ensure specific components of your website are working properly.

In addition to scheduling, you can also trigger tests via API to execute an entire test suite, very handy for web applications as part of the deployment process.

You can also enable notifications, either at the individual test level, suite level, or organization level. To set this at the organization level, click on your avatar on the top right side of the page, then click notifications. As you can see here you can set multiple email addresses, and even enable webhooks. With webbooks enabled you could theoretically receive updates of failed tests in something like Slack - very handy!