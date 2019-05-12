---
id: 393
title: 'Why I Don&#8217;t Want An Apple TV SDK'
date: 2012-05-31T18:22:40-07:00
author: Zac White
layout: post
guid: http://www.zacwhite.com/?p=393
permalink: /2012/05/31/why-i-dont-want-an-apple-tv-sdk/
tags:
  - ""
categories:
  - advice
  - apple
  - software
---
There have been some [rumors](http://www.macrumors.com/2012/05/30/apple-to-unveil-television-set-operating-system-at-wwdc/) about an Apple TV OS being introduced at WWDC and presumably an SDK. And it seems everyone is hoping Apple will &#8216;fix&#8217; TV. It is a seriously broken system. But just an SDK won&#8217;t fix it.

##### The path of least resistance.

It would be easy for Apple to offer an SDK for Apple TV. The platform is there and many content providers already have an app that they could port to a TV interface. ESPN, ABC, NBC, etc. all have the infrastructure to deliver video to iOS on the phone/tablet, so adding Apple TV wouldn&#8217;t be too difficult. But I think it would be a mistake.

The problem with that is consistency. ESPN would have a layout totally different from ABC, and re-learning each new app you downloaded would be painful experience. Changing &#8220;channels&#8221; by switching apps would be a completely crazy context switch. TV&#8217;s current redeeming quality is it&#8217;s ease of use.

##### There&#8217;s a better way.

What if instead of allowing &#8220;apps,&#8221; Apple required structured content for inclusion in their &#8216;TV Store&#8217;? The basic pieces that would cover most use-cases are:

  * Live video stream. Using HTTP Live Streaming of course.
  * Program Guide. An XML representation of the past and upcoming shows.
  * On-demand video clips. Links to video clips. Although, they could be full episodes.

And now imagine Apple lets users subscribe monthly to each &#8220;channel.&#8221; Prices set by the channel, but you could imagine $2 for most Cable and maybe even free for the broadcast stations. Movie channels like HBO could be more at the $10 price point. Ads could be served by you or by Apple&#8217;s iAds. You could bootstrap a TV channel without needing millions of dollars. Independent video podcast networks like [TWiT](http://twit.tv/) could have a channel. I could start a channel in my garage. All for just the cost of a camera and a server.

##### Siri and the interface.

Because they&#8217;re getting structured data, Apple could provide a clean and consistent interface. And it seems a solid bet that the interface will be primarily voice based. You could ask Siri &#8220;Tune to Wolf Blitzer.&#8221; and she would change to the CNN channel. Or &#8220;Remind me when the next show about whales airs.&#8221; and she would do a search and schedule a reminder.

But the Apple TV doesn&#8217;t have a mic? Apple could introduce a new <a href="http://www.ilounge.com/index.php/news/comments/third-gen-apple-tv-teardown-finds-bluetooth-4.0-chip/" title="bluetooth remote" target="_blank">bluetooth remote</a> with a mic built in (idea from [@rickriemer](http://twitter.com/#!/rickriemer)). Or one could simply use the new &#8220;TV&#8221; app built into iOS on iPad, iPhone and iPod touch. When you&#8217;re in your living room, it acts as a remote control and interface for Siri for your TV. And when you&#8217;re on the bus, you can watch streaming TV just as you can at home.

##### The sad part.

This won&#8217;t happen. Not because it&#8217;s hard or because users won&#8217;t like it. Because of copyright holders. I don&#8217;t know much about the industry, but I&#8217;m guessing channels won&#8217;t be able to just add their content to the Apple TV without restructuring all their movie and TV show contracts. Lawyers&#8230;what can you do?