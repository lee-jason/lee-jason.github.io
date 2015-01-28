---
layout: post
title: "6.82 Visualizer Postmortem"
date: 2015-01-27 16:22:06 -0800
comments: true
categories: ["javascript", "project", "postmortem", "angular"]
---

<a href="http://jasonjl.me/682Visualizer">website</a>   
<a href="https://github.com/lee-jason/682Visualizer">source code</a>   

6.82 Visualizer is a gold and experience data charter for the game Dota 2. For those unfamiliar to the game of Dota, there are two teams of five that duke it out against each other to ultimately destroy their opponents base. Just like a real war, the way to gain an advantage over an opponent is to have a stronger capital, that's essentially what Dota's gold and experience is. There was a controversial update to the game where it drastically changed the effect of how each team would gain experience and gold giving the losing team a boost in capital in order to catch up. Many members in the community were worried that this would change their game for the worse and they expressed it with personal anecdotes of many lost games. But when everyone had stories I wanted the data. I sought to provide the community with the hard facts in an easy to view format of how the experience and gold were exactly affected so that we can see clearly just how much it altered the game.
 
<!-- more --> 

<h2>The Backend</h2>
Backend? What backend? There actually is no backend to this site.  The entire application is self contained in the html and js file.  The pages are served through github's project pages much like this blog you're reading.
 
<h2>The Frontend</h2>
The frontend was built using a charting library <a href="http://www.chartjs.org/">chart.js</a> as well as <a href="https://angularjs.org/">angular.js</a> for the data binding. In the angular file, I have one controller for the form that maintains all the adjustable data. I knew that angular would be a good fit for this project since it very easily binds changeable data to the view. I also created a 'chart' directive attribute tag. This chart attribute identifies a canvas element to be one of the multiple data charts. It allowed me to create one 'chart' type of class and apply it to all the canvas elements. And finally I had one factory that contained all the formulas for calculating the data. I tried using as much as angular offered in intelligently splitting up the tasks of the site.
 
<h2>The good, the bad</h2>
This is actually the first site I made that was incredibly well received. Unlike my other sites, marketing this one was no problem. I posted it once on reddit/r/dota2 where it quickly rose through the ranks with nothing but positive praise. I felt very alive after receiving the first bits of feedback. I quickly continued to work after the initial release creating more graphs, more analysis, preset configurations, sharable configurations, layout changes (you can actually see the <a href="https://github.com/lee-jason/682Visualizer/commits/gh-pages">history here</a>. I keep learning that community feedback really drives me forward and motivates me to a degree that is quite abnormal for me. Community figures were tweeting about my site, forums and fanpages in China, Germany and Russia were referring to my page.  The page was being shared organically through twitter and facebook. It was actually quite amazing for me. I'm really proud of the fact that I was finally first to market in something. Usually most of my ideas are imitations or alterations on an already existing idea, but this time I got lucky.

That's all well and good but of course there's some bad.  The bad is that the page is not optimized in a degree that I would be proud of. Granted, there's a ton of things to update but playing with the sliders produces somewhat choppy animation.  Movement is not fluid and I've still yet to discover why. It could be the charting library I used, it could be a large amount of angular watches, it could be just the large amount of repaints slowing the browser.  Whatever it is, I do not know. Optimizing that page to update at 60fps was not my highest priority. I'm also a little ashamed that I wasn't able to produce a proper mobile/table view.  According to Google Analytics, about 10-15% of hits were coming in from tablets.  The site is definitely usable on tablets and mobile devices but it is unoptimized for those sizes.