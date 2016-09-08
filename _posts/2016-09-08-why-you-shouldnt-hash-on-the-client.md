---
layout: single
title: Why You Shouldn't Hash on the Client
date: 2016-09-08 00:00:00
tags: [passwords, security, hashing]
---

Every so often I see posts on Stack Exchange, or Hacker News where someone has
figured out that their passwords are being sent to the server and the server can
see them! The logic that we see is that if the password is hashed client side,
    then only the hash needs to be sent to the server, so the server never knows
    the password. Unfortunately, I sometimes even see this go one step further
    when people suggest that with this arrangement, HTTPS isn't required. Wrong. 

**Always use HTTPS - It's 2016. You have no excuse. None.**

Ok, now that we know to use HTTPS, why is hashing on the client a bad thing? Put
simply, when you hash on the client, the hash becomes the password. Instead of
authenticating with the server by sending a passphrase, you authenticate by
sending a hash. This means that your password search space is reduced to the
hash. Thankfully, with modern hashes this isn't really the end of the world as
the lengths are usually enough to still provide reasonable security. The problem
is that it is suddenly much easier to brute force as they don't need to run
through the algorithms, just the hashes.

The second reason is the most important. If the server is compromised, all the
hashes are available to everyone. Since only the hash needs to be sent to the
server, whoever has this list now has access to all accounts, as though the
passwords were stored in plain text (which the basically were). 

So that's why you shouldn't hash passwords on the client.

