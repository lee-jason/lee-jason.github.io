---
layout: post
title: "Quiz With Me Postmortem"
date: 2015-01-27 12:20:43 -0800
comments: true
categories: ["project", "javascript", "postmortem"]
---

<a href="quiz.jasonjl.me">website</a>   
<a href="">source code</a>   

Quiz With Me is a community generated trivia site. Each user that connects to the site competes with all other users on the same question after a set period of time. Once time is up, the polls are tallied and users can compare their choice to those of their peers. Users can play anonymously or sign-up and login to keep track of their points. Each user can also submit a multiple choice trivia question with at least four or more potential answers. Each question can also be judged by the community as to whether it is factually correct or just a nonsensical question. Quiz with me brings the joy of answering trivia questions vs the entire world!
 
<!-- more --> 

<h2>The Backend</h2>
The backend is ran on node.js with the express framework with a mongo+mongoose persistence layer.  This is my second project working with node and I wanted to create endpoints that follow public api guidelines. There are api endpoints GET, POST, CREATE endpoints for users and questions that handle account and question management respectively. You would think I would use socket.io again to create real-time experience but I really wanted to practice working on creating a RESTful api. Its funny, all API creation guides recommend making your server side content stateless.  The problem here is that I needed to synchronize all users to the server's state, the time remaining on the current question, and the question itself. I think its easy to create a stateless REST endpoints when dealing with normal documents, but when we're creating a game of sorts, its kind of assumed that we're going to be maintaining a state that all clients have to synchronize to right?
 
<h2>The Frontend</h2>
The front end is composed of a single index.html page that contains a bootstrapped themed skeleton and an angular.js app file that fills it all in.  The angular.js file has all the routes inside providers that asynchronously contact the server endpoints to get new questions, or handle logins and registrations, or create new questions. There's controllers for the questions segment, a controller for the poll segment, and controller for the header bar.  Each of these controllers does what angular does best which is watch for changes to objects inside the controller which automatically update the {{angular}} tags in the front end. The page is also responsive in that the buttons and content will reorganize themselves based off the window size.  There's slight display issues regarding the header bar in the mobile view but I'm generally happy with how I finally got to try out this new framework everyone loves to hate.
 
<h2>The good, the bad</h2>
Of course, competing with your friends and strangers across the world would be the most ideal scenario but the problem is the lack of userbase.  The site functions well with all the features of account management, question submission, question tribunal, in a some what clear presentation but all of this doesn't really matter unless someone is using it.  The presentation is also very bare, and is still really in the prototyping stage.  I'm not really a designer, which is why I used the bootstrap framework to make the page look somewhat good, but it still looks very barebones. At this point I'm happy that the functionality works and making the front end look professional was never the goal. The site suffers from terrible SEO due to the Angular front end that dynamically fills in content. I tried to remedy this by adding an about lightbox describing the site but it doesn't seem to help. I'm still unsure how to organically get hits for web applications like these without actually doing the work of marketing it first. Its making me believe that the new Google SEO algorithm is dependent heavily on public impression and how well it does on social network sites, twitter, facebook, reddit and so on. The goal of this project was to continue working with node web applications and to get my feet wet with angular.  I totally think I met my goal when felt like I got at least waist deep in the angular pool.