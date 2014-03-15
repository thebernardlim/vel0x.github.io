---
layout: post
title: Should I redirect HTTP to HTTPS?
date:   2014-01-28 12:15:57
categories: posts
---

No.

I've seen a lot of things about this all saying that it's a fantastic idea. Yes there are complaints about worse performance due to the complexity, cost of certificates, etc. but there is an extremely important reason why you should not redirect HTTP to HTTPS: It gives a false sense of trust. 

Imagine a user, Alice, who connects to `http://example.com`. Alice has been here before and is already logged in through the magic of cookies. However, when she goes to the site, it redirects her to `https://example.com`, but too late. Her cookies have already been sent through plain text on the first request. 

A different attack is an active man in the middle attack where the attacker can watch for HTTP traffic and redirects to HTTPS and spoof these websites using similar looking urls. An example of this was presented back in 2009 at Black Hat DC: <http://www.thoughtcrime.org/software/sslstrip/>

Fortunately there is a method to do this correctly. The [HSTS](http://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) protocol ensures that traffic only communicates with a server using HTTPS, which eliminates the above problems. 
