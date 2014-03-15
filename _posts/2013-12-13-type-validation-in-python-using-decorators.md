---
layout: post
title: Type validation in Python using decorators
date:   2014-01-28 12:15:57
categories: posts
---

So in Python you generally do test driven development to make sure that you don't get weird type errors. Sometimes, however, it's good to be alerted immediately, so I've made a small decorator which can check for types of arguments as follows:

<script src="https://gist.github.com/Vel0x/7085125.js"></script>