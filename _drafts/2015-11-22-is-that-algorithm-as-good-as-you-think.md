---
layout: post
title: Is that algorithm as good as you think?
date: 2015-11-22 12:00:00
tags: [algorithms, software, hardware]
---

We've all been there. We've all spent time working feverently, only to sit back
with a sigh and admire the wonder that we just created. Sometimes you are going
for speed, sometimes for memory, sometimes just for style, but you always have
an end goal in mind. Let's ignore the style option for now and have a think
about the other two. 

Memory can be tricky if you don't really understand it. Sure, over time we all
hear the stories of someone storing ten million elements in memory, each one
being represented by a struct that looks like this:

    struct some_type {
        char first;
        double second;
        short third;
        int fourth;
    }

The error occurs in thinking that you are just going to use 15 bytes for that
struct since that's the number you get when you do `sizeof(char) +
sizeof(double) + sizeof(short) + sizeof(int)`. By now, everyone hopefully
realises that types are aligned differently in memory and your struct will be
larger than you expect. For more information have a read of ["The Lost Art of C
Structure Packing"](http://www.catb.org/esr/structure-packing/). But what about
speed? What gotchas are hiding out there?

Let's have a quick reminder of the types of things which can kill an algorithm
in terms of speed. 

1. CPU Stalling (Which produced
   [this](http://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-an-unsorted-array?lq=1)
   infamous StackOverflow thread)
2. Not caching
3. Locking unecessarily
4. Lack of parallelism

There are certainly others, but these are just the ones that immediately came to
mind. #2, #3 and #4 in that list are completely due to the implementation of the
algorithm, but #1 strays into a much more grey area. So, I ask, how much should
a developer know about the hardware that their algorithm will run on? 

It could be argued that in terms of CPU stalling, a developer must always be
aware that this is an issue and should take action to avoid this happening.
However, when we start paying attention to stalling, we end up trying far too
hard to avoid branch misprediction. This inevitably leads to this:

    return a > b ? a : b;

becoming:

    return a - ((a - b) & (a - b) >> 31);

Ok great, you have a nice branchless max function. That will speed up your
algorthm now right? Well, no. Not necessarily. Remember, it's 2015. Compilers
are good. In fact, compilers are better than you. Write what you want the
program to do and let the compiler figure out these details. (Of course there
are exceptions to this, but don't worry about those for now)

So when should you really worry about the hardware you are running on? Answer:
When you are accessing the disk. 

Back at university I had a database systems course, and it was probably the most
interesting course I took. Not to mention the fact that it's probably the most
useful one too, despite the fact that I don't write database systems. There was
a lot useful information about algorithm design which was applicable to all
areas of software development. The one that really stuck with me though was the
implementation of an external merge sort. 

Let's imagine you have a very large file. This file is 16TB in size and contains
random integers. You have a nice shiny computer with 16GB of RAM and you have
been asked to sort the file. How do you do it? 

The naive way is to load in 16GB of data, sort it in place, then write it out.
Repeat for the next 16GB, and keep going until you have done this to the whole
file. Now you have 1000 sorted 16GB files. Perform a simple n-way merge (in this
        case n will be 1000) and write out. You now have a 16TB sorted file.
This is the simple way to do it, but lets think more low level. 

When you load data from disk, it will come in page by page. So let's imagine a
new scenario, with less memory and a bigger file. Let's say you have a file
which is `N` pages long and you have `B` pages of space in RAM. This time, you will
read in `B` pages, sort the data and write out, repeating these steps for a total
of `ceil(N/B)` runs. Now you need to merge. You can only fit `B` pages in memory
at any one time, so that means you can only merge `B` of these sorted files at any
one time. That means that your merge step will now take much longer. You now
have to merge `B` files, repeatedly until you are down to a single file. The run
time of this is now approximately `1 + ceil(log<sub>B-1</sub>(ceil(N/B)))`. Let's apply some
numbers to this to get an idea of the growth (the numbers from `B` seem odd, but 
they seem to be used in every example I can find of this algorithm so I went with them too. 
Hopefully it is obvious that they are powers of 2, plus 1.). 

<table>
    <tr>
        <th>N</th><th>B=3</th><th>B=5</th><th>B=9</th><th>B=17</th><th>B=129</th><th>B=257</th><th>B=4097</th>
    </tr>
    <tr>
        <td class="rowhead">100</td><td>7</td><td>4</td><td>3</td><td>2</td><td>1</td><td>1</td><td>1</td>
    </tr>
    <tr>
        <td class="rowhead">1,000</td><td>10</td><td>5</td><td>4</td><td>3</td><td>2</td><td>2</td><td>1</td>
    </tr>
    <tr>
        <td class="rowhead">10,000</td><td>13</td><td>7</td><td>5</td><td>4</td><td>2</td><td>2</td><td>2</td>
    </tr>
    <tr>
        <td class="rowhead">100,000</td><td>17</td><td>9</td><td>6</td><td>5</td><td>3</td><td>3</td><td>2</td>
    </tr>
    <tr>
        <td class="rowhead">1,000,000</td><td>20</td><td>10</td><td>7</td><td>5</td><td>3</td><td>3</td><td>2</td>
    </tr>
    <tr>
        <td class="rowhead">10,000,000</td><td>23</td><td>12</td><td>8</td><td>6</td><td>4</td><td>3</td><td>2</td>
    </tr>
    <tr>
        <td class="rowhead">100,000,000</td><td>26</td><td>14</td><td>9</td><td>7</td><td>4</td><td>4</td><td>3</td>
    </tr>
    <tr>
        <td class="rowhead">1,000,000,000</td><td>30</td><td>15</td><td>10</td><td>8</td><td>5</td><td>4</td><td>3</td>
    </tr>
</table>

With `N` being 1,000,000,000 and a standard page size of 4MB, that's 3.8PT of data. Obviously keeping passes of
this to a minimum would be great. Note that with `B` set to 4097, that's approximately 16 GB of memory, which is about what you
might expect with todays hardware. 

Once we apply disk seek times to this though, it gets a little crazy. If we say that it takes 5ms to read or write a page somewhere on disk
then we get the result for `N = 1,000,000,000 and `B` = 3 as 300,000,000 seconds. That's about 9 and a half years. With `B` = 4097 its a 
little better at about 11.5 months. Still extremely slow though. For a more normal amount of data of 400GB and 16GB of RAM it will take 
approximately 5.5 hours. Still, it would be nice to avoid that if we can. So how do we do it?

Well, the factor which destroys us is the disk reads and writes. The equation I used to calculate the cost takes 2*`N` * #passes * disk cost. 
Clearly we have to do what we can to get the number of passes down to as little as possible since we can't shrink the file or the
disk cost. So far we thought we had the optimal method of doing this, and from a purely computational standpoint that may be correct. 
We have just ignored the hard ware and it has come back to bite us. If we had paid attention to the hardware and it's costs then
maybe we could make a better algorithm?

So, let's see how we can get the number of passes down. We know accessing disk is slow. We know processors are fast. 
Ok, let's trade processor operations for disk accesses. How do we do that? We use a heap. How does that work?

This time, we aren't going to read in our B pages, sort, write and repeat as before. This time we keep two heaps in memory. 
One is for the current run and one is for the next run. Remember, the size of both of these heaps together must fit in
B pages of memory. Here is the algorithm:

    Loop while there are more values to be read:
        Read in until the current heap is full  
        Loop while we have entries in the current heap and we fit in memory:
            Get the lowest value in the heap
            Write it out
            Get the next value
            If the next value < the lowest value:
                Write to the current heap
            Otherwise:
                Write to the next heap
        
        Write out remaining values on current heap (if any)
        Swap the heaps

It has been proven that this results in average run lengths of `2B` which is twice as good as we had before!


It's rather counter intuitive to do extra processing that is unecessary to sort data, however sometimes it is necessary. 
When creating an algorithm of any kind, always take into consideration the hardware it will run on. I'm not just saying think 
about a desktop vs a laptop, I'm talking about the fundamentals of computer hardware. Remember that RAM exists, that we fetch 
data from disk, that network delays occur, etc. It's all to easy to treat the computer as a magical box. I call `fopen` and 
suddenly I have data. Behind the scenes there is a lot of work being done, and every programmer out there should be aware that it is happening.
