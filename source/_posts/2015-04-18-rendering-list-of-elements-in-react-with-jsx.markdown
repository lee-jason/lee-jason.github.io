---
layout: post
title: "Rendering List of Elements in React with JSX"
date: 2015-04-18 19:43:14 -0700
comments: true
categories: ['javascript', 'jsx', 'react', 'guide']
---
    
Understanding how JSX is processed can be slightly tricky to understand. Knowing when its appropriate to use JavaScript code and when to use HTML in your JavaScript can be very nuanced when writing React code. Understanding how the individuals of Javascript, JSX, and React come together is critical in writing basic React that will render in predictable ways every time. This guide will go over the slightly tricky scenario of getting a list to render and how it can help us understand more about React and JSX.

<!-- more -->
<style type="text/css">
  .react-example {
    margin: 0 auto;
    width: 500px;
    height: 300px;
    border: 1px solid #e3e3e3;
  }
</style>
<script src="http://fb.me/react-0.13.1.js"></script>
<script src="http://fb.me/JSXTransformer-0.13.1.js"></script>

<b><i>preface:</i></b> unfortunately, the jsx code highlighting may not be perfect. Each JSX React code example is followed by the transformed version to JavaScript, followed by the actual rendered code in the page. Feel free to inspect the elements to get a better idea of how React renders your code.

You may have landed here because you tried to render a list in React and wondered why it didn't work. You may have tried passing and rendering the list as a prop. Note how react by default places elements in a list in spans.

{% codeblock lang:javascript Unexpected Incorrect rendering of list in JSX %}
  var IncorrectListRender = React.createClass({
    render: function() {
      return (
        <ul>
          <li>{this.props.list}</li>
        </ul>
      )
    }
  });
  React.render(<IncorrectListRender list={[1,2,3,4,5]} />, document.getElementById('incorrect-list-render'));
{% endcodeblock %}
{% codeblock lang:javascript Unexpected Incorrect rendering of list in JavaScript %}
  var IncorrectListRender = React.createClass({displayName: "IncorrectListRender",
    render: function() {
      return (
        React.createElement("ul", null, 
          React.createElement("li", null, this.props.list)
        )
      )
    }
  });
  React.render(React.createElement(IncorrectListRender, {list: [1,2,3,4,5]}), document.getElementById('incorrect-list-render'));
{% endcodeblock %}
<div class="react-example" id="incorrect-list-render"></div>
<script type="text/jsx">
  var IncorrectListRender = React.createClass({
    render: function() {
      return (
        <ul>
          <li>{this.props.list}</li>
        </ul>
      )
    }
  });
  React.render(<IncorrectListRender  list={[1,2,3,4,5]} />, document.getElementById('incorrect-list-render'));
</script>

or you may have tried to individually pick out elements in the array and plant them in your render code. This would work, but its not very flexible. Notice how the second render call with the longer list still is only able to display the first five elements max.

{% codeblock lang:javascript Inflexible way of rendering lists in JSX %}
  var InflexibleListRender = React.createClass({
    render: function() {
      return (
        <ul>
          <li>{this.props.list[0]}</li>
          <li>{this.props.list[1]}</li>
          <li>{this.props.list[2]}</li>
          <li>{this.props.list[3]}</li>
          <li>{this.props.list[4]}</li>
        </ul>
      )
    }
  });
  React.render(<InflexibleListRender list={[1,2,3,4,5]} />, document.getElementById('inflexible-list-render1'));
  React.render(<InflexibleListRender list={[1,2,3,4,5,6,7,8,9,10]} />, document.getElementById('inflexible-list-render2'));
{% endcodeblock %}
{% codeblock lang:javascript Inflexible way of rendering lists in JavaScript %}
 var InflexibleListRender = React.createClass({displayName: "InflexibleListRender",
    render: function() {
      return (
        React.createElement("ul", null, 
          React.createElement("li", null, this.props.list[0]), 
          React.createElement("li", null, this.props.list[1]), 
          React.createElement("li", null, this.props.list[2]), 
          React.createElement("li", null, this.props.list[3]), 
          React.createElement("li", null, this.props.list[4])
        )
      )
    }
  });
  React.render(React.createElement(InflexibleListRender, {list: [1,2,3,4,5]}), document.getElementById('inflexible-list-render1'));
  React.render(React.createElement(InflexibleListRender, {list: [1,2,3,4,5,6,7,8,9,10]}), document.getElementById('inflexible-list-render2'));
{% endcodeblock %}
<div class="react-example" id="inflexible-list-render1"></div>
<div class="react-example" id="inflexible-list-render2"></div>
<script type="text/jsx">
  var InflexibleListRender = React.createClass({
    render: function() {
      return (
        <ul>
          <li>{this.props.list[0]}</li>
          <li>{this.props.list[1]}</li>
          <li>{this.props.list[2]}</li>
          <li>{this.props.list[3]}</li>
          <li>{this.props.list[4]}</li>
        </ul>
      )
    }
  });
  React.render(<InflexibleListRender list={[1,2,3,4,5]} />, document.getElementById('inflexible-list-render1'));
  React.render(<InflexibleListRender list={[1,2,3,4,5]} />, document.getElementById('inflexible-list-render2'));
</script>


A good solution would be flexible to the amount of items in a list and would render with its own containing element in a repeating fashion. Notice how the following render function is able to accomodate arrays of all sizes. This is due to rendering in a list of React Elements created from the list of numbers coming in from the props.

{% codeblock lang:javascript Properly Repeatable React Elements in JSX %}
  var ProperListRender = React.createClass({
    render: function() {
      return (
        <ul>
          {this.props.list.map(function(listValue){
            return <li>{listValue}</li>;
          })}
        </ul>
      )
    }
  });
  React.render(<ProperListRender list={[1,2,3,4,5]} />, document.getElementById('proper-list-render1'));
  React.render(<ProperListRender list={[1,2,3,4,5,6,7,8,9,10]} />, document.getElementById('proper-list-render2'));
{% endcodeblock %}
{% codeblock lang:javascript Properly repeatable React Elements in JavaScript %}
  var ProperListRender = React.createClass({displayName: "ProperListRender",
    render: function() {
      return (
        React.createElement("ul", null, 
          this.props.list.map(function(listValue){
            return React.createElement("li", null, listValue);
          })
        )
      )
    }
  });
  React.render(React.createElement(ProperListRender, {list: [1,2,3,4,5]}), document.getElementById('proper-list-render1'));
  React.render(React.createElement(ProperListRender, {list: [1,2,3,4,5,6,7,8,9,10]}), document.getElementById('proper-list-render2'));
{% endcodeblock %}
<div class="react-example" id="proper-list-render1"></div>
<div class="react-example" id="proper-list-render2"></div>
<script type="text/jsx">
  var ProperListRender = React.createClass({
    render: function() {
      return (
        <ul>
          {this.props.list.map(function(listValue){
            return <li>{listValue}</li>;
          })}
        </ul>
      )
    }
  });
  React.render(<ProperListRender list={[1,2,3,4,5]} />, document.getElementById('proper-list-render1'));
  React.render(<ProperListRender list={[1,2,3,4,5,6,7,8,9,10]} />, document.getElementById('proper-list-render2'));
</script>

The solution above is flexible enough to accomodate as many or as little list items that comes from the props field.

Lets break it down to see why it works

First each React class object needs to have a render function. Think of the render like a screen refresh. The render function is supposed to return a value or list of values (most commonly composed of JSX elements) that will be drawn onto the screen. The render function is automatically called whenever the object's state is signalled to change. States are outside the scope of this guide but you can read more about state <a href="https://facebook.github.io/react/docs/tutorial.html#reactive-state">here</a>. 

Notice how html is plainly inserted into a JavaScript function. The JSX transformer/compiler will automatically detect HTML and React tags inside JavaScript and convert them to the equivalent JavaScript expression. If you would like to read more about the end result feel free to <a href="https://facebook.github.io/react/jsx-compiler.html">try out</a> the compiler and <a href="https://facebook.github.io/react/docs/jsx-in-depth.html">read up</a> on what it does to your tags.

Also take note of the curly brackets peppered through out the html tags. The brackets signify to the JSX transformer that there is actual JavaScript inside of it. Combining JSX and JavaScript together allows us to use the ease of HTML tagging and the logical power of JavaScript to create dynamic HTML templates right inside of our React code. In the case of the list example, note how we start with a containing <code>&lt;ul>...&lt;/ul></code> block. This <code></ul></code> block is then split with a pair of brackets that perform a <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map">mapping</a> to create and return a new list of html <code>&lt;li>{listValue}&lt;/li></code>elements.  Also notice again how <code>{listValue}</code> had to be surrounded again in brackets since its embedded within html elements. If you want to know more about what you can do with React Elements take a look <a href="https://facebook.github.io/react/docs/top-level-api.html#react.createelement">here</a>.

<h2>Conclusion</h2>
The React renderer is actually quite flexible. You don't necessarily have to feed it just Numbers or just React Elements but can mix and match all sorts of JavaScript objects. You can filter and parse the original objects in your list and use JavaScript logic to render a completely new list with dynamic React Elements. Truly understanding how your JSX code will be compiled to plain old JavaScript is the key to writing effective and predictable React everytime.