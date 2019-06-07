---
layout: post
author: Jonathan Frappier
title: Adding Monitors to DataDog
permalink: /:title/
modified: 2015-11-24
category: articles
tags: [monitoring, datadog, data dog]
share: true
---
One of my primary complaints with something like Nagios is adding monitors in, which is typically done via config files that need to be reloaded - if its not in the correct format sometimes it doesn't work, sometimes Nagios doesn't restart. Now that I have a host added to DataDog, I am going to take a look at how to create a monitor. Since the host I added is my Ansible master, I will look at adding a monitor to ensure that Ansible is running.

In the DataDog interface, navigate to Monitors &gt;&gt; New Monitor. There are quite a few options to select from, including ensuring a specific host is checking into DataDog, this is quite likely a check you will want for machines that are of the "pet" variety, since DataDog will remove hosts that have not checked in within a 3 hour window, as well as specific metrics or services.

<img src="/images/fulls/datadog-select-monitor-type.png" class="fit image">

To monitor for a specific process, select ... yup ... Process. Sadly, this requires config file updates on each host no less, not something very easy to accomplish at scale without something like Ansible to ensure all of the desired config files are deployed. Still, I am going to check it out. To add a process monitor to a host you must edit conf.d/process.yaml according to the directions, problem with that is, as you might recognize, there is generally no conf.d directory at the root, so their directions are a bit off (as I find most linux directions are - sorry but its true). I will assume for a moment they are referring to the conf.d/process.yaml folder for the DataDog agent, but again this information isn't something that was presented during the install.

The full path you are looking for is /etc/dd-agent/conf.d - in this folder you will find an example process.yaml file (slight annoyonce - other items are in directories/sub-directories that start with
datadog" - but not this one). Here is the example file:
<pre>init_config:
  # the check will refresh the matching pid list every X seconds
  # except if it detects a change before. You might want to set it
  # low if you want to alert on process service checks.
  # pid_cache_duration: 120

instances:
# The `system.processes.cpu.pct` metric sent by this check is only accurate for processes that live
# for more than 30 seconds. Do not expect its value to be accurate for shorter-lived processes.
#
#  - name: (required) STRING. It will be used to uniquely identify your metrics as they will be tagged with this name
#    search_string: (required) LIST OF STRINGS. If one of the elements in the list matches,
#                    return the counter of all the processes that contain the string
#    exact_match: (optional) Boolean. Default to True, if you want to look for an arbitrary
#                 string, use exact_match: False
#    ignore_denied_access: (optional) Boolean. Default to True, when getting the number of files descriptors, dd-agent user might
#    get a denied access. Set this to true to not issue a warning if that happens.
#    thresholds: (optional) Two ranges: critical and warning
#         warning: (optional) List of two values: If the number of processes found is below the first value or
#                  above the second one, the process check will return WARNING.
#         critical: (optional) List of two values: If the number of processes found is below the first value or
#                   above the second one, the process check will return CRITICAL.
#     In this example, process check will return OK for 3 to 5 process. WARNING for 1, 2, 6, 7 processes and Critical below 1 or above 7.
#     CRITICAL is always dominant in case of overlapping.
#
#
# Examples:
#

  - name: ssh
    search_string: ['ssh', 'sshd']
    # tags:
    #   - env:staging
    #   - cluster:big-data
    thresholds:
      # critical if no sshd or more than 8 sshd are running
      critical: [1, 7]
      # warning if 1, 2, 6, 7 sshd processes are running
      warning: [3, 5]
      # ok if 3, 4, 5 processes are running

  - name: postgres
    search_string: ['postgres']
    ignore_denied_access: True

  - name: nodeserver
    search_string: ['node server.js']
</pre>
I made a copy of this file and removed the example checks, entering just:
<pre>- name: ansible
    search_string: ['ansible']</pre>
And restarted the agent (sudo /etc/init.d/datadog-agent restart<em>).</em> Once the new monitor is created, you will be able to select the new monitor and select the monitor thresholds.conditions that fit your needs and customize the notifications before saving. You can now navigate back to Monitors &gt;&gt; Manage Monitors to see what you have created, as well as their status.

<img src="/images/fulls/datadog1.png" class="fit image">