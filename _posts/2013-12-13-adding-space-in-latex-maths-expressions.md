---
layout: post
title: Adding space in LaTeX maths expressions
date:   2013-12-13 17:44:01
categories: posts
---
            
So this is a simple one which is used everywhere. Essentially I was trying to express some run times in LaTeX using the following:

    O(n log(n))

However, this displayed as:

    O(nlog(n))

The solution is to add some horizontal space using real units as follows:

    O(n\hspace{2 mm}log(n))

I found 2mm to be about right when using `pdflatex`, but feel free to play with it your self. 
