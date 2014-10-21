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