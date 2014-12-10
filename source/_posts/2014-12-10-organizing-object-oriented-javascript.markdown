---
layout: post
title: "Organizing Object Oriented Javascript"
date: 2014-12-10 12:18:49 -0800
comments: true
categories: ['javascript', 'guide']
---
Object oriented Javascript isn't really a new concept.  For years people have been trying to make Javascript more familiar towards object oriented programmers by perverting its naturally prototypical architecture towards something more familiar. There are many guides on how to create classes with private and public members and inheritance but there are very few guides on how to organize these into an actual usuable and familiar project structure.  I think the primary problem in why developers have so many varying ways of organizing their Javascript code is in that the language was never really meant to be built to such huge degrees.  Large scale Javascript projects are popping up with the rise in popularity of NodeJS and large scale front end frameworks that try to organize the usual mess that comes from hacking together a 'native' application experience on the web. I've been trying to organize my js code for my projects and I've been looking around to find a familiar ground in how I organize Javascript in that's easy and makes sense (at least to me).

<!-- more -->

Javascript can simulate classes, but how do you organize them in files?  Native Java can support a single class in a single file as well as any additional subclasses with in that file.  The way that different classes are brought together in Java is by importing class dependencies before each file.  Unfortunately at this current state there is no largely supported 'import' feature for Javascript which brings Javacript developers out of the woodwork to make up functionality for it.  <a href="http://requirejs.org/">RequireJS</a> seems to be a pretty good module loader that emulates the functionality of an import. This means that its no longer necessary to rely on properly ordering your \<script\> tags in your html in the order of dependencies, but rather you can do this directly in the Javascript files themselves. This means that you can just include a single \<script\> on the page.  This file will be the entry point of the program and simulates the <code>public static void main(String args[])</code> function for Java. This main.js file will contain all dependencies of the program, its as if all your js code is written on a single page (but its not because that would be bad).

{% codeblock lang:main.js %}
    //placeholder for example require.js code
{% endcodeblock %}

Okay so now we have a way to 'import' js files together and specify dependencies, now we need to see what exactly we expose in these files.  Javascript has ways of hiding information from outside classes but RequireJS also has the option to choose which paramters/functions to expose to the outside world.  I've been taking a look at the <a href="https://github.com/adobe/brackets">Brackets</a> source code and the approach that they take is to expose public methods and properties through RequireJS's export object. This works as a method of information hiding but I think the problem with this is that instead of using Javascripts construct of information hiding by using scoped variables in functions, the decision to create public or private variables/methods is left up to whether the human behind the code wants to expose it.  It also changes the traditional view of Javascript classes meaning someone who is already familiar with Javascript will have to refamiliarize with how a RequireJS project is set up since methods and variables don't have to be encapsulated within a class function.  Then again there is always a learning curve when an unfamiliar framework or library is used in a project.

RequireJS changes how we expose or hide methods in js. All properties and functions are defined in a <code>define(function(require, exports, module){})</code>. At the end of the function, we 'export' all variables that we want to be public and exposed from this file.  This method works as you can see from the <a href="https://github.com/adobe/brackets">Brackets source code</a> but it assumes that the methods exposed through this object is from a singleton object.  Further internal classes can be made for each singleton object and exposed through RequireJS <code>export</code> object. A good file to look at for an example would be the <a href="https://github.com/adobe/brackets/blob/master/src/command/Menus.js">Menus.js</a>. Checkout the exports at the bottom of the page, it exposes some of the variables and functions defined earlier but also keeps ignores other keeping those private.  As a naming convention they prefix these private attributes with an '_'.  The export also exposes inner classes.  These classes are created in a more traditional object oriented javascript fashion as in through a function. Check out its <a href="http://brackets.io/docs/current/modules/command/Menus.html#classes">api page</a>.

{% codeblock lang:main.js %}
    //placeholder for example require.js code
{% endcodeblock %}

I know there's many ways to organize a Javascript project but I don't agree with Bracket's implementation entirely. I think the way that Brackets exports the file as an already created singleton object is something that's not entirely familiar with people coming in from Java/C# backgrounds.  While Javascript is really good about creating singleton objects, it'd be more familiar for Java programmers to only expose the constructor function for each file. Each Javascript class would then be created in the traditional <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript">Javascript fashion</a> and methods would be <a href="http://javascript.crockford.com/private.html">public or private</a> based off the standard Javascript way.  I think this method of project organization would be more simpler due to people not having to adjust their ideals on what they learned about object oriented Javascript so far while also practicing a separation of code into its own files.

Meaning say a unfamiliar javascript programmer created a new function constructor object and assigned all properties to its own object.

{% codeblock lang:main.js %}
    function person(){
        this.private = 'secret';
        this.public = 'public';
    }
{% endcodeblock %}