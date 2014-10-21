---
layout: post
title: Digraphs and Trigraphs in C/C++
date: 2014-09-06 12:00:00
tags: [programming, C, C++]
---

Recently a mention of the proposal to remove trigraphs from C++1z[^proposal] (the version
after C++14) caught my eye. While I was aware of the existence of these,
although I must admit that this was a recent encounter, many people were not. 
The best thing to do, I figured, was to explain about these old character
sequences. 

Digraphs and trigraphs originated in ANSI C (although other languages may have had
support for variants before this). They exist due to the difficulty of typing
certain characters in various keyboard layouts and character sets. The original
spec decided that extra support was needed for the following characters:

    #\^[]|{}~

So what are trigraphs then? Trigraphs are an alternative method of representing
these characters. Each trigraph begins with two question marks and is then
followed by a 3rd character. For the above characters the mapping is as follows:

<table>
<tr><td><b>Character</b></td><td><b>Trigraph</b></td></tr>
<tr><td>#</td><td>??=</td></tr>
<tr><td>\</td><td>??/</td></tr>
<tr><td>^</td><td>??'</td></tr>
<tr><td>[</td><td>??(</td></tr>
<tr><td>]</td><td>??)</td></tr>
<tr><td>|</td><td>??!</td></tr>
<tr><td>{</td><td>??&lt;</td></tr>
<tr><td>}</td><td>??&gt;</td></tr>
<tr><td>~</td><td>??-</td></tr>
</table>

So how do they work? Basically the C preprocessor runs and converts all of these
matching character sequences to the corresponding equivalent character before
the program is compiled or even any other preprocessing. Let's have a look at a simple surprise you may find:

    int main(){
        printf("Trigraphs do what??!\n");
    }

Usually we would expect to see the string `Trigraphs do what??!` printed out followed by a
new line. In this example though, the `??!` sequence is converted to a `|` and
instead we get `Trigraphs do what|`. Sometimes you really want these sequences
in your code though. If that is the case, just escape the questions marks with a
backslash and the preprocessor will leave them alone. 

In case you want to try this, you should be aware that gcc ignores trigraphs, but support may be enabled using the `-trigraphs`
option. 

So what about digraphs? Well, from what I can tell, someone decided in time for the C99
spec that trigraphs were just too difficult and used so much that it would be better to 
make them easier to read. As such they came up with 5 new two character versions
of some of the characters above. These are:

<table>
<tr><td><b>Character</b></td><td><b>Digraph</b></td></tr>
<tr><td>#</td><td>%:</td></tr>
<tr><td>[</td><td>&lt;:</td></tr>
<tr><td>]</td><td>:&gt;</td></tr>
<tr><td>{</td><td>&lt;%</td></tr>
<tr><td>}</td><td>%&gt;</td></tr>
</table>

With the digraphs (which are replaced at the tokenization unlike trigraphs) and 
trigraphs we can have some weird and wonderful creations such as the following 
C++ program:

    int main() {
        <:]{%>; //This becomes an empty lambda
    }

or this correctly formed block comment:
  
    /??/
    * A comment *??/
    /
    
One of the useful uses of trigraphs are the additional ones added into C++.


<table>
<tr><td><b>Token</b></td><td><b>Equivalent</b></td></tr>
<tr><td>%:%:</td><td>##</td></tr>
<tr><td>and</td><td>&&</td></tr>
<tr><td>bitor</td><td>|</td></tr>
<tr><td>or</td><td>||</td></tr>
<tr><td>xor</td><td>^</td></tr>
<tr><td>compl</td><td>~</td></tr>
<tr><td>bitand</td><td>&</td></tr>
<tr><td>and_eq</td><td>&=</td></tr>
<tr><td>or_eq</td><td>|=</td></tr>
<tr><td>xor_eq</td><td>^=</td></tr>
<tr><td>not</td><td>!</td></tr>
<tr><td>not_eq</td><td>!=</td></tr>
</table>




<http://en.wikipedia.org/wiki/Digraphs_and_trigraphs>

[^proposal]:<http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3981.html>


