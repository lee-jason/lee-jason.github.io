---
layout: post
title: "Codewars, Leetcode, Hackerrank. Practical Programming Practice Challenges"
date: 2015-03-30 12:41:31 -0700
comments: true
categories: ['programming', 'java', 'javascript']
---
Sometimes the projects you work on just aren't stimulating enough. Sometimes you need to fill your brain with as many programming problems as possible. Usually when I work on a project I only have about one or two serious problems I need to solve and after I solve them its smooth sailing. When you need some more challenges you need to be exposed to as quickly as possible, you need as many bite-sized challenges that your little brain can stand.  This article is a review on the online judges, <a href="http://www.codewars.com/">Codewars</a>, <a href="https://leetcode.com/">LeetCode</a>, and <a href="https://www.hackerrank.com/">HackerRank</a>.

<!-- more -->

<h2>Codewars</h2>
<a href="http://www.codewars.com/">Codewars</a> takes the concept of the 'Code Kata' and gamifies the exercises so programmers always have something to challenge themselves when they got a few minutes to spare. Challenges are usually short, ranging from minutes to a few hours.  Most of the questions often have contexts associated with them so it always feels like you're actually solving a potential real world problem rather using your tools as a programmer, even though the contexts are often fantastical in nature.

<h3>Things I like</h3>
There's a clear progression that Codewars wants you to take so it always ensures you know the fundamentals of your language first before you move on to the harder challenges.

Solutions of all the other members are available to analyze after solving the problem. I truly think that social programming is the way to get better at writing code. The solutions are bite sized so its easier to digest new programming features or new ways of organizing code that you've never considered before. I highly recommend Codewars if you really want to learn more about your language.

Codewars also allows you to run a test submission to run your test cases before officially submitting your answer. I believe the amount of submissions of a solution will negatively affect your score but I'm not sure. Its better to be safe than sorry and Codewars definitely allows you to do that.

The code editor is also pretty good with syntax highlighting and code editing.

<h3>Things I don't like</h3>
The Java error messages are less than helpful.  I'm not sure how they return exceptions when your code doesn't compile, but often times the console is going to return an exception from the enclosing outer stack that executes the code meaning you don't get any useful stack debugging information, you just know somethings wrong.

Each question has tags associated with them describing what methods of programming you'll use to solve the problem. These tags often give a hint on what to do to solve the problem which is kind of a problem since people look at these tags as extra meta information to give them a lead. I often like to come into a problem blind and find out myself what to do in order to get to a solution.

<h3>Overall</h3>
Overall I think its actually a really neat service. Like I said earlier, I've learned so much more about JavaScript than I ever would have if I were to just continue working on my own projects. I'm definitely going to use it again when I delve into Python.

<h2>LeetCode OJ</h2>
<a href="https://leetcode.com/">LeetCode OJ</a> is all about the algorithms.  There are no pretenses, you're going to be solving very straightforward problems that have very defined lower bounds in terms of space and memory. These problems will often test cases on all edges of the spectrum and will only accept answers that meet the lowest bound limitation.  Your preferred language is just a means to work around an algorithm. At the end of each problem, Leetcode ranks you up against your peers by how quickly your code executed.

<h3>Things I like</h3>
No muss no fuss, these questions ask you about algorithms and after enough exercises will hammer the common ways to solve them into your brain. I think this is a good second level after reading and solving the exercises to Crack the Coding Interview. There are tons more exercises here that touches on things that CTCI doesn't which makes it a natural progression to the book. 

The code editor is actually really good. Everything flows like a real professional code editor including quality of life improvements such as auto tab indentation, mass commenting, auto closing brackets, mass tab indentation... 

The questions are very direct.  There are no fillers, no context. At first its a little ambiguous to know what values will be used to test your program, but after a while it becomes very obvious what kinds of edge case values Leetcode expects your program to process.

<h3>Things I don't like</h3>
Since the online judge only wants answers that are strictly the lowest bound in terms of space and runtime, it'll often fail passing solutions that are not the most optimal. It will fail an acceptable solution of O(nlogn) if the lowest bound possible is O(n). Sometimes you think of the correct algorithm, but implement it in such a way that it still exceeds the time limit. It ambiguous to know what exactly is the bit of code to optimize in order to pass the test.

The community seems more concerned with writing concise code rather than best practice code. Discussion solutions are often compressed to the point where its difficult to read.

I really wish they would allow us to see each user's submitted code so we can see exactly what we need to do to get the most optimization out of our language of choice.

<h3>Overall</h3>
This site is great if you need some extra exercises to hone in on pure algorithm practice. I would not recommend it as a place to actually learn from 0% knowledge but more of a place if you're looking for medium to advanced challenges. You have to be familiar with your language of choice as your language is just a tool you should be familiar with to solve a greater problem.

<h2>HackerRank</h2>
<a href="https://www.hackerrank.com/">HackerRank</a> is site that focuses more on the competitive nature of programming. HackerRank encourages you to participate in its many ongoing week long challenges. They support a ton of languages cover a wide range of practice problems ranging from algorithms, functional programming, linux shell cmds, and even AI.

<h3>Things I like</h3>
Since each challenge requests you parse a text file, it actually gives you plenty of exercises in reading in a file, parsing it, and outputting it.  I feel like this is something that's overlooked by a lot of challenge sites so its nice to see a site that allows you to practice that skill.

Each challenge has very clearly defined variables.  Sometimes its actually kind of annoying, but each challenge will let you know exactly what its going to test for.

There are a ton of active competitions going on at a time meaning you can always test your chops against other programmers.

<h3>Things I don't like</h3>
Its code editor is missing a lot of convenience features that the other code editors have. Sure it has code highlighting but other than that its very uncomfortable to use.

Parsing a text file every time is kind of annoying. Some times you just want the values to be passed in as primitive data as if it would be used as a modular piece in a larger project.

<h3>Overall</h3>
I've used this the least amount so I'm not yet decided on whether I like it or not. The challenges are appropriately challenging but the text editor is not very fun to use. Its less of a site to practice but more to compete. Some of you may view this as the same thing.
