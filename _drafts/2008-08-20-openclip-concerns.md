---
id: 158
title: OpenClip Concerns
date: 2008-08-20T18:33:57-07:00
author: Zac White
layout: post
guid: http://zacwhite.com/blog/?p=158
permalink: /2008/08/20/openclip-concerns/
categories:
  - apple
  - mine
  - openclip
  - software
---
<p class="note">
  <strong>Note:</strong> This is also mirrored on <a href="http://openclip.org/main.php">openclip.org</a>.
</p>

Let me first explain how OpenClip works:

How it works is relatively simple and doesn&#8217;t break the SDK agreement. OpenClip works by looking into the Documents folder of other applications to get their pastes. Applications are allowed to write all they want to their own Documents directory (for copy), so no foul there.  
Applications are also allowed to read outside their sandbox into the Documents directories for other apps (for paste), so no foul there.

**How could that ever go wrong? What&#8217;s the problem?**

The problem is Apple is probably going to shut down reading into other application&#8217;s boxes. I&#8217;m all for that as long as one of two things happens before:

  * Apple ports NSPasteboard to the iPhone SDK (radar://6158362)

NSPasteboard is what I modeled OpenClip after. It allows copy and paste between applications, but more than that, it adds _communication_ between applications. Porting NSPasteboard would make OpenClip moot and developers could easily respond by changing all references to OCPasteboard to NSPasteboard. Easy for everyone involved.

  * Apple shuts down sandbox reading but creates a public folder for apps to write to. (radar://6156881)

If Apple does this, inter-app communication would still be possible through OpenClip or some other file communication method.

Applications need to work together. That&#8217;s the Apple Way®. I don&#8217;t know what you think, but to me the smaller and simpler an app is on the iPhone, the better it is. To make apps that are simple but powerful, developers need to make applications that can communicate.

**What happens if Apple shuts it down?**

Here&#8217;s what happens if a new version of the iPhone OS comes out and OpenClip can&#8217;t communicate anymore:

  * Developers who implement OpenClip to either \*only\* copy or \*only\* paste can easily check if that will work before displaying any UI. If it breaks, those options just disappear.
  * Developers who implement to do both copy and paste retain copy and paste within their application. That means that when and if the next update breaks OpenClip, applications won&#8217;t stop launching, they won&#8217;t crash when you try and copy and they won&#8217;t get garbage data. All that will happen is the app will only be able to copy and paste in the application. Not a horrible way to degrade if you ask me.

**What about the UI? There are going to be like 20 implementations for copy/paste!**

UI is a hard problem to solve. One of OpenClips goals is to provide examples of UIs for copy and paste. But to be honest, the best thing you can do with OpenClip is use it only for simple data. Only copy interesting data that a user would want to copy and present UI similar to Apple&#8217;s push to save image UI. Doing what MagicPad does and providing a whole way to select text is not what 90% of apps need. Look at what Twittelator did with copy in the video on GeekBrief.tv (http://www.geekbrief.tv/copy-and-paste-for-iphone). Press and hold needs to be the way most apps utilize copy and paste.

You can check out Proximi&#8217;s MagicPad UI proposal video [here](http://magicpad.proximi.com/video.php). It&#8217;s worth a look.

Lets face it. Hardly anyone is not buying an iPhone because it doesn&#8217;t copy and paste. It&#8217;s useful, but not necessary. Apple knows this so they put it at the bottom of their to-do list. If Apple were to implement NSPasteboard, however, 90% of apps could gain some really great functionality. Until then, maybe OpenClip can serve as a sneak peek to Apple, developers and users that this kind of framework would benefit the iPhone greatly.