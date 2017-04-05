---
layout: single
title: The Joel Test for 2017
date: 2017-04-04 19:00:00
tags: [joel test, software, development]
---

Back in 2013, I took a course on "Software Architecture, Process, and Management". One of the topics which came up was: How do you rate a development team? It's not a simple challenge, and to get a full answer would take a considerable investigation and a lengthy report at the end. Thankfully, Joel Spolsky came up with a simple 12 question test to make this process relatively painless, named "[The Joel Test](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/)". The test isn't perfect, and doesn't claim to be, but what it does give you is a solid basis to work from to find out those last few details.

    1. Do you use source control?
    2. Can you make a build in one step?
    3. Do you make daily builds?
    4. Do you have a bug database?
    5. Do you fix bugs before writing new code?
    6. Do you have an up-to-date schedule?
    7. Do you have a spec?
    8. Do programmers have quiet working conditions?
    9. Do you use the best tools money can buy?
    10. Do you have testers?
    11. Do new candidates write code during their interview?
    12. Do you do hallway usability testing?

The beauty of the Joel test is that in a relatively small number of yes or no questions, you can get the answers you are looking for. There are no exceptions or caveats that need to be taken into account, it's purely yes or no to each question and you have your results. A score of 12 is perfect, 11 tolerable and 10 or less is a failure. This beauty did not go unnoticed by me or by a few of my fellow students. I quickly used it to gauge the quality of companies I was interviewing at for internships and later for my first job out of university. Most employers had never heard of the test, which wasn't surprising. What _was_ surprising was how many companies, which I had previously though were good, failed the test. 

Was I getting the questions wrong? Was I not making them clear? Maybe I just couldn't count properly all the way to twelve? 

The test which I had been told was effectively the holy grail of questions in response to "Is there anything you'd like to ask us?", was effectively a failure to me. Sure, it might just be that all the companies I interviewed at were terrible, but despite being new to the software world, I had a feeling that wasn't the case. While I did take the test into consideration for my various applications, it was never a deciding factor for anywhere. 

So what went wrong with the Joel test for me? 

Each programmer is different. They will have different views about what is good and what is bad, and these views will change over time. The Joel test tries to be unbiased in this and asks objective questions, rather than subjective ones. No programmer out there is going to disagree that tracking bugs is a good idea. But what about some of the other questions? 

Created in 2000, the Joel test is now 17 years old. The test is older than OS X. When it was created, Windows 2000 was state of the art, the Playstation 2 had just been released, and Pentium III chips were whizzing along at 1GHz. In the computing world, it was an eternity ago. As power of computer processors (and generally hardware) was doubling every 18 months, our development processes were improving too. The questions in the Joel test reflected the epitome of software development at the time. In 2017 though, they just aren't quite as relevant. 

Taking question #1 as an example:

> Do you use source control? 

At the time this was written Git, today's most popular VCS, didn't exist. Well sure, we all know that Git didn't appear on the scene until 2005. What's easy to forget is that Subversion, the previous King of the VCS world, wasn't released until October 2000. That's 2 months after the Joel test was originally published. Source control existed, but it was in no way as common as it is today. Does that mean that we should even bother asking this question any more? Perhaps. Should we modify the question to better suit today's world? Probably. 

Let's take a look at each question in turn to see how it fits into today's world. 

### 1. Do you use source control?

Despite picking on it above, this question still seems relevant in today's world. All teams _should_ use source control, there is no reason not to. Today's debates revolve around whether we should be using a distributed VCS or not. Normal projects can use Git or Mercurial (as examples) just fine. Larger companies with enormous repositories find that they need to use a centralised version control system. There isn't a right or wrong answer to this. What's important is that version control is used. However, given that this is so common, I'm not convinced that this question has a place today. 

*Proposed update:* Remove rule.

### 2. Can you make a build in one step?

Joel clarifies this one by stating:

> By this I mean: how many steps does it take to make a shipping build from the latest source snapshot?

The reasoning behind it still rings true today. However, I do feel that this is our first contender for an out of date question. I'd propose that making a build should be done in _zero_ steps. Continuous integration now exists and and is ubiquitous. It is wise, and indeed sane, to use it. The process of creating a ship build should be that they commit their code, and then when management, or whoever is responsible, decides to release, they press a "release" button, and that's it. All developers need to do is make sure their code is committed and the CI server takes care of the rest. 

CI has brought the development industry forward by leaps and bounds. It can carry us even further if we all use it, and use it to its fullest potential. 

*Proposed update:* Are all builds handled automatically by a Continuous Integration server? 

### 3. Do you make daily builds?

This follows on with #2. Daily builds are fine if that's what works for your team. I'd say that daily should be the minimum bar though. A better approach, in my eyes, is to make sure that builds are created for each commit to your main development branch. This is a sort of "bleeding edge" kind of build, but it means you will find your issues faster, and more effectively since it's easier to pin down the version when something broke. 

This build should be done by your CI. There is a second factor to this though. Not only should you make daily builds, but you should _use_ them as well. This is, admittedly, harder when you work on software which isn't related to your day to day life (either personally or professionally). In the case where you write an accounting app, or a controller for a rocket, it's unlikely that you will need to use these products. However, I think it's a good idea to sit down with your software for at least 20 minutes a day and just use it, making sure that everything still works. In the case where you do work on something you can use[^using_outlook] day to day, this is even better. It fulfils the same role as a 20 minute session, but in practice, you are going to do a lot more than 20 minutes of testing, and will expose the project to more real-world scenarios. 

*Proposed update:* Do you make and use daily builds? 

### 4. Do you have a bug database?

Well of course you should use a bug database of some kind. Have you ever met a developer who thinks that you shouldn't? Where this one changes is that today our bug trackers are a lot more fully featured. No longer are they just simple interfaces to a table in a database, they are full featured planning and management applications, often with a key distinction between tasks and bugs (a sneak peek at #5). 

When you create a bug today, you give it a title, description, repro steps, affected platforms, etc. just as you've always done. Where it goes differently, is that now, management are going to go through your bugs, assign them to sprints, you are going to move things around on a Kanban board, your Scrum master is going to have conversations with you about difficult tickets, etc. 

These issue trackers are no longer just about keeping a record of _what_ needs done. They are about who does it, how it affects the team, what effect it has on the product and a hundred other variables. Now, while I believe you should have some sort of software that can do this, it isn't essential that it is one and the same with your issue tracker. Nor is it something which gives a simple yes or no answer due to the fact that every team is different. The only minor change I'd make, is to word it slightly differently to reflect the capabilities that we have today. 

*Proposed update:* Do you use an issue tracker?

### 5. Do you fix bugs before writing new code?

Joel has a great explanation of why this is so important, and 17 years on, it still holds true. 

*Proposed update:* N/A

### 6. Do you have an up-to-date schedule?

As a developer, you might not care so much about this. Why does it matter if your team doesn't meet your deadline? It wasn't your fault? Leave this to management.

Leaving it to management might work, but it probably won't. Today, with the various different agile methodologies we have, getting estimates from developers, or at least someone who is capable of calculating a reasonable estimate, is one of the key factors in planning. It helps prioritise what the team is going to work on, who is going to do it, and when they are going to do it. You can stick your fingers in your ears and pretend that it has nothing to do with you, but at the end of the day, each and every developer has a responsibility to the team. 

Does it still hold today though? Definitely. Sure, we don't use waterfall any longer, and making sure we hit that testing phase on time doesn't make much sense any longer, but even in agile (which lots of people believe to mean "making it up as we go along"), and indeed, _especially_ in agile, the schedule is the master and needs to be kept up to date. The difference today is that the schedule can change to accommodate new information. 

*Proposed update:* N/A

### 7. Do you have a spec?

While the schedule rule may have passed through unscathed, the same thing can't be said for the spec unfortunately. In the waterfall days, the spec was crucial as you needed to know what you were building. If you didn't you couldn't allocate resources effectively, and your project was, effectively, doomed to failure. 

Back in 2001, when you created your program, whether it be for a coffee maker, a rocket, or a just a straight forward CRUD interface, you knew what had to be done, and how much would it would take to do it approximately. Developers, testers and PMs were assigned, they worked through it all, tested, and shipped, before moving on to the next project. Today though, things are a little different in a lot of cases. Sure, if you are making any of the above, things are still similar, but what about if you are writing a Twitter client? Or a text editor? Maybe even an email client? Are any of these ever "done"? Usually not. 

A spec works when you can get all the information up front, and plan things out. When you are working on a continual piece of software, the spec is just as important for your MVP, but after that it is effectively useless. In order for the spec to be as important in today's world, you need to make sure that you are continually taking into account the latest information and adjusting your process and product accordingly. 

This information can take many forms. It might just be simple telemetry about what people are doing with the app, or it could, and possibly should, be more advanced, such as the results of A/B testing new features and designs. 

*Proposed update:* Do you have up to date information on your products performance and usage?

### 8. Do programmers have quiet working conditions?

Images of software development in the 80s and 90s bring to mind a sea of cubicles. Today, the result is pretty similar, the only difference is that the dividers are often much smaller, or missing entirely, in the interests of having an "open" office. 

Personally, I'm not a fan of the open office. I like having an office of my own where I can have dedicated development time, I can personalise it how I like, and I can turn up my music when I want to get into the zone. However, not everyone agrees with me. I know many people who like the idea of open offices. They promote more effective and direct communication between members of a team, often leading to faster and better solutions to problems. 

The rule though asks about quiet working conditions, not solitary ones. Open offices, by their very nature, tend to be noisier than having your own office. When asked about the noise issues of open offices, the usual response is to just wear headphones. With noise cancelling technology, this is a reasonable fix in most cases, and the one I use often when I'm in a shared office.

Like it or not, quiet working conditions are generally tied to having your own office vs having an open office. There is no right or wrong answer for which is correct. 

*Proposed update:* Remove rule

### 9. Do you use the best tools money can buy?

This rule was the hardest to come to a decision on. I actually ended up writing most of the rest of this post before coming back and deciding on this one. 

The issue that I had with it is that often, companies can't afford the best tools money can buy. Just because they can't today, doesn't mean they won't be able to tomorrow. However, I think the spirit of the rule still stands. It's the duty of the company to make the lives of the developers as easy and as happy as possible. Happy and unstressed developers product better code.

One key point though to remember about this is that this does not mean that money needs to be thrown at the problem. If the best tool is free, then that's the one that should be used. 

*Proposed update:* N/A

### 10. Do you have testers?

Testers are an area that every company does differently. A couple of years ago, between when I was an intern and when I went full time, Microsoft [merged the engineer and tester roles](https://www.quora.com/Why-is-Microsoft-merging-its-SDE-and-SDET-positions-into-the-software-engineer-title). I originally wrote about why I thought that merging testers and developers was a good thing due to the resources I wasted as an intern, and how removing the safety net of testers encouraged me to write better code. Now, that was true for Microsoft, but let's look at what Joel has to say about this:

> If your team doesn’t have dedicated testers, at least one for every two or three programmers, you are either shipping buggy products, or you’re wasting money by having $100/hour programmers do work that can be done by $30/hour testers.

The main difference between how I viewed this and how Joel does is that at Microsoft, testers and developers were equal. Developers weren't getting paid 3 times as much as testers. The time a tester spent going over changes, and making sure fixes worked, and didn't introduce problems, was just as valuable as a developers time. However, it still felt like a safety net. 

Having testers is a great thing, but Joel makes the best point, and that is make sure you aren't wasting the value of your testers. 

Developers need to take on the responsibility for their own code. If you aren't very confident that you code is bug free, it shouldn't be checked in. That means you need to run manual tests, unit tests, integration tests, etc. You aren't going to be checking in code without going through code review, so why check in without going through tests? 

Testers don't need to go through each and every change, but they do need to make sure the product as a whole is still stable. They should be able to spend their time going through a test matrix that is far larger than would be worth a developer doing. Having testers isn't they key, having a comprehensive and efficient test plan is.

*Proposed update:* Do you have a comprehensive test plan?

### 11. Do new candidates write code during their interview?

If you were to search for the keyword "interview" on various development sites, such as Hacker News, or some sub-reddits, how many of these resulting articles would have something positive to say about the process? Very few. Everyone is aware that the current interview system is broken and we need a better one. Until the time that someone can come up with one, assuming that a single one exists, which is unlikely, we need to work with what we have. 

There are a few different ways of handing interviews currently:

1. Solve some problems on a whiteboard
2. Do this homework project.
3. Come and work with us for 6 months on probation. 

The thing that they all have in common is that the developer is expected to write code. If that's what their job is going to be, that's what you should test them on. Don't penalise them for getting the syntax of a particular operation wrong, or not choosing the language that your team works in, but make sure they can write code. 

As we have seen before though, this one is so ubiquitous that I don't think it's worth including any longer.

*Proposed update:* Remove rule.

### 12. Do you do hallway usability testing?

As one of the lesser known rules, let's take a look at Joel's definition:

> A hallway usability test is where you grab the next person that passes by in the hallway and force them to try to use the code you just wrote. If you do this to five people, you will learn 95% of what there is to learn about usability problems in your code.

Now, you aren't going to hear any argument from me that hallway usability testing is a good thing, but things have come a long way from four thousand grey buttons on a grey tool bar, in a grey window on a grey desktop. People who use computers (which has a very wide range of definitions these days), aren't necessarily well versed in how they work. For specialised tools, having what you need at your finger tips with buttons, drop-downs, keyboard shortcuts, etc. is still great. For Facebook however, your grandparents need to be able to use it. It needs to have a clear design, with high contrast between elements, have interaction points located in sensible places, and be suitably sized. 

Creating a user interface and experience of any kind, and by that I don't just mean the regular UI, but making sure things like saving a file are handled in a sensible manner, is no longer something that any developer can do, or should be expected to do. It is now a specialised skill, which should be handled by dedicated UI and UX designers. 

*Proposed update:* Do you have dedicated UI and UX designers?

## Review of the Joel Test

So, let's see what our test now looks like:

    1. Are all builds handled automatically by a Continuous Integration server? 
    2. Do you make and use daily builds? 
    3. Do you use an issue tracker?
    4. Do you fix bugs before writing new code?
    5. Do you have an up-to-date schedule?
    6. Do you have up to date information on your products performance and usage?
    7. Do you use the best tools money can buy?
    8. Do you have a comprehensive test plan?
    9. Do you have dedicated UI and UX designers?

We've cut a few rules, and improved others. But we aren't done yet. This was how we can change the existing test to be more suitable for modern development, but we need to add in new rules that matter today. 

## New Rules

### Does all code go through code review?

Everyone makes mistakes. It doesn't matter if you are fresh out of school or a war hardened veteran of software, you will make mistakes. All too often we depend on the compiler to tell us about our mistakes, and that's not how it works. That's why we test. But testing is fallible too. We can't rely 100% on it either. Having a fresh pair of eyes on your code will see things that you simply didn't see. Further more, they can help you make sure your code follows your teams style, which is something your compiler can't do (linters are a topic for a different time). Having consistent code helps others on your team read and use your code. 

### Do you have coding standards? 

This one goes hand in hand with the one above. Writing code is all well and good, but the trick is that you need to be able to read it again. Unfortunately, this isn't always the case, and we should do what we can to ensure that code is as easy to read as possible. The number one way of doing that, is ensuring the code is consistent in the code base. By having a set of standards which the team must adhere to, it makes it much easier to read and understand code, search through the code base for particular components, types, etc., and helps new developers get up to speed by clearly defining the style expected of them, rather then forcing them to figure it out as they go. This can also be paired with a linter in your CI system to enforce the standards. 

### Are new employees given training?

It seems obvious, but it just isn't in lots of places. Many new developers join a company and get thrown in at the deep end. This process is a waste of time for the developer and the company. Some will manage to swim, but most will sink. When you join a company, you should be given the information you require to do your job. Now, I'm not saying that you should be sat down in a conference room for a week to sit through HR sessions and proclamations from VPs, and in fact I hope that this isn't the case, but you should be given the documentation for the system you work on, and have someone already on your team assigned as a mentor for a reasonable time period so that you can get up to speed as quickly as possible. Yes, that means that the other developer will have a lower output during the training session, but after that you get _two_ developers, so anyone who can do basic arithmetic can see that it is a worthwhile investment. 


## Conclusion

So, lets see what our new "Joel Test" for 2017 looks like:

    1. Are all builds handled automatically by a Continuous Integration server? 
    2. Do you make and use daily builds? 
    3. Do you use an issue tracker?
    4. Do you fix bugs before writing new code?
    5. Do you have an up-to-date schedule?
    6. Do you have up to date information on your products performance and usage?
    7. Do you use the best tools money can buy?
    8. Do you have a comprehensive test plan?
    9. Do you have dedicated UI and UX designers?
    10. Does all code go through code review?
    11. Do you have coding standards?
    12. Are new employees given training?

It's perfect! We shall all use it indiscriminately and rejoice.

Except no... The test isn't a panacea. It's just a guide. It's a flag system rather than a thorough evaluation. A company can get a 12/12 score and still be a terrible place to work. However, a company that doesn't get at least 10 is effectively guaranteed to be a terrible place to work. Make sure your potential employers get a high score of 11 or 12, and then use your own judgement. Don't go in blind. 

_P.S. I've had this post sitting in my drafts for quite a while. Yesterday Stack Overflow mentioned that they are [asking similar questions of themselves](https://twitter.com/StackOverflow/status/849352372610584576). Since there isn't a right or wrong answer to this sort of thing (including what I wrote above - it could be totally wrong for you and your situation), I figured I'd get mine out, and hopefully prompt people to start thinking about this so that we can form a better idea as whole industry rather than as a lone developer._


[^using_outlook]: It turns out that working on an email client, bugs tend to be found pretty quickly. Who would have guessed that developers, particularly at large companies like Microsoft, would use email quite often? 

