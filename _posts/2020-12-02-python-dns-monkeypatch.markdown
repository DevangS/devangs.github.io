---
layout: post
title: Monkey patching DNS lookups in Python 
category: systems
---

Background
========
Working in infrastructure can be incredibly rewarding. You get the opportunity to build systems that scale to millions of users and operate under tight latency tolerances. Most nascent companies run their services in containers, but children, come close to the fire, and let me tell you about some tales about the cowboy days of devops. Days long before there were clouds of compute, and oceans of containers. 

A time where you had to get an IPMI shell to revive a machine, and when the interface between dev and ops were python modules rather than Dockerfiles.

Once upon a time, there existed monolithic services. Where configuration as code meant hardcoding URLs for dependencies inline. I may never have written such code, but I have surely been entrusted with its maintenance. 

This is the first in a series of posts where we visit the snowdens of yesteryear. 

Problem
========
Given a legacy 3K LOC repository, we want to be able to reshape all traffic for 1 or more domains.  

How
=======
So how exactly do we implement this? We will override the DNS lookup done by a socket. In python this is implemented by [socket.getaddrinfo](https://docs.python.org/2.6/library/socket.html#socket.getaddrinfo)

	def _override_socket_dns(lookups):
	    actual_getaddrinfo = socket.getaddrinfo

	    def new_getaddrinfo(*args):
		override = lookups.get(args[0])
		if override is not None:
		    return actual_getaddrinfo(override, *args[1:])
		return actual_getaddrinfo(*args)

	    return new_getaddrinfo

	# example usage of the above
	rsp = requests.get(‘https://dayindev.com’)
	overrides = {‘dayindev.com’: [‘1.1.1.1’, ‘cnamesarecooltoo.dayindev.com’]
	socket.getaddrinfo = _override_socket_dns(overrides)
	from requests.packages.urllib3.exceptions import InsecureRequestWarning
	requests.packages.urllib3.disable_warnings(InsecureRequestWarning)

	# will make a request to our overriden records instead! 
	rsp = requests.get(‘https://dayindev.com’)


Use cases
========
The above implement chooses to globally override all lookups, but you can be more selective depending on your end goals. Some ideas: 

* Implement client side load balancing to do canaries, failovers, and blue/green deployments
* Decrypt outbound SSL traffic by rerouting all lookups to a local mitmproxy instance

