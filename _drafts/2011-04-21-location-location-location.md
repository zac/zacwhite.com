---
id: 281
title: Location, Location, Location
date: 2011-04-21T23:01:45-07:00
author: Zac White
layout: post
guid: http://www.zacwhite.com/blog/?p=281
permalink: /2011/04/21/location-location-location/
tags:
  - ""
categories:
  - apple
  - iphone
---
A lot has been written about why Apple is keeping locations in a file called &#8216;consolidated.db&#8217;, so I&#8217;ll try and keep it brief.

##### My theory:

Apple is constantly doing cell triangulation in the background. These locations are stored in &#8216;consolidated.db&#8217; along with a timestamp. They are used for the [significant location change API](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html%23//apple_ref/occ/clm/CLLocationManager/significantLocationChangeMonitoringAvailable).

I say it&#8217;s just cell triangulation because the points recorded aren&#8217;t exactly where you&#8217;ve been. There is also a column in the database for the &#8216;HorizontalAccuracy&#8217;. This never falls below 500 (meters presumably) in my file and is often higher. I have one point that has an error of 79130m!

##### Questions

##### Why are there no points at my home or work? I spend all day there!

Cell triangulation isn&#8217;t very accurate. I would guess that inside, cell triangulation puts you all over the place. It would be interesting to see the error circles around each point. This would make things a little more clear.

##### Why is Apple keeping a **complete** history?

Engineers over-engineer. They&#8217;d rather be safe than introduce a bug. It&#8217;s a lot easier to keep all the data than to purge it.

##### What if they&#8217;re cell tower locations as suggested by [this article](http://www.willclarke.net/?p=247)?

Why would Apple be tracking cell tower locations. That wouldn&#8217;t make sense. They know where the cell towers are.

I doubt there&#8217;s anything malicious going on here. Just a solution to a problem that failed to take into account how insane people are about location information.