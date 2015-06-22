---
layout: post
title: "Logging information on browser crashes"
date: 2015-06-21 13:25:04 -0700
comments: true
categories: ['browsers', 'javascript']
---

Every now and then your web application does something so wild and unpredictable that it crashes the browser that you're running it on. In order to create a better product for our users, we would need to log pertinent information every time our app crashes. Unfortunately there is no way to send a crash log before or during the crash due to the unpredictable nature of the crash and the browser's web environment no longer working. The best thing to do is to send the logs <i>after</i> the crash. This post will go through a technique to detect when the user's previous session has crashed so that we can perform the relevant logging actions.

<!-- more -->

<h2>Preface</h2>
Currently I've only been able to support this technique on Google's Chrome and Mozilla's Firefox browser. This technique takes advantage of these two browser's support for restorable sessionStorage. The <a href="https://wiki.whatwg.org/wiki/FAQ#What_is_the_WHATWG.3F">WHATWG</a> HTML spec currently describes sessionStorage to delete itself once the browser context ends but also makes a note that browsing contexts can continue to persist even after the browser is closed. This is useful if the browser chooses to save the session to reload it after the user closes the browser window or if the browser window crashes. 
<a href="https://html.spec.whatwg.org/multipage/webstorage.html#the-sessionstorage-attribute">WHATWG spec on sessionStorage</a>

<h2>Simulating the Crash</h2>
First, we'll have to find out ways to simulate crashes on our browsers. Fortunately for our sake, its pretty easy to do for both Mozilla and Chrome.

<h3>Crashing on Chrome</h3>
<ol>
	<li>Go to the page you want to crash<li>
	<li>Paste this into your url 'chrome://crash'</li>
	<li>Press Enter</li>
</ol>
That was pretty easy. This brings up Chrome's 'Aw Snap!' error page which simulates as if an error actually happened on the previous site you were on. 

<h3>Crashing on Firefox</h3>
<ol>
	<li>Go to the page you want to crash</li>
	<li>Open your Task Manager on Windows or Activity Monitor on OSX</li>
	<li>Find your firefox process and end the process or force quit it</li>
</ol>
Force stopping the processes trigger's Firefox's crash conditions and emulates a real crash scenario. Firefox should ask you to restore your previous tabs along with your previous session as well.

<h2>Acting on the Crash</h2>
Now that we know how to crash the browsers we just need to place the code in.

{% codeblock lang:javascript Logging on browser crash  %}
   window.addEventListener('load', function () {
   	sessionStorage.setItem('good_exit', 'pending');
   	sessionStorage.setItem('last_visited_url', window.location)
   });

   window.addEventListener('beforeunload', function () {
   	sessionStorage.setItem('good_exit', 'true');
   });
   
   if(sessionStorage.getItem('good_exit') && 
      sessionStorage.getItem('good_exit') !== 'true') {
   	/*
   		insert crash logging code here
   	*/
   }
{% endcodeblock %}

What the code above is doing is checking everytime that the user successfully closes, refreshes, or browsers to another page we make a note of it by applying a <code>good_exit = true</code> flag in our sessionStorage. When the user successfully loads the page, the 'good_exit' flag will be set to 'pending' as we're not sure if the user will successfully exit from this session. If the user unsuccessfully closes, refreshes, or browsers to another page and manages to crash the browser, the 'good_exit' flag will not be changed to 'true' and will stay as 'pending. Everytime the user loads the page we check whether they're coming back from a crash. If they are, then we apply our crashalytics code.

<h2>Things to do after a crash</h2>
Remember that your sessionStorage is saved and recovered so anything that your user was doing before the crash can be recovered.  This means you can store the last visited url, the time that they visited your site, any actions they taken on the page, anything that you would be able to store in a cookie you can store in a sessionStorage. After you recover this information, you can check for any temporary work done on the page so you can reload it. Say for instance if the user was typing an essay in your textbox but had the browser crash, you can save the contents of the textbox every few seconds so that you can reload it once the user comes back. Or if you just want some analytics on the last actions the user took you can create a sessionStorage item to log and trace the actions taken by the user before the browser crashed. When the visitor returns you can post that information back to your server so you can fix your buggy pages. Or when a user comes back to your page you can directly ask them to submit a bug report about what they were doing before they crashed. Many native desktop software already do something like this and now you too can bring this feature to your buggy web application!

<h2>Caveats</h2>
An already listed Caveat is this does not currently work for Internet Explorer or Safari. I wasn't able to reliable crash those browsers to test whether they bring back sessions after crashes. Another caveat is that since sessionStorage content isn't shared between tabs, users will have to refresh or reload the same tab with the same sessionStorage in order for the crash logging code to execute. If users open a new tab to connect after a crash then there will be no record of there being a crash. It will act as if you started a new browser session. As you can see this method isn't 100% reliable, but it does give options as to what you can do.