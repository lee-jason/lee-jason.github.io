---
layout: post
title: "Adding validation for embedded array in array objects in mongoose"
date: 2014-10-23 23:12:09 -0700
comments: true
categories: ['javascript', 'mongoose', 'guide']
---

Surprisingly there was very little information I could find when googling this issue.  Granted, it's a very niche issue but here's what I personally found when trying things out.  If you're unfamiliar with mongoose's validation please consult the <a href="http://mongoosejs.com/docs/validation.html">guide</a> first.  This document assumes some familiarity with mongoose schemas, embedded schemas, and validation.

<!-- more -->

There's a lot of pre-explanation. if you want to go directly to the answer head to the <a href="#arrayInArray">end of the page</a>

So in mongoose, there's several ways to define a schema.  Once a Schema is defined, you can require mongoose to put custom validation on the object before it gets saved into the database. Mongoose documentation recommends you use the <a href="http://mongoosejs.com/docs/api.html#schema_Schema-path"><code>Schema#path(path, constructor)</code></a> function to get the SchemaType object where you can then use <a href="http://mongoosejs.com/docs/api.html#schematype_SchemaType-validate"><code>SchemaType#validate(obj, [errorMsg])</code></a> on.  The problem occurs when your path branches into an array.

The following examples are attempting to model a Car object that can have one or more features.
{% codeblock lang:js creating a schema with an array of an embedded object %}
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
var CarSchema = new Schema({
    name: {
        type: String,
        required: true,
    },
    features: [{
        name: {
            type: String,
            required: true //this 'required' option only implies that if creating an object in the features array, the object better have a 'name' attribute in it.  Does not mean that the 'features' array is required.
        }
    }]
});

mongoose.model('Car', CarSchema);
{% endcodeblock %}

the following below creates two schemas, car and features, and puts a reference of features in cars similar to how typical RDBMS work.
{% codeblock lang:js creating a schema with an array by creating multiple schemas %}
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
var CarSchema = new Schema({
    name: {
        type: String,
        required: true,
    },
    features: [{
        type: Schema.ObjectId,
        ref: Feature
        required: true; //will make it so that the feature's array will be required to save this document
    }]
});

var FeatureSchema = new Schema({
    name: {
        type: String,
        required: true
    }
});

mongoose.model('Car', CarSchema);
mongoose.model('Feature', FeatureSchema);
{% endcodeblock %}

Each method of creating mongoose objects has some benefits and tradeoffs.
<h4>Embedded Object in Single Schema</h4>
<h5>Pros</h5>
<ul>
    <li>data access is simpler, requires no joining of documents</li>
    <li>less boilerplate code to write</li>
</ul>
<h5>Cons</h5>
<ul>
    <li>If data is large, can get messy</li>
    <li>Not possible to set SchemaType options directly in Schema</li>
</ul>

<h4>Objects split between multiple Schemas</h4>
<h5>Pros</h5>
<ul>
    <li>Similar to RDBMS normalization structure</li>
    <li>data is more scalable and reusable</li>
    <li>Mongoose supports the addition of validation through SchemaType options in the Schema declaration itself</li>
</ul>
<h5>Cons</h5>
<ul>
    <li>Increased amount of boilerplate code needed for Schema creation.</li>
    <li>filesystem can get messy with large amount of tables required for a simple object.</li>
</ul>

<h2>Lets talk about adding validation</h2>

Lets say that we're trying to add a 'required' validation.  This validation will check to see if the Car object has a features array with a feature in it.  If it doesn't, the save will fail, if it does, then the document will be stored in the database.

{% codeblock lang:js Adding validation to embedded Schema objects %}
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
var CarSchema = new Schema({
    name: {
        type: String,
        required: true,
    },
    features: [{
        name: {
            type: String,
            required: true
        }
    }]
    
});

CarSchema.path('features').validate(function(features){
    if(!features){return false}
    else if(features.length === 0){return false}
    return true;
}, 'Car needs to have at least one feature');

mongoose.model('Car', CarSchema);
{% endcodeblock %}

When embedded objects are used to define a schema, you first have to use <a href="http://mongoosejs.com/docs/api.html#schema_Schema-path"><code>Schema#path(path, constructor)</code></a> to get the Schema object and then use its validate function.

As for the second example, the validation is already set with the 'required' option being set on the 'features' attribute.  By not declaring the Schema internally, you can place options such as 'required' and 'validate' in the Schema declaration. Refer to <a href="http://mongoosejs.com/docs/api.html#schematype-js">this page</a> to see what other <code>SchemaType</code> options there are.

{% codeblock lang:js Adding validation module Schema declarations %}
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

//the custom validation that will get applied to the features attribute.
var notEmpty = function(features){
    if(features.length === 0){return false}
    else {return true};
}

var CarSchema = new Schema({
    name: {
        type: String,
        required: true,
    },
    features: [{
        type: Schema.ObjectId,
        ref: Feature
        required: true; //this will prevent a Car model from being saved without a features array.
        validate: [notEmpty, 'Please add at least one feature in the features array'] //this adds custom validation through the function check of notEmpty.
    }]
});

var FeatureSchema = new Schema({
    name: {
        type: String,
        required: true //this will prevent a Feature model from being saved without a name attribute.
    }
});

mongoose.model('Car', CarSchema);
mongoose.model('Feature', FeatureSchema);
{% endcodeblock %}

<h2 id="arrayInArray">Adding validation to a Schema with an array in an array</h2>
What if you had an embedded Schema that declared an embedded object in an array that declared another embedded object inside of another array.  This is the tricky part that's not very well documented.

{% codeblock lang:js Adding validation to embedded Schema objects %}
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
var CarSchema = new Schema({
    name: {
        type: String,
        required: true,
    },
    features: [{
        name: {
            type: String,
            required: true
        },
        model: [{
            year: {
                type: Number
            }
        }]
    }]
    
});

CarSchema.path('features').schema.path('model').schema.path('year').validate(function(features){
    if(!features){return false}
    else if(features.length === 0){return false}
    return true;
}, 'Car needs to have at least one feature');

mongoose.model('Car', CarSchema);
{% endcodeblock %}

You're unable to specify the path when the object is behind arrays.  Accessing the Schema in the array itself cannot be accessed like <code>CarSchema.path('features.model.year')</code>, but it can be accessed by grabbing the <code>schema</code> attribute which returns the <code>SchemaType</code> of the object in the array.  Now that we have the <code>SchemaType</code> object we apply its path to continue the chain until we get the attribute that we want to apply the validation for. As far as I know, the <code>DocumentArray.schema</code> attribute is not a documented feature.