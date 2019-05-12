---
id: 246
title: What if Adobe generated code?
date: 2010-05-27T01:01:42-07:00
author: Zac White
layout: post
guid: http://www.zacwhite.com/blog/?p=246
permalink: /2010/05/27/what-if-adobe-generated-code/
tags:
  - ""
categories:
  - apple
  - software
---
Wil Shipley had an idea for Adobe:

> If Adobe were smart, they’d modify their Flash &#x25B6; iPhone code to just output Obj-C source code. Not much Apple could do.&#8221;

I&#8217;m sure Adobe is smart. And that&#8217;s why they aren&#8217;t getting into a battle they can&#8217;t possibly win. Let&#8217;s explore a what-if scenario of Adobe going down this path. How would Apple react in this cat and mouse game? Could they create a tool to automatically filter Flash created apps without the sourcecode to your program? 

#### Step 1: Class dump executable.

For those of you that don&#8217;t know, you can get all Objective-C symbols out of a Mach-O binary with a wonderful tool called [class-dump](http://www.codethecode.com/projects/class-dump/). If they aren&#8217;t there, you&#8217;ve already detected it&#8217;s a possible obfuscated build. 

#### Step 2: Search class-dump output.

Look for suspicious symbols. Because anything outputted by a source generating engine is um, source, you can see exactly what pops out of a typical compile. It isn&#8217;t as though Apple can&#8217;t afford a copy of CS5. You can search for &#8220;CS5FlashView&#8221;[<sup>1</sup>](#foot1){#refFoot1} or any myriad of classes/symbols that would be put into the boilerplate output. This necessitates something way more devious as suggested by [rentzsch](http://twitter.com/rentzsch/status/14812033481): [polymorphic code](http://en.wikipedia.org/wiki/Polymorphic_code). 

#### Step 3: Search for patterns.

After Adobe had spent a good chunk of time creating an Objective-C polymorphic coder, Adobe could now distribute a code generation engine that produces binaries that looks like it&#8217;s a home-made animation and application framework. But wait, if all the binaries produce the same obfuscated code, that is trivial to detect. Just look for &#8220;ER0GlashView.&#8221; This necessitates a nondeterministic Objective-C polymorphic coder. A perfect Ph.D topic, but nothing that&#8217;s been done to my knowledge. If it existed, [this contest](http://www.ioccc.org/) would be utterly pointless. 

And you&#8217;re not just dealing with hiding a 200 line C function. Imagine randomizing the following:

  * ##### Class Names
    
    If you produce class names like &#8220;QTi3uigejr&#8221; dictionary attacks can trivially red flag programs.

  * ##### Symbols
    
    It isn&#8217;t enough to generate random symbol names. You have to randomize the structure of your classes so signatures of symbols don&#8217;t resemble the original versions. It would be fairly easy to create a JavaScript like obfuscater which converted your classnames/symbols to meaningless letter combinations. Now imagine running that on two trees of headers outputted from class-dump. One from a known Flash generated app and the other from an unknown. A significant chunk would turn out to be the exact same as the headers would be distilled into their underlying structure. 

  * ##### Class hierarchy
    
    Ok, so now you use perfectly hidden class names, but every class hierarchy produces a recognizable fingerprint for a library. Without knowing anything about the classes, you can see relative relationships between them. The bigger the code-base, the easier this is. I&#8217;d assume the flash support structure is fairly large and its interdependency very complicated. 

#### Prohibitively hard.</p> 

Let&#8217;s say you find an awesome little UIView subclass, but Apple has banned anyone from using it on the store. It wouldn&#8217;t be too hard to get around. Copy/paste, rename some methods, change a few method names around, etc. You then would have plausible deniability and you could say it wasn&#8217;t the banned UIView. 

Imagine me asking you to incorporate [three20](http://github.com/facebook/three20) into your app and obfuscate it in a way where I couldn&#8217;t tell you were using it. And assuming the code-generator produces deterministic output, access to all your source code.[<sup>2</sup>](#foot2){#refFoot2} How hard would that be? You aren&#8217;t just dealing with changing the names of things. You are tasked with changing the fundamental structure of internal APIs and preserving assumptions that were undoubtedly made when making the code work with itself. 

Now imagine making a program that could produce unique obfuscations of [three20](http://github.com/facebook/three20) and in a way that made it look from the outside like it was redesigned and rewritten from scratch by a human.[<sup>3</sup>](#foot3){#refFoot3} Brain. Explode.

#### Consequences.

What if Adobe successfully can hide its entire Flash subsystem and so it debuts CS5.1 with the feature turned back on? Hundreds of apps get onto the store by tricking Apple gatekeepers. What do you think will happen when Apple figures out a way to detect with 99% accuracy that an app was generated with Flash?[<sup>4</sup>](#foot4){#refFoot4} Yep, they&#8217;ll pull those apps faster than Brian J. Hogan picked up that phone. 

The point is, there is no reason for Adobe to take that chance with their customers.</p> 

<div style="border-left: 1px solid #353535">
  <ol>
    <li id="foot1">
      This class name is just an example. It&#8217;s anyone&#8217;s guess as to how Adobe would need to implement this.<a href="#refFoot1">&#x261D;</a> <li id="foot2">
        One could just generate a &#8216;Hello World&#8217; program and see that all Flash views inherit from some base class for example.<a href="#refFoot2">&#x261D;</a> <li id="foot3">
          Not to mention you couldn&#8217;t use any shared resources (images, plists, etc.) without obfuscating those.<a href="#refFoot3">&#x261D;</a> <li id="foot4">
            And for all the examples given, the detection takes a fraction of the engineering effort of the obfuscation. It&#8217;s only reasonable to assume that detection will take less effort than generation. <a href="#refFoot4">&#x261D;</a> </ol> </div>