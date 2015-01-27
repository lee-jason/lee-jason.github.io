---
layout: post
title: "Confess.me Postmortem"
date: 2015-01-27 09:47:57 -0800
comments: true
categories: ["project", "javascript", "postmortem"]
---
---
layout: post
title: "Confess.me Postmortem"
date: 2015-01-27 09:47:57 -0800
comments: true
categories: ["project", "javascript", "postmortem"]
---

<a href="confess.jasonjl.me">website</a>   
<a href="">source code</a>   

Confess.me is an anonymous real-time secret posting website. Its meant to be a safe place where you can share with the world about what's been bothering you. Other supportive members who read these messages can give back simple responses of support letting the secret sharer know that they've been heard. The secret sharer can thank these anonymous eyes and ears for their support in turn. All of this is done in real time letting user's know that there are people out there willing to listen.

<!-- more -->

<h2>The Backend</h2>
This site was an experiment on trying out some new technologies on the backend.  Its my first web application using node with express on the backend, using the socket.io library for easy real-time communication implementation.  I use a basic cookie-based session tracking to ensure that users who revisit the page with the same cookie will maintain the same anonymous identity so long as the cookie exists. This means that users who refresh the page, or leave and comeback will maintain the same identity. Any 'pardons' will still be directed to that session.  This was actually one of the more interesting things to implement because the cookie-session isn't tied to the socket object that's given to them. Users connect to the /root which gives them the index.html page.  This page then creates a socket object then sends it to the server. I needed to keep track of each socket that was made and store it in a session to socket object map. Its funny, as I write this out it seems so simple, but at the time I was thinking for a while how to make socket.io remember its socket connections. After that it was smooth sailing.  The socket.io documentation is very basic but explains all there is to know about real-time communication.  Set the server up to listen for events, have the client send those events. Have the client listen for events, make the server send those events. Its very basic but I can imagine so much more could be done with this ranging from real-time games, polls, data, and even parallel computing. 

In terms of persistence, there wasn't supposed to be any message persistence due to me wanting users to feel comfortable that their messages are only meant to be seen with the current users on the site and nobody else but since the site has a 1 person user base I felt the need to store some of the latest messages so that new visitors will see that there's actually some content rather than a blank screen. I use the hip new document storage system mongo wrapped with the mongoose framework to support a Schema.  For this project I really just had one schema, that being Message, but I just wanted to try all these hip trending web technologies. Mongo as itself is a pretty simple concept to understand. As a person who is already familiar with manipulating javascript objects, the nature of the mongo datastore came very naturally.  My only wish is that it would carry some familiar SQL syntax for table searching but unfortunately its a new syntax, still simple, but new. Mongoose required a bit more reading to understand though. Its documentation is decent but <a href="http://jasonjl.me/blog/2014/10/17/framework-frustration/">I never really liked frameworks</a> due to the amount of fixes or workarounds or extensions to problems of a base technology that you've never found the need for.

<h2>The Frontend</h2>
The front end is pretty simple, uses the client side javascript for socket.io and some extra javascript to accomodate the multi-column layout.  The front end is media query responsive so it'll shrink itself to as many or little columns needed on smaller devices.  It was fun trying to get the messages to organize themselves based on a multi-column layout vs a single column mobile layout. My rule for the multi-column layout was to always places the message in the smallest column as well as have newer messages up top, that way no column will be incredibly long. When users resize their window, messages get reorganized so that it always follows this rule. I also used google's third party library on html sanitization to prevent the page from accepting links and to protect users against XSS attacks.  The problem with this real time updating of content is that the site has zero SEO. All content is dynamically injected through javascript so there is no real content on the index.html page. To be fair though, I didn't really optimize the page to be picked up by search engines, otherwise I would have included an about page, or help, or a blog, or something with actual content, but at this moment I'm just happy that its primary function works, even when there's no audience.

I've always been fascinated with real time communication and what that could mean for the web.  I thought for this project having someone respond with immediate results would be better than having to refresh the page constantly to see if someone replied to your secret. 

<h2>The good, the bad</h2>
The good part about the site is that it works. The bad part about the site is that it has no user base so it can't really function as intended.  Ideally there will always be more than a single person on, but at this point it has no user base other than my multiple testing browser windows. At this point I'm not sure if it can even handle more than five people at a time much less a hundred or even a thousand. Currently the code is sitting on a free Heroku dyno, I never intend on taking it to a real production level plan unless it happens to explode which I doubt. This was also my first experience in javascript server side programming and what people describe as callback hell.  It was interesting imagining how code jumps around to different functions in a non-sequential manner and to be honest, I'm still not entirely sure how to handle errors.  I haven't yet fallen into callback hell but I can imagine that it can get pretty nasty in larger applications.

Well there you have it. I hope this site will be useful as an outlet for the things that trouble you.