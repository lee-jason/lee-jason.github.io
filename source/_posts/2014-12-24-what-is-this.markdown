---
layout: post
title: "What is this?"
date: 2014-12-24 21:49:29 -0800
comments: true
categories: ['guide', 'javascript']
---
The <code>this</code> keyword in JavaScript is probably one of the most confusing and misunderstood fundamental concepts of the language.  It's honestly not something that's too terribly intuitive or something that's understood even after reading several articles.  The process of completely learning about <code>this</code> and its ramifications to the language is a long journey of reading and experimenting.  Here's hopefully an easy explanation for <code>this</code>.

<!-- more -->
The reference to <code>this</code> is very dynamic.  <code>this</code> can reference any object depending on whether we specify the reference explicitly in the code, or whether we leave it up to the ECMA standards to assume what we want <code>this</code> to reference.  Most of the times we leave the <code>this</code> reference to be decided by the interpreter which is where most of our issues and confusion comes in. <code>this</code> is different in a few different contexts, here's the main contexts you'll be seeing <code>this</code> used in and how you can make it work predictably for you each time.

<h2><code>this</code> in a constructor</h2>
Here's what happens when you use the <code>new</code> keyword to create a new object in JavaScript.  The following text is an altered and truncated version from the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new">Mozilla Developer Network</a> on what happens when creating objects with <code>new</code>.

1.  A new object is created, inheriting from Object.prototype.
2.  The constructor function Object is called with the specified arguments and this bound to the newly created object...

The MDN docs uses an interesting word, 'bound'.  From this point on, whenever you see the word bound, you should associate it with setting the <code>this</code> reference.  If I say functionA is bound to ObjectA then you should think that functionA's <code>this</code> === ObjectA.  The MDN documentation says that it first creates an object using Object.create(), this step is not really that interesting as it relates to <code>this</code>, it's just merely setting up an object for us to bind to. Then it executes the constructor function with the <code>this</code> being the shell of the object.  Remember that constructors are just functions after all.  What this means is that when the function is executed, the interpreter starts attaching objects to the empty object. The example below codes out what the MDN doc is saying

{% codeblock lang:javascript Creating a new Person with the new keyword  %}
    var Person = function(){
        this.age = 20;
        this.name = 'Jason';
    }
    var person = new Person();
    console.log(person.age) //20
    console.log(person.name //'Jason'
{% endcodeblock %}

The above code is equivalent to the code below

{% codeblock lang:javascript Creating a new Person using Object create and object binding  %}
    var Person = function(){
        this.age = 20;
        this.name = 'Jason';
    }
    var person = Object.create(Person.prototype);
    //this is where the binding happens 
    Person.call(person);
    console.log(person.age) //20
    console.log(person.name) //'Jason')
{% endcodeblock %}

Note in the code above this line <code>Person.call(person)</code>.  What that line means is that the <code>Person</code> function is executing and explicitly making the empty <code>person</code> object equal to <code>this</code>. This means that after that function call, the once empty <code>person</code> object now has two properties that were applied to it, <code>person.age</code> and <code>person.name</code>. The following bit of code is identical to what the line <code>Person.call(person)</code> is doing above.

{% codeblock lang:javascript Call Comparison  %}
    var person = {}
    var Person = function(){
        person.age = 20;
        person.name = 'Jason';
    }
    Person();
    console.log(person.age) //20
    console.log(person.name) //'Jason')
{% endcodeblock %}

If you need more information about the function method <code>call</code> consult the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call">documentation</a>.

You may be asking, what if the function was just executed without specifically binding the <code>this</code> reference? Well that's what we will find out in the next topic

<h2><code>this</code> in a function</h2>
So what if we execute a function with <code>this</code> without explicitly defining a binding?  Then we leave it up to the mercy of the interpreter.  Most of the time, <code>this</code> will be refered to the global object.  The global object varies between JavaScript interpreters. In the browser the global object will refer to the <code>window</code> object, in Node.js it'll refer to <code>global</code> object.  In either case, unknowingly attaching objects to the global object is probably not what you wanted to do.

{% codeblock lang:javascript Executing unbound function %}
    var unboundFunction = function(){
        this.param1 = 1;
        this.param2 = 2;
    }
    unboundFunction();
    console.log(window.param1) //1
    console.log(window.param2) //2
{% endcodeblock %}

Note how when the function was executed without a binding, it automatically defaulted to the global object which in this case is <code>window</code>.

With that said, you now are empowered about everything there is to know about <code>this</code>. I can't stress how important it is that we know how our <code>this</code> is being binded, explicitly or implicitly.  Now that we know everything there is to know about <code>this</code>, here's some examples of where things get tricky

<h2>Bind when passing around functions</h2>
A good rule of thumb is to always bind objects to functions when passing them around as parameters or when returning them.  Often times functions are passed around and users are shocked to see that their <code>this</code> doesn't actually reference their expected object.  To prevent this from happening to you, use the function method <code>bind</code>. 

The following example passes a function to an <code>Executor</code> function that runs any function that gets passed into it.  We create a person object and pass in its <code>growUp</code> function to the Executor where we think it will update our person's age.  

{% codeblock lang:javascript Executing unbounded function %}
    //a function that executes any function passed into it
    var Executor = function(func){
        console.log('executing function');
        func();
    }
    //creation of a person object with a growUp method that ages the person object by 1.
    var Person = function(){
        this.age = 10;
        this.growUp = function(){
            this.age += 1;
        }
    }
    var person = new Person();

    //confirm the property window.age to see that its undefined before method execution
    console.log(window.age) //undefined

    Executor(person.growUp);

    console.log(person.age) //10
    console.log(window.age) //NaN
{% endcodeblock %}

As you can see from the example above, passing the person's growUp function to be executed by the <code>Executor</code> causes the function to bind the global object to <code>this</code> by default.  That is why <code>person.age</code> doesn't change, and <code>window.age</code> went from <code>undefined</code> to <code>NaN</code>. If we explicitly define the binding before passing the function, then we can ensure that the <code>growUp</code> function will use the person object no matter where it's executed.

{% codeblock lang:javascript Executing unbounded function %}
    //a function that executes any function passed into it
    var Executor = function(func){
        console.log('executing function');
        func();
    }
    //creation of a person object with a growUp method that ages the person object by 1.
    //note the .bind at the end of the growUp function
    var Person = function(){
        this.age = 10;
        this.growUp = function(){
            this.age += 1;
        }.bind(this);
    }
    var person = new Person();

    //confirm the property window.age to see that its undefined before method execution
    console.log(window.age) //undefined

    Executor(person.growUp);

    console.log(person.age) //11
    console.log(window.age) //undefined
{% endcodeblock %}

Note how the age now properly adds on the person object and how the window object is not affected.  This is because we use the <code>bind</code> method that every function has.  This specifies which object to use to reference <code>this</code> in the function.  
This example is a little contrived but it's definitely important to keep in mind especially when dealing with callback code or asynchronous JavaScript where functions are passed around like candy.

<h2>Careful about functions that return anonymous functions</h2>

Be careful about your <code>this</code> reference when dealing with functions within functions.  The following example is super contrived but here's the rundown.  Imagine a person object that's at a holiday party and has a property for which drink he's having.  A person that's under 21 will be drinking eggnog while a person 21 or over would be drinking booze. The person has a <code>drinkFunction</code> that will return him another function that updates the state of his drink to either 'eggnog' or 'booze'. (I understand that this can be done with a single function but please bear with me.)

{% codeblock lang:javascript Executing unbounded function in function %}
    var Person = function(){
        this.age = 10;
        this.drinking = '';
        //top level functions like these are autobinded by default to whatever they're attached to.  In this case, the same 'this' object that the constructor binds to.
        this.drinkFunction = function(){
            //'this.age' resolves correctly and pulls the age property of the current object due to the function properly binding to the corresponding person object.
            if(this.age < 21){
                //this function is not bound to anything. It defaults to binding 'this' to the global object
                return function(){
                    this.drinking = 'eggnog';
                }
            }
            else{
                //this function is not bound to anything. It defaults to binding 'this' to the global object
                return function(){
                    this.drinking = 'booze';
                }
            }
        }
    }
    var person = new Person();

    //confirm the property window.drinking to see that its undefined before method execution
    console.log(window.drinking) //undefined
    //execute the drinkFunction as well as the function that it returns
    person.drinkFunction()();

    console.log(person.drinking) //''
    console.log(window.drinking) //'eggnog'
{% endcodeblock %}

Note how once again, the person.drinking object wasn't the one that was affected, but rather the one in the global window.  This is because we executed the function that we returned in the global state meaning the <code>this.drinking</code> property in the inner returned function was by default bound to the <code>window</code> object when we executed it. If we want the <code>this</code> object to reference the same person object that's executing it, we either need to <code>bind</code> the internal returned function or <code>call</code> the function when executing.  Take a look how at the example below.


{% codeblock lang:javascript Executing function with binded anonymous function %}
    var Person = function(){
        this.age = 10;
        this.drinking = '';
        this.drinkFunction = function(){
            if(this.age < 21){
                //note the binds to the same top level 'this' object.  This ensures that the anonymous function references the same 'this' object that gets created when new Person is constructed
                return function(){
                    this.drinking = 'milk';
                }.bind(this)
            }
            else{
                //note the binds to the same top level 'this' object.  This ensures that the anonymous function references the same 'this' object that gets created when new Person is constructed
                return function(){
                    this.drinking = 'booze';
                }.bind(this)
            }
        }
    }
    var person = new Person();
    //execute the drinkFunction as well as the function that it returns
    person.drinkFunction()();

    console.log(person.drinking) //'eggnog'
    console.log(window.drinking) //undefined
{% endcodeblock %}

The code up top is also equivalent to the following

{% codeblock lang:javascript Executing function with anonymous function by 'call'ing %}
    var Person = function(){
        this.age = 10;
        this.drinking = '';
        this.drinkFunction = function(){
            if(this.age < 21){
                return function(){
                    this.drinking = 'milk';
                }
            }
            else{
                return function(){
                    this.drinking = 'booze';
                }
            }
        }
    }
    var person = new Person();
    //note how even without binds we can set the 'this' object inside the anonymous functions using call
    person.drinkFunction().call(person);

    console.log(person.drinking) //'eggnog'
    console.log(window.drinking) //undefined
{% endcodeblock %}


I know that even when trying to explain it in the most normal non technical terms possible, there definitely is still a lot of confusion that arises from understanding <code>this</code> entirely.  What I highly recommend is to open up your JavaScript console, REPL, inspector, whatever and seriously try creating constructors, functions, functions in functions and executing them using binds, calls or applies, and see how they affect the object that you bind to. If you want to learn more about binding, consult the documentation for <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind">bind</a>, <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call">call</a>, and <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply">apply</a>. Each are applicable in their own uses which exceed the scope of this guide.