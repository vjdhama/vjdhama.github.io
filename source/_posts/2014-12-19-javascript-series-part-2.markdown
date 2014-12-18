---
layout: post
title: "JavaScript series Part 2"
date: 2014-12-19 01:42:52 +0530
comments: true
categories: 
---

In the last blog I discussed about basics of JavaScript including objects, object properties, object creation etc. Continuing on the same basics now.

###More on properties

The method `Object.defineProperty()` allows precise addition to or modification of a property on an object. As explained in the previous blog :

>If the data property is created using `Object.defineProperty`, `Object.defineProperties`, or `Object.create` functions, the *writable*, *enumerable*, and *configurable* attributes are all set to true.

For Example :

``` js
var  person = {"Name" : "Dark Lord"};
```

**Console Log**

``` js
person.Name
"Dark Lord"
person.Name = "Ronan"
"Ronan"
```

If the object is created like :

``` js
var  person = Object.create({},
    "Name", { value : "Dark Lord"});
```

Since by default properties ARE NOT writable, enumerable or configurable, the `person` name cannot be changed. Name  property will also not appear in `Object.keys()`.

**Console Log**

``` js
person.Name
"Dark Lord"
person.Name = "Ronan"
"Ronan"
person.Name
"Dark Lord"
Object.keys(person)
[]
```

###Interdependent properties

While using getters and setters, I found a interesting feature related to them.

Now suppose have some money in your wallet. Let's consider you have 200 dollars. Now we are going to represent that information in terms of a object.

```js
    my_money = {"dollars" : 200}
```

Now let's just consider a hypothesis, that i am frequent flyer between Europe and USA. So when i am in Europe i have to know how much money do i have in euros. So i'll try to demostrate that using below code snippet.

``` js
    Object.defineProperty(my_money,
        'euros',{
        get: function(){return this.dollars / 1.24},
        set: function(value){this.dollars = value * 1.24},
        enumerable: true
    });
```

The properties `dollars` ans `euros` are interdependent now. Whenever value of `dollars` is changed, the value of `euros` changes and vice-versa. So this code snippet is a simple currency converter.

**Console Output**
``` js
my_money.dollars
200
my_money.euros
161.29032258064515
my_money.euros = 180
180
my_money.dollars
223.2
my_money.dollars = 300
300
my_money.euros
241.93548387096774
```

###Type coercion (crazy stuff)

In JavaScript, we have so called falsy values. These are respectively: `0`, `null`, `undefined`, `false`, `""`, `NaN`. Falsy values are important to understand type coercion. Due to type coercion numbers can become strings, strings can become numbers, and arrays can become strings.

Lets start with a code snippet.

**Console**

``` js
a = +[];
a
0
```

Above happens because `[]` in a string context becomes `""`, and since unary `+` operator converts its operand to Number type, the value of `a` becomes `0`.

OK that was understandable. But there are cases which are not easily understandable, like

``` js
a = +[ [] ];
a
0
```

Well you'd probably ask me what the heck just happened?

Don't worry about above result, it has perfectly good explanation. The outer empty square brackets force the inner ones to a number, since it is empty, it coerces into `0`. So the expresion reduces to `+[ 0 ]`. And we know how that is resolved to value `0` from previous snippet.

An interesting thing to note is that on evaluating `++[ [] ]` will give a `ReferenceError`, since `++` is bound by the rule that it should always evaluate to a reference and `[]` exits nowhere.

>Literals, always evaluate to values: `1`, `"foo"`, `true`, `({ x: 1 })`, and `[]` are all values. Expressions involving property accessors, on the other hand — `foo.bar`, `[1][0]`, `({ x: 1 }).x` — are all references.


Sticking `[0]` at the end will solve the `ReferenceError`.

``` js
++[ [] ] [ 0 ]
1
```
Now, lets swap 0 for `+[]`. Thus we get the final result:

```js
++[ [] ] [ +[] ]
1
```

More about coercion : [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Using_the_Equality_Operators), [Javascriptweblog](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/) and
[Ben Nadel blog](http://www.bennadel.com/blog/2407-javascript-isnan-coerces-values-when-testing-for-numbers.htm).
