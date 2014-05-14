---
layout: post
title: The Need for a Password Security Group
date:   2014-05-16 12:00:00
tags: [password, security, encryption]
---

Recently I have become increasingly exposed to companies with bad password security practices. These range from Virgin Media, who require that your password be 8 to 10 characters, begin with a letter, and consist of only letters and numbers, to the Bank of Scotland who limit your password to 20 characters and it is case *insensitive*.

Many years ago when computers were first starting up, and passwords were brand new ideas, it made sense to have a simple word or phrase which was easy to remember. Early unix machines even limited your passwords to 8 characters. This wasn't an issue however as the ability to crack passwords wasn't exactly in place. The year is now 2014 however, and password cracking is a fun game that anyone can take part in[^1]. With this in mind, password security is an increasingly concerning task for both the users and the developers of a system.

In the case of users, there is one sure way of making your passwords more secure and that is to use a password manager. Personally I use [1Password](https://agilebits.com/onepassword) and have found it absolutely fantastic. However, it doesn't matter which one you use as long as it is secure. Popular alternatives to 1Password include [KeePass](http://keepass.info/) and [LastPass](https://lastpass.com/). Use which ever you like the most. 

For developers we have a whole different story. Let's see some of the reasons that developers put in place bad password restrictions (both in terms of character limits and length limits):

1. The demands of a manager / PM.
2. Users get confused when their passwords are too long.
3. Support requests go up when extra characters are allowed.
4. It has to be integrated with a legacy system.
5. Long passwords can make Denial of Service attacks easier.
6. Our database isn't configured for longer passwords.
7. If we allow special characters users might abuse this to perform injection attacks.

The list goes on and on. However, there is only one reason on that list which is valid for a limitation of any kind. That reason is number 5. Long passwords can make DoS attacks easier. That is a fact. One which is made much worse when you combine it with a PBKDF (which is what you should be using if you are a developer!). For all the others though, they are solvable. If your manager or PM thinks it should be shorter, explain why they are wrong. If your database can't handle longer passwords, then fix it. If you are worried about injections then use prepared statments. As a developer you have a duty to the users of your product. They have every right to expect you to have done your utmost to ensure their data is kept secure. Unfortunately, not all developers and companies do this, which brings me to my main point. How do we encourage companies to do this?

I propose a group is formed of individuals who care strongly about the security of passwords, and ideally have expertise in the field. This group would be responsible for drawing up requirements and recommended practices for password complexities and storage, and would liase with companies to help encourage them to adopt these practices. With the support of companies like Google and Mozilla, their browsers could alert users when a website does not follow the practices the group recommends. This could be accomplished with extensions at first, before complete browser integration. 

By having a dedicate group representing the people for security, we might have a single coherent voice, and hopefully companies will listen.


[1]: <http://arstechnica.com/security/2013/03/how-i-became-a-password-cracker/>
