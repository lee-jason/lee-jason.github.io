---
layout: post
title: "Things I learned from failing technical interviews"
date: 2015-03-11 10:46:56 -0700
comments: true
categories: ['career', 'mathmatics']
published: false
---
I just had a long month of technical phone interviews from numerous companies that I got from the technical job hunting service <a href="http://hired.com">hired</a>. Its funny, you don't truly learn until you've actually made mistakes in a scenario that actually matters. In this case, I think I learned a ton of lessons from getting rejected from all my phone interviews.

<!-- more -->

<b><i>preface!</i></b>   
I have tried to remain objective in my learnings but there may be hints of bitterness here and there on the account of, you know, failing most of my technical interviews.

<h2>The only winning move is not to play</h2>
Interviewing is a losing game. All applicants are losing, its just a matter of losing the least.  From the time you make that first greeting the clock starts ticking and you're already losing. Your interviewers are expecting you to finish a task in a certain amount of time and once finished will compare your time and performance to that of your peers.  They are not looking for things that will impress them but rather for things that will reduce their opinion of you.  There is no way to 'win' an interview, only a way to lose the least. Say too many things that rub the interviewer the wrong way and you're out of the race.  

<h2>The odds are stacked against you</h2>
A typical technical interview path usually involves five or more gatekeepers you have to pass. You usually have to impress each of the following people in order to advance to the next level
<ol>
  <li>non technical Company Recruiter</li>
  <li>technical phone interviewer #1</li>
  <li>technical phone interviewer #2</li>
  <li>CTO/manager</li>
  <li>Take home assignment</li>
  <li>onsite interviewer #1</li>
  <li>onsite interviewer #2</li>
  <li>onsite interviewer #3</li>
  <li>onsite interviewer #4</li>
  <li>onsite interviewer #5</li>
  <li>onsite interviewer #6</li>
  <li>day two onsite interviewer #1</li>
  <li>day two onsite interviewer #2</li>
  <li>day two onsite interviewer #3</li>
  <li>day two onsite interviewer #4</li>
  <li>day two onsite interviewer #5</li>
  <li>day two onsite interviewer #6</li>
  <li>Congrats you got the job!</li>
</ol>
<i>The amount of interviewers has been expanded to indicate a nightmare interviewing scenario. Your interviews will probably not look like this, but is not outside the realm of impossibility</i>

Remember earlier when I said that technical interviews is a losing game? Imagine a scenario where you are actually a really good engineer (unlike me haha!), and you have a 99% chance of passing through each gatekeeper. This means that you ultimately have an 84% (.99^17) chance of passing through each gatekeeper and landing that job.  The odds are good, but not guaranteed.  Now imagine if you're a good engineer but you're no Linus Torvalds and you have a 95% chance of making it through each phase. That means you ultimately have a 41% (.95^17) chance of impressing everyone and getting the job of your dreams. If you're not within the top echelons of programming you might as well start practicing burger flipping because the only thing you'll be engineering is the perfect .99c cheeseburger at McDonalds. Okay, that was mean, but my point is, you can be very good at what you do and still be rejected because of an interview with a gatekeeper going very wrong, and that's okay!  

<h2>Fail Fast. Try Again</h2>
Its okay to fail at these interviews. Failing a phone screen doesn't say anything about your character or intelligence. Say you have a 40% chance of getting past all the gatekeepers and landing a job. The probability of you landing that job at the first company you interview at is 40%, <code>1 - P(successive failures)</code>. The probability of you landing that job after two seperate company interviews is at 64%. The probability of landing the job after three seperate company interviews is 79%! Okay lets say you're a average engineer and you have an 80% chance of making it through each gatekeeper.  We first calculate the probability of you getting past each gatekeeper and landing the job (.80^17) 2%. Ouch, you have a 98% chance at failing to get a job at a company with 17 gatekeepers. Now we figure out if you only apply to companies that all use 17 gatekeepers in their employee process then you will have a 2% (1 - .98^1) chance getting in on your first try 4% chance on your second (1 - .98^2), 6% on your third (1 - .98^3), 8% on your fourth (1 - 98^4) okay you get the picture.  The thing is, is that interviews are not all cut and dry like a math problem.  There's a lot of things you can't control  such as the amount of interview gatekeepers and your probability of acceptance as an engineer for the position.  While we can't ever get our acceptance probability to 100%, we can definitely do some things to improve those odds.

<h2>Do not be yourself</h2>
Its kind of psychopathic, but do not be yourself.  Be prepared to take on a persona of the most attractice engineer that has ever graced this planet. Never willingly give away anything that might expose your humanity.  Technical interviews are like an opposite <a href="http://en.wikipedia.org/wiki/Turing_test">Turing Test</a>.  Instead of tricking the user that the computer is a human, you have to trick the interviewer that you are in fact a robot. You do not make syntax errors, you do not need to consult apis, you do not need to check a table of twos, you do not need to learn anymore as you already know everything there is to know about engineering. Working for a company is not an opportunity to learn, companies don't want to hear that, they don't want to pay you for that. Its an opportunity for the company to buy you to offload work on the current team much like how they may buy an additional server for their load balancer. You are viewed as an additional core to their server cluster and you will act that way. After you get the job, feel free to be yourself but until then, you must give off the persona that you are perfect. 

<h2>Do not panic</h2>
We are not perfect. Nobody is. We are flawed as human beings. We are not a compiler. We are not an interpreter. We are not a server core. We do not know every computer science principle, algorithm, strategy, pattern, data struct, language api, language feature. There is so much we don't know, and that's okay.  If you don't know something and a gatekeeper deems you unworthy, so what. Who cares, you failed, move on.  Its almost a guarantee that you will be asked something you don't know and even after an hour of thinking about it and problem solving around it you still won't know.  It happens.  The important thing is to not panic. You don't think well, or even think at all when you're panicking.  Stay calm, stay cool, don't look at the clock, don't freak out.  You will get questions you don't know the answer to, you will feel like an idiot after hours of interviews, you will realize your ignorance.  Its cool, it happens, just make sure you're the calmest idiot in the room. Interviewers interview tons of people that don't know the answer to their question. They ultimately want to see if you can figure it out in less than an hour but do so by being calm and collected, as if you encounter mysteries every day and this is just another one of them. Don't give the gatekeeper more ammunition to not let you move on to the next level. 

<h2>Do not talk</h2>
Do not talk too much that is.  Say you're asked about a subject that you know truckloads about.  Do not drone on and on and on hoping to impress your interviewer.  They probably don't care.  Remember, they are only looking for things to dock points off of and talking too much is actually a negative. Give the general answer, then ask if you can go further in depth. Every single technical interview help book/guide recommends to keep talking during your interview because your interviewer doesn't know if you're thinking or if you're just stuck. This doesn't mean talk all the time. Its incredibly difficult to blurt out a stream of consciousness and think at the same time. Take the time to close the yap and think. Let your interviewer know you're going to think for a little bit and actually do it. 

<h2>Remember Culture Fit</h2>
One of the most disparaging reads I've read was about an interviewer that was gunning for Google. If you haven't read this blog, take a look at some of the <a href="http://iwillgetthatjobatgoogle.tumblr.com/archive">articles</a>. This person is dedicated, has a goal, and a company in mind, seems pretty intelligent and very motivated.   <a href="http://iwillgetthatjobatgoogle.tumblr.com/post/19790649528/unfortunately">Unfortunately</a> about four months into the blog creation and after ~120 articles about programming and computer science concepts, he does not make it.  Even after answering all the questions correctly he was not offered the position(granted, there is no way to validate this other than taking his word for it).  Even if you are Steve Wozniak, Bill Gates, Linus Torvalds, Tim Berners-Lee, or Al Gore, sometimes they will reject you just because you didn't gel with one of the gatekeepers.  Maybe the gatekeeper was threatened by your intelligence, your smile, your dashing good looks, your charming voice, your chiseled chin, your flowing hair, whatever the case sometimes you get denied because you said, wrote, or did the wrong thing that may or may not be related to programming. It sucks, it happens, move on.

<h2>Conclusion</h2>
Be cool, remain positive, keep trying.