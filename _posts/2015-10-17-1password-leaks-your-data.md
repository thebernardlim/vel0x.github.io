---
layout: post
title: 1Password Leaks Your Data 
date: 2015-10-22 14:19:00
tags: [software, security, passwords, password managers, 1Password]
---

Seriously. 

For those of you who don't know, [1PasswordAnywhere](https://support.1password.com/guides/mac/1passwordanywhere.html) is a feature of [1Password](https://agilebits.com/onepassword) which allows you to access your data without needing their client software. 1Password originally only used the "[Agile Keychain](https://support.1password.com/agile-keychain-design/)" format to store their data (not including when they were OS X keychain only). This format basically stores your data as a series of JavaScript files which are decrypted when you supply your master password. Since the files are JavaScript and implementations of various crypto algorithms exist in JavaScript, there was no reason why AgileBits couldn't come along and make a HTML and JavaScript client for viewing your data, so they did. 

If you browse to your .agilekeychain "file" on disk, you find that it is actually a directory. Inside this directory is a file named "1Password.html". If you access this file over HTTP (note that using the file protocol won't work), you will be greeted with a grey page which has a lock image and a password field. Enter your password and your keychain will unlock and you have a read only view of your data. 

So what's the problem? Well, it turns out that your metadata isn't encrypted. I discovered this after having a sync issue with [Dropbox](https://www.dropbox.com/referrals/NTI2ODM1MTA5?src=global9) (I use Dropbox to host my keychain). The file that had issues was `1Password.agilekeychain/data/default/contents.js`. Being a curious kind of guy I opened the file to see what was in there. The answer is the name and address of every item that I have in 1Password. Every single one. *In plain text*. 

For those of you thinking "So what?", perhaps you have nothing of interest in there, but there are other considerations. Perhaps I signed up for `somespecificpornsite.com` and this isn't something I want to broadcast. However, I've done just that. Anyone who knows the link to the main log in page for my keychain can just alter the link and get this file. They can go through and find out exactly what shady sites I have accounts on, what software I have licences for, the bank card and accounts I hold, the titles of any secure notes I have, any anything else I've decided to store in there. 

The second concern, and possibly larger concern, is that the login location is
stored with the entry title. In other words, if I sign in at `https://example.com/login`
then that is stored with the keychain entry. In 99% of cases this isn't an issue. It's
the 1% of cases which are a concern. Developers aren't perfect. We make bad
decisions and sometimes dangerous ones. Recently I signed up with a large ISP in the
UK and had to reset my password due to a bug on their system. I was sent an
email with a reset link in the email. I click the link, enter a new password,
and press submit. At this point two things happen. The first is that my password
is reset. The second is that 1Password prompts to save my credentials. Since I
used an auto-generated password and I like to keep my passwords secure, I click
save. Now my new password is stored in my keychain. What if my ISP made a
mistake with that email link though? Maybe they made the mistake that is all too
common... So I go back to my email and click the password reset link again. Sure
enough, I get prompted with a screen where I can reset my password again. They
didn't check to see if I had already used the link. And now that link is stored
in my 1Password metadata, publicly accessible. Anyone can go and paste this link into
their browser and they have full access to my account. Presumably I don't need
to explain any more about how that is a huge issue?

But it gets worse. I decided to have a look and see just how bad things were. Thanks to people having links for easy access to their keychain on their websites, Google has indexed some of these. A simple search brings up results. By looking at one of these it was a simple matter to identify the owner of the keychain and where he lived. I know what his job is. I even know the names of his wife and children. If I was malicious, it would be easy to convince someone that I had compromised their account and had access to all of their credentials. Not to mention the fact that they have revealed their location online which may put their personal safety at risk. 

So what did I do? Well, immediately I tried to reach out to AgileBits to make them aware of this data breach. When I received a response from one of their engineers I was given a few links to the details about their keychain and the assurance that not only were they aware of this breach, but it was *by design*. 

> When we built the keychain, were aware that it would be possible to see that a user has logins across different sites when it is unencrypted; while this might have some privacy implications if an attacker gained access to it, your passwords are never exposed or shared as they are encrypted by your Master Password and they do not appear in the keychain in any way.

Searching through the forums I found claims by employees that it was designed this way for performance reasons. The logic was that if the metadata was encrypted with the regular data, then when the user unlocked their keychain, if they wanted to search for an entry, all entries would have to be decrypted to find what they were looking for. And that is correct. What I didn't understand, and asked AgileBits, was why not just encrypt the metadata file using the master password in some fashion, then they only have a single file to decrypt? Again, their reason was performance. 

> When we first developed AgileKeychain a few years ago, 1Password had significantly less processing power with which to function, and decrypting the keychain on the fly to do something as simple as a login search incurred huge performance penalties for our users. Because this provided a poor experience for our users, we decided against requiring extra decryption steps for this process. 

So what do we do now?

Well, in December 2012, AgileBits changed the format of their keychain from the Agile Keychain, to [OPVault](https://support.1password.com/opvault-design/). So how is this new format? Well the first thing is that you cannot use 1PasswordAnywhere with this format any longer. Since the data is stored as JSON, I can't figure out why, but perhaps AgileBits decided having your keychain accessible on the internet isn't the best idea? The metadata is significantly better than the Agile format, but it has one significant problem. If you create an OPVault on OS X, you **must** enter a password hint. This password hint is still stored in plain text (see example below). Sure you can also see the type of the stored items and when they were updated and created, but this could be easily determined by monitoring the keychain and so is inconsequential. Furthermore, 1Password does not create the OPVault by default. It still sticks with the Agile Keychain with no information about the security risks. In fact, convincing 1Password for OS X to even allow you to use the OPVault format can require the use of the command line: <https://discussions.agilebits.com/discussion/39875/getting-your-data-into-the-opvault-format> 

Let me summarise: Do not use the Agile Keychain format. It leaks your data. If you are using it, [convert it to the OPVault format immediately](https://discussions.agilebits.com/discussion/39875/getting-your-data-into-the-opvault-format). 

I've used 1Password for a few years now. In that time I've watched password managers rise and fall due to security holes. 1Password stood solidly against this onslaught and I stood by it. After this my confidence has been shaken. However, I will continue to use 1Password. Sure, there are problems with metadata. I'll now have to switch to the OPVault format and lose the 1PasswordAnywhere functionality. Yes, they didn't broadcast the downsides of using the older vault format and they make it difficult to use the new one by default. However, they didn't ever deny this. They clearly document their keychain formats to the extent that anyone can go and write something which can decrypt it. I didn't read the documentation when I signed up. I admittedly shouldn't have to, but it's there. AgileBits dropped the ball on metadata security, but I have no worries at all that my passwords are still safe. 

