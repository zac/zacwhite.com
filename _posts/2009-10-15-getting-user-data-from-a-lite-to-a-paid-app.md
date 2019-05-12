---
id: 182
title: Getting User Data From a Lite to a Paid App
date: 2009-10-15T19:20:18-07:00
author: Zac White
layout: post
guid: http://www.zacwhite.com/blog/?p=182
permalink: /2009/10/15/getting-user-data-from-a-lite-to-a-paid-app/
tags:
  - ""
categories:
  - iphone
  - software
---
It&#8217;s a common problem now on the App Store. User buys Lite version. User likes lite version. User buys the paid version and loses all of her/his data. Not a very good experience.

How can we fix this?

UIPasteboard of course!

Apple introduced an excellent and oft overlooked addition to the iPhone SDK when it gave developers the same power that they have on the desktop for sharing large chunks of data between applications. But Zac, you say, you of all people should know you can&#8217;t _share_ data between apps&#8230;they&#8217;re sandboxed! Not true! Think of UIPasteboard as those little paper airplanes you used to send notes in back in school. No sandbox can hold UIPasteboard&#8230;

##### Method 1

This first method is fairly straightforward and easy for small data sets: Keep custom pasteboard filled with data you&#8217;d want users to import into your paid app.

<pre class="prettyprint">UIPasteboard *sharedPasteboard = [UIPasteboard pasteboardWithName:@"com.mydomain.myapplite.sharedpasteboard" create:YES];
sharedPasteboard.persistent = YES;</pre>

By executing that on launch and keeping your sharedPasteboard up to date, you&#8217;ll always be able to request that from your paid app and auto-discover data from the lite version. The nicest thing about this method is most people _won&#8217;t notice_ you did anything. Maybe you want users to notice your work&#8230;but sometimes the best features are the ones users don&#8217;t notice.

##### Method 2

This is for apps that have lots of content to put on the pasteboard and updating it all the time wouldn&#8217;t necessarily be so great. It&#8217;s a little weird but bear with me.

First, make sure you app can respond to the MyAppLite-export:// scheme. The parameters to this don&#8217;t really matter, but you could use them to specify some subset of data to export. You could also make this a little more generic and perhaps a little more secure by specifying a pasteboard name to the URL.

When MyAppLite is launched using this scheme, the users is presented with an &#8220;Exporting&#8230;&#8221; screen or something of the sort while MyAppLite puts everything requested on a special, one time use, pasteboard. After the pasteboard has been filled, MyAppLite opens MyApp-import://NAME\_OF\_PASTEBOARD. Now the paid app knows where to look to get all the necessary MyAppLite data off of the temporary pasteboard.

##### Security Concerns

Method 2 definitely has some security risks which possibly could be worked around. For one, if someone finds out your schemes they can read users data from a lite version and overwrite data in the paid version. With some obfuscated keys, this might not be much of an issue. Also, you wouldn&#8217;t want to send passwords or sensitive data using this method. That kind of data should be stored in the keychain anyway that is already available across your apps.

I hope this gives you some ideas on how to transfer data with UIPasteboard. I&#8217;ll try and post some sample code illustrating some of these concepts soon.