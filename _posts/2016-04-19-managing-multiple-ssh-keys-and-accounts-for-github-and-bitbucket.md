---
layout: single
title: Managing Multiple SSH Keys and Accounts for Github and Bitbucket
date: 2016-04-19 12:00:00
tags: [git, ssh, keys, security]
---

This is an error that gets me every time and I always forget what the solution
is. The easiest way for me to remember how to solve it is to write about it. 

**Scenario:**

You have a personal account on either Github or Bitbucket
(I'll just refer to both as Bitbucket from here on; partially because it's
 easier, but also because I've only really tested this on Bitbucket, but I
 assume Github will be the same.) and you land yourself a nice
new job. For this job, you will be using Github as well, but since you don't
want to mix your professional and personal life, you create two separate Github
accounts. Now, for authentication, you prefer to use SSH keys. The problem is
that when you go and paste in the contents of `id_rsa.pub` into the new public
key text box, you get an error telling you that it is already in use with a
different account. 

"No problem" you think to yourself. You go and generate a new key, remembering
to call `ssh-add` and you paste this into the text field. Github accepts the new
key and everything is good in the world. 

Then you go and clone a repo in your new team, but when you try you get an
error:
    
    conq: repository access denied.

What's the deal? You assume it's some key issue so you start to debug, running
things like:

    ssh -T -v git@bitbucket.org

That works quite happily, so it's not a problem with the keys then...

Hang on, do we really know that it's not a problem with the keys? Well, no not
really. It works for calling `ssh`, but does `git` know what to do with multiple
keys? The answer appears to be no. I'm not sure entirely why, but it seems to
just offer up the first key and then stop. 

**Solution:**

Fortunately, there is a solution, even though it is a little annoying to deal
with. 

First up, ensure you have a file called `~/.ssh/config`. If it doesn't exist,
      create it. In here you are going to add entries for the service where you
      have conflicting accounts. For example:

    Host personal_account
      HostName bitbucket.org
      IdentityFile ~/.ssh/personal_private_key
    Host work_account
     HostName bitbucket.org
     IdentityFile ~/.ssh/work_private_key

Those names `personal_account` and `work_account` can be substituted for anything that makes sense to you. 
Personally, I just use the usernames for the accounts. 

Now, when you are setting up the remotes, for instance when cloning, you are
going to replace the hostname with the alias you chose. e.g.

    git clone git@bitbucket.org:work_username/work_project.git

will become

    git clone git@work_account:work_username/work_project.git

What this does is change the hostname so that the keys have to be looked up.
Since the keys are specified in the `config` file, it always looks up the
correct key. 

If you already have the repositories set up and are now failing because of
multiple accounts, you just need to change the remote values with:

    git remote remove origin
    git remote add origin git@work_account:work_username/project.git


