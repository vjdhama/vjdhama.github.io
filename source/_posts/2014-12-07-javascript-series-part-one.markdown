---
layout: post
title: "JavaScript series Part One"
date: 2014-12-07 15:56:02 +0530
comments: true
categories: ['javascript']
keywords: javascript 
description: The Basics
---

This is my first blog among a series of blog posts about javascript. The reasoning behind the blog series is to document and share the concepts I learn about javascript.

Lets start with the basics.

##What's JavaScript

Javascript is an object-oriented language with [first-class](https://en.wikipedia.org/wiki/First-class_functions) functions. In a nutshell, JavaScript is a dynamic scripting language supporting [prototype](#section2) based object construction.

##Objects

An object is a collection of properties and has a single `prototype` object. The prototype may be either an object or the `null` value.

JavaScript objects can be thought of as simple collections of name - value pairs. All primitive types except `null` and `undefined` are treated as objects.

Each name - value pair is called an *property*. That is property is named collection of attributes. The name part in any property is a JavaScript String and it is *unique* within that object. The value part can be primitive data types or other objects.

### Types of Properties

There are two type of properties

1. Data Properties
2. Accessor properties

####Data Properties

A data property is a property that can get and set a value. Data properties contain the value and writable properties in their descriptors.

Attributes for a data property descriptor.

| Data descriptor attribute| Description | Default |
| -- | -- | -- |
| **value** | The current value of the property. | **undefined** |
| **writable** |**true** or **false**. If **writable** is set to **true**, the property value can be modified. | **false** |
|** enumerable** |**true** or **false**. If **enumerable** is set to **true**, the property can be enumerated by a **for…in **statement. | **false** |
| **configurable** |  **true** or **false**. If **configurable** is set to **true**, property attributes can be changed, and the property can be deleted. | **false** |

If the data property is created using `Object.defineProperty`, `Object.defineProperties`, or `Object.create` functions, the **writable**, **enumerable**, and **configurable** attributes are all set to true.

####Accessor properties

An accessor property calls a user-provided function every time that the property value is set or retrieved. The descriptor for an accessor property contains a get attribute, a set attribute, or both.

Attributes for an accessor property descriptor.

| Accessor descriptor attribute| Description | Default |
| -- | -- | -- |
| **get** | A function that returns the property value. The function has no parameters. | **undefined** |
| **set** | A function that sets the property value. It has one parameter that contains the value to be assigned.  | **undefined** |
| **enumerable** |**true** or **false**. If enumerable is set to **true**, the property can be enumerated by a **for…in** statement. | **false** |
| **configurable**|  **true** or **false**. If configurable is set to **true**, property attributes can be changed, and the property can be deleted. | **false** |

When a get accessor is undefined and an attempt is made to access the property value, the value undefined is returned. When a set accessor is undefined and an attempt is made to assign a value to the accessor property, nothing occurs.


To obtain a descriptor object that applies to an existing property, you can use the [Object.getOwnPropertyDescriptor Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor).

>Starting in JavaScript 1.8.1, setters are no longer called when setting properties in object and array initializers.

##Theory in Action

###Creating a Object

You can create an object using an `object initializer`. Alternatively, you can first create a constructor function and then instantiate an object using that function and the new operator.

####Using object initializers

In JavaScript 1.2 and later, you can create an object using an object initializer.

The syntax for an object using an object initializer is:

``` js
var obj = { property_1:   value_1,   // property_# may be an identifier...
            2:            value_2,   // or a number...
            // ...,
            "property n": value_n }; // or a string
```

You can also add `getter` and `setter` using object initializer. All you need to do is to prefix a getter method with get and a setter method with set. For instance :

``` js
var o = {
  a: 7,
  get b() { return this.a + 1; },
  set c(x) { this.a = x / 2; }
};

o.c = 10 // Runs the setter, which assigns 10 / 2 (5) to the 'a' property
console.log(o.b) // Runs the getter, which yields a + 1 or 6
```
Getters and setters can also be added to an object at any time after creation using the `Object.defineProperties` method. Here's an example that defines the same getter and setter used in the previous example:

``` js
var o = { a:7 }

Object.defineProperties(o, {
    "b": { get: function () { return this.a + 1; } },
    "c": { set: function (x) { this.a = x / 2; } }
});

o.c = 10 // Runs the setter, which assigns 10 / 2 (5) to the 'a' property
console.log(o.b) // Runs the getter, which yields a + 1 or 6
```
####Using a constructor function

Using a constructor function a object can be created in two steps:

1. Write a constructor function to define a object.
2. Create a instance of object with new.

For instance, you would write the following function:

``` js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

```

Now you can create an object called mycar as follows:

``` js

var mycar = new Car("Eagle", "Talon TSi", 1993);
```

####Using the `Object.create` method

Objects can also be created using the `Object.create` method. Example :

``` js
var o = null;

o = Object.create(Object.prototype, {
  // foo is a regular 'value property'
  foo: { writable: true, configurable: true, value: 'hello' },
  // bar is a getter-and-setter (accessor) property
  bar: {
    configurable: false,
    get: function() { return 10; },
    set: function(value) { console.log('Setting `o.bar` to', value); }
  }
});

```


THAT'S IT !!

More to folow on next Blog.
