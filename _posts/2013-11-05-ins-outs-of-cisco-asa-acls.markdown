---
layout: post
author: Jonathan Frappier
title: The Ins and Outs of Cisco ASA ACLs
permalink: /:title/
modified: 2013-11-05
category: articles
tags: [cisco, acl, network, firewall, ]
share: true
---
<img src="/images/fulls/acl-in-outside-int.jpg" class="fit image">
On most firewalls I have ever inherited, access control lists (ACL) are typically applied to inbound traffic on an interface, you can however have an ACL applied to outbound traffic.  Some firewalls obscure this a bit and have you select the interface name where traffic is coming from and where its going to, on a Cisco ASA however you would apply the ACL either "in" or "out" of an interface.  This can be a bit confusing, especially if you aren't working with firewalls every day.  Essentially you just need to visualize where the traffic is coming FROM and where it is going TO in relationship to the interface you are applying the ACL to.   Lets take a look at an example; this is a fairly generic example you might see on most any ASA, we are applying an ACL to the "outside" interface for all "in" bound traffic to the interface, that is it is limiting traffic generated coming from one direction to another which will later be defined in the ACL itself:
<pre>access-group acl_outside_in in interface outside</pre>
Now the "outside" interface here is nothing more than the name given to a physical port on the ASA so it really could be anything.  In your ASA config it would look something like this:
<pre>interface GigabitEthernet0/0
 speed 1000
 duplex full
 nameif outside
 security-level 0
 ip address x.x.x.x y.y.y.y</pre>
If we changed nameif to homersimpson then the ACL would very simply be:
<pre>access-group acl_outside_in in interface homersimpson</pre>
The ACL name, "acl_outside_in" is also just a name that you give to the ACL, so again we could just as easily called the ACL "familyguy" which would turn the ACL into:
<pre>access-group familyguy in interface homersimpson</pre>
Typically I find that your ACLs and Interface names are descriptive of their purposes, unless you subscribe to the "security though obscurity" practice.  Other than ensuring you are using the correct ACL and Interface name (because then it would not work), the only item left in the ACL is really the "in" or "out."

Now, you would define the where the traffic is coming from and where is it going to.  To build on the above example, lets allow anyone on the public internet to access a server with an IP address 10.0.0.10 behind the firewall on port 443 (NAT is handled separatley).
<pre>access-group acl_outside_in extended permit tcp any 10.0.0.10 255.255.255.255 eq 443</pre>
What we have done here is said to allow traffic FROM anywhere TO 10.0.0.10, using a full subnet mask limits it to the specific IP address and eq defines the specific port.  You can continue to create rules in this manner and they would be applied in a top down manner until you reach your default deny rule.  A default deny rule is typically present by default to deny all traffic, then you explicitly permit specific traffic.  You could create a default permit rule that just allows everything, but that'd be bad mmmkay.

Looking at it graphically;

<img src="/images/fulls/acl-in-outside-int.jpg" class="fit image">

Now, here is where I think the mind gets twisted up, if you wanted to apply an ACL to another interface, lets use the example of an interface named "inside" to filter traffic going out to the internet you might think that would be applied "out" like in the example below:
<pre>access-group acl_inside_out out interface inside</pre>
But you would be wrong, that would apply the ACL on the outside of the inside interface.  Again think about the flow of traffic, you are coming IN to the inside interface, so your ACL would look like:
<pre>access-group acl_inside_outbound in interface inside</pre>
Because the traffic is coming from the inside network IN to the inside interface.  Lets look at an actual ACL example that allows a server connected to a network segment on the inside interface to have outbound access on port 443, that is the host specified in the ACL could access any website/service running on tcp port 443.
<pre>access-group ac l_inside_outbound extended permit tcp 10.0.0.10 255.255.255.255 any eq 443</pre>
The visual would be:

<img src="/images/fulls/acl-in-inside-int.jpg" class="fit image">

To sum it up, the direction in which the ACL is applied has to do with the direction the traffic is flowing and to which interface.  I seem to run mainly into ACLs applied IN to an interface as opposed to OUT of an interface.  Hope this helped!