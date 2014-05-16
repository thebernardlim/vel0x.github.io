---
layout: post
title: The Need for a Password Security Group
date:   2014-05-16 12:00:00
tags: [password, security, encryption]
---

Recently I have become increasingly exposed to companies with bad password security practices. These range from Virgin Media, who require that your password be 8 to 10 characters, begin with a letter, and consist of only letters and numbers, to the Bank of Scotland who limit your password to 20 characters and it is case *insensitive*.

Many years ago when computers were first starting up, and passwords were brand new ideas, it made sense to have a simple word or phrase which was easy to remember. Early unix machines even limited your passwords to 8 characters, but this wasn't really an issue as the ability to crack passwords wasn't exactly in place. However, the year is now 2014 and password cracking is a fun game that anyone can take part in[^1]. With this in mind, password security is an increasingly concerning task for both the users and the developers of a system.

In the case of users, there is one sure way of making your passwords more secure and that is to use a password manager. Personally I use [1Password](https://agilebits.com/onepassword) and have found it absolutely fantastic. However, it doesn't matter which one you use as long as it is secure. Popular alternatives to 1Password include [KeePass](http://keepass.info/) and [LastPass](https://lastpass.com/). Use which ever you like the most. 

For developers we have a whole different story. Developers have a duty to make sure that passwords are stored safely and securely. But as we know, this isn't always the case. Let's see some of the reasons that make developers put restrictions on allowed characters and lengths:

1. The demands of a manager / PM.
2. Users get confused when their passwords are too long.
3. Support requests go up when special characters are allowed.
4. The system has to integrate with a legacy system.
5. Long passwords can make denial of service attacks easier.
6. The database isn't configured for longer passwords.
7. Allowing special characters may allow injection attacks.

The list goes on and on. However, there is only one reason on that list which is valid for a limitation of any kind. That reason is number 5. Hashing long passwords takes time and this can introduce a route for DoS attacks. That is an unfortunate fact. And the time taken is much longer when using a PBKDF, which is something all systems should be doing. All the other reasons however, are solvable. If your manager or PM thinks it should be shorter, explain why they are wrong. If your database can't handle longer passwords, then fix it. If you are worried about injections then use prepared statments. As a developer, manager, PM or any one else affiliated with a product, you have a duty to the users of that product. They have every right to expect you to have done your utmost to ensure their data is kept secure. Unfortunately, not all developers and companies do this, which brings me to my main point. How do we encourage companies to do this?

Current companies face very little backlack about bad password policies. Just 10 days ago British Gas was subject to some ranting and disbelief over their password policies on twitter[^2], but already this is forgotten. More importantly the average user has no idea what sort of things British Gas are doing which could be detrimental to security and therefore risking their personal information. In order to solve this, companies need to have some driving force directed at them which encourages them to adopt safer practices. In order to acheive this I propose that a group is formed of individuals who care strongly about the security of passwords, and ideally have expertise in the field. 

This group would be responsible for drawing up requirements and recommended practices for password complexities and storage, and would liase with companies to help encourage them to adopt these practices. With the support of companies like Google and Mozilla, their browsers could alert users when a website does not follow the practices the group recommends. This could be accomplished with extensions at first, before complete browser integration. The most important point of this group is to encourage, *not* to bully. We must understand some of the reasoning that went into bad password management and empathise with the companies, before guiding them towards better practices. Further more, this group should also provide information for the general public in an easily accessible manner. Users have a right to know how their data is being handled and publishing information about practices and which companies conform and which ones don't can only be helpful to us in the long run. Remember, security should not be obtained through obscurity.

Passwords have been around for decades with little or no change. No group helps push forward the boundaries, improving and securing passwords. There are groups which exist with a focus on security, such as the [OWASP](https://www.owasp.org/) group. These groups focus on security in general, and don't have the resources to focus on passwords as a key area. Other groups such as the [FIDO Alliance](http://fidoalliance.org/) are approaching the problem from a different angle, by encouraging us to move away from passwords. This will be extremely important in the long run, however I feel that passwords will stick with us for a long time, and so should be represented. By having a dedicated group representing the people for password security, we might change the grumblings on Twitter and other social media, and instead have a single coherent voice, one which companies will listen to.


[1]: <http://arstechnica.com/security/2013/03/how-i-became-a-password-cracker/>
[2]: <https://twitter.com/BritishGasHelp/status/463619139220021248>
