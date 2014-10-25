---
layout: post
title: "Deconstructed: iMac with Retina 5k display"
date: 2014-10-20 19:01:40 -0700
comments: true
categories: ['deconstructed', 'front-end']
published: false
---
So a friend clued me in today about how much he hated parallax scrolling effects and how it plagues the modern web.  I'm still not tired of it and I enjoy taking a look at cute and quirky sites to see how they were made.  He clued me into the latest offender of web design, that being none other than the company that prides themselves on elegant design themselves, Apple. I go off to take a look to see if its really is as terrible as he says.

<!--more-->

<a href="http://www.apple.com/imac-with-retina">iMac with Retina 5K display</a>
<img src="https://s3.amazonaws.com/jasonjlblog/Apple+-+iMac+with+Retina+5K%C2%A0display.png" />

<img src="https://s3.amazonaws.com/jasonjlblog/Apple+-+iMac+with+Retina+5K%C2%A0display+(1).png" />

The content that I want to deconstruct is the zoom out effect that starts with the man embracing the waterfall and by scrolling down, slowly zoms out and displays that the man is but a very small part of a very large resolution picture.  Its true, this effect is not parallax scrolling, but rather zoom scrolling(?). It uses the same concept of parallax scrolling by manipulating the display of the content based off of the position of the scroll bar.  This is usually accomplished through attaching a css manipulating function to the scroll event handler.

we're going to dig through Apple's source code.

Upon initial inspection the most obvious thing is inspecting the element itself.  This brings up that a background image is being used.  This background image is only a fraction of the size of the initial image.  This background image is actually used as a fallback incase the user was not able to get the full experience of the page due to incompatable browser or lack of Javascript capabilities. 

digging further into the DOM, I tried to find an img tag that contained the giant picture but I wasn't able to find one.  I keep digging and found a container with the class '' that contained lots of canvas elements. Playing around with the width height and positioning of the canvas I found out that these are the elements that are displaying the actual picture.  The interesting thing is, is that the picture was split up into a grid of 5x3.  Each canvas element held a section of the picture and were absolutely positioned together to seamlessly display the whole thing.  I believe the reason why it was done this way was to get around the way that jpegs load on modern browsers.  Jpegs load through a stream.  Any image data that the browser gets its digital hands on, it will display, even if the picture is incomplete or blurry.  Apple, being very considerate of the user experience, probably didn't want the user to start their journey onto this page with a blurry image that slowly clears into view.  They wanted the content and presentation of their page prepped first before they were ready for the unveiling.  The use of displaying the image through the canvas elements would allow for this preparation time before it was actually ready for the viewer.

Another theory is that canvas elements just work better with CSS3 translates/transforms.

This theory can actually be very incorrect and that I don't know what I'm talking about.

Digging into the Javascript source

