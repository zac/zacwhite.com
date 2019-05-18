---
id: 49
title: Circuit Design With Quartz Composer
date: 2006-05-23T14:46:59-07:00
author: Zac White
layout: post
guid: https://zacwhite.com/blog/2006/05/23/circuit-design-with-quartz-composer/
permalink: /blog/2006/05/23/circuit-design-with-quartz-composer/
categories:
  - random
  - software
hidden: true
---
You didn&#8217;t think it would be done, did you? You didn&#8217;t think one person would have enough free time, boredom, and sheer ingenuity to do it, but I did.

The other day I sat down to write an entry to the esteemed contest, [ironcoder](http://ironcoder.org/). This was short lived however, because I got sucked in by a virtually unknown gem of 10.4: Quartz Composer. I was browsing around dragging random &#8216;patches&#8217; into the canvas, when I saw an innocuous line in the patch library.

![]({{ "/" | relative_url  }}assets/images/logic.png) 

This little patch holds the same power that started the computer revolution. Armed with my limited digital design knowledge, I set out to create some well known circuits from within Quartz Composer. I started out building a full adder. This takes three inputs and produces two outputs. When you chain these together, you can make an N bit adder. You have one in your CPU right now, chugging away at thousands of operations a second.

[![]({{ "/" | relative_url  }}assets/images/fulladder-small.png)]({{ "/" | relative_url  }}assets/images/fulladder.png)

This allowed me to string them together to create a 4 bit adder.

[![]({{ "/" | relative_url  }}assets/images/4bit-small.png)]({{ "/" | relative_url  }}assets/images/4bit.png)

This produces the following output:

[![]({{ "/" | relative_url  }}assets/images/4bitoutput-small.png)]({{ "/" | relative_url  }}assets/images/4bitoutput.png)

After this I knew what I had to do. I had to create the foundation of modern computer storage&#8230;the flip flop. This little guy holds a bit (binary digit) indefinitely. If you want a fast way to store data close to a processor&#8230;this is the way to do it.

[![]({{ "/" | relative_url  }}assets/images/flipflop-small.png)]({{ "/" | relative_url  }}assets/images/flipflop.png)

CPUs have things called registers that are basically a bank of flip flops with some logic to do different operations. So my next task was to create something like that. Because of a limitation with Quartz Composer (it can&#8217;t do recursion), I had to not implement some features I wanted to. I ended up with a [parallel load register](http://www.cs.umd.edu/class/spring2003/cmsc311/Notes/Seq/parload.html) that could &#8216;hold&#8217;, &#8216;load&#8217;, and &#8216;clear&#8217;.

[![]({{ "/" | relative_url  }}assets/images/register-small.png)]({{ "/" | relative_url  }}assets/images/register.png)

I then plugged that register into a small test flow that would print out the contents.

[![]({{ "/" | relative_url  }}assets/images/registertest-small.png)]({{ "/" | relative_url  }}assets/images/registertest.png)

And it produces the following output when you hold the load line high on the next rising edge of the clock:

[![]({{ "/" | relative_url  }}assets/images/registeroutput-small.png)]({{ "/" | relative_url  }}assets/images/registeroutput.png)

So there you have it. The escapades of a bored college student. Mess around with the following stuff and email me (zacwhite at gmail.com) or IM me (cubeman) if you make some modifications.

<p class="download">
  <a href="{{ "/" | relative_url  }}software/downloads/4bitadder.qtz.zip">Download 4bitadder.qtz.zip</a><br /> <a href="{{ "/" | relative_url  }}software/downloads/flipflop.qtz.zip">Download flipflop.qtz.zip</a>
</p>