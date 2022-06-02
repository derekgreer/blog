---
id: 835
title: 'SOLID JavaScript: The Liskov Substitution Principle'
date: 2011-12-31T16:00:32+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=835
permalink: /2011/12/31/solid-javascript-the-liskov-substitution-principle/
dsq_thread_id:
  - "629997709"
categories:
  - Uncategorized
tags:
  - Design Principles
  - JavaScript
  - SOLID
---
<div style="border-bottom: black 1px solid; border-left: black 1px solid; float: right; margin-left: 4px; border-top: black 1px solid; border-right: black 1px solid" class="toc">
  <u>Series <noindex></noindex> Table of Contents</u> <br /><a href="/2011/12/08/solid-javascript-single-responsibility-principle/">The Single Responsibility Principle</a> <br /><a href="/2011/12/19/solid-javascript-the-openclosed-principle/">The Open/Closed Principle</a> <br />The Liskov Substitution Principle <br /><a href="/2012/01/08/solid-javascript-the-interface-segregation-principle/">The Interface Segregation Principle</a> <br /><a href="/2012/01/22/solid-javascript-the-dependency-inversion-principle/">The Dependency Inversion Principle</a>
</div>

This is the thrid installment in the SOLID JavaScript series which explores the SOLID design principles within the context of the JavaScript language. In this installment, we’ll be covering the Liskov Substitution Principle.

<div style="clear: both">
  &#160;
</div>

## The Liskov Substitution Principle

The Liskov Substitution Principle relates to the interoperability of objects within an inheritance hierarchy.&#160; The principle states:

> Subtypes must be substitutable for their base types.

In object-oriented programming, inheritance provides a mechanism for sharing code within a hierarchy of related types.&#160; This is achieved through a process of encapsulating any common data and behavior within a base type, and then defining specialized types in terms of the the base type.&#160; To adhere to the Liskov Substitution Principle, a derived type should be semantically equivalent to the anticipated behavior of its base type.

To help illustrate, consider the following code:

```javascript
function Vehicle(my) {
    my = my || {};
    my.speed = 0;
    my.running = false;

    this.speed = function() {
        return my.speed;
    };
    this.start = function() {
        my.running = true;
    };
    this.stop = function() {
        my.running = false;
    };
    this.accelerate = function() {
        my.speed++;
    };
    this.decelerate = function() {
        my.speed--;
    };
    this.state = function() {
        if (!my.running) {
            return "parked";
        }
        else if (my.running && my.speed) {
            return "moving";
        }
        else if (my.running) {
            return "idle";
        }
    };
}
```

This listing shows a Vehicle constructor which provides a few basic operations for a vehicle object.&#160; Let’s say this constructor is currently being used in production by several clients.&#160; Now, consider we’re asked to add a new constructor which represents faster moving vehicles.&#160; After thinking about it for a bit, we come up with the following new constructor:

```javascript
function FastVehicle(my) {
    my = my || {};
    
    var that = new Vehicle(my);
    that.accelerate = function() {
        my.speed += 3;
    };
    return that;
}
```

After testing out our new FastVehicle constructor in the browser’s console window, we’re satisfied that everything works as expected.&#160; Objects created with FastVehicle accelerate 3 times faster than before and all the inherited methods function as expected.&#160; Confident that everything works as expected, we decide to deploy the new version of the library.&#160; Upon using the new type, however, we’re informed that objects created with the FastVehicle constructor break existing client code.&#160; Here’s the snippet of code that reveals the problem:

```javascript
var maneuver = function(vehicle) {
    write(vehicle.state());
    vehicle.start();
    write(vehicle.state());
    vehicle.accelerate();
    write(vehicle.state());
    write(vehicle.speed());
    vehicle.decelerate();
    write(vehicle.speed());
    if (vehicle.state() != "idle") {
        throw "The vehicle is still moving!";
    }
    vehicle.stop();
    write(vehicle.state());
};
```

Upon running the code, we see that an exception is thrown: “_The vehicle is still moving!_”.&#160; It appears that the author of this function made the assumption that vehicles always accelerate and decelerate uniformly.&#160; Objects created from our FastVehicle aren’t completely substitutable for objects created from our base Vehicle constructor.&#160; Our FastVehicle violates the Liskov Substitution Principle! 

At this point, you might be thinking: “_But, the client shouldn’t have assumed all vehicles will behave that way._” Irrelevant!&#160; Violations of the Liskov Substitution Principle aren’t based upon whether we think derived objects _should_ be able to make certain modifications in behavior, but whether such modifications _can_ be made in light of existing expectations.

In the case of this example, resolving the incompatibility issue will require a bit of redesign on the part of the vehicle library, the consuming clients, or perhaps both.

## Mitigating LSP Violations

So, how can we avoid Liskov Substitution Principle Violations?&#160; Unfortunately, this isn’t always possible.&#160; To avoid LSP violations altogether, you would be faced with the difficult task of anticipating every single way a library might ever be used.&#160; Add to this the possibility that you might not be the original author of the code you’re being asked to extend and you may not have visibility into all the ways the library is currently being used.&#160; That said, there are a few strategies for heading off violations within your own code.

### Contracts

One strategy for heading off the most egregious violations of the LSP is to use contracts.&#160; Contracts manifest in two main forms: executable specifications and error checking.&#160; With executable specifications, the contract for how a particular library is intended to be used is contained in a suite of automated tests.&#160; The second approach is to include error checking directly in the code itself in the form of preconditions, postconditions and invariant checks.&#160; This technique is known as <a href="http://en.wikipedia.org/wiki/Design_by_contract" target="_blank">Design By Contract</a>, a term coined by Bertrand Meyer, creator of the Eiffel programming language.&#160; The topics of automated testing and Design By Contract are beyond the scope of this series, but I’ll leave you with the following recommendations for when to use each.

  * Always use Test-Driven Development to guide the design of your own code 
  * Optionally use Design By Contract techniques when designing reusable libraries 

For code you’ll be maintaining and consuming yourself, the use of Design By Contract techniques tends to just add a lot of unnecessary noise and overhead to your code.&#160; If you’re in control of the input, a test suite serves as a better control for specifying how a library is intended to be used.&#160; If you’re authoring reusable libraries, Design By Contract techniques serve both to guard against improper usage and as a debugging tool for your users.

### Avoid Inheritance

Another strategy for avoiding LSP violations is to minimize the use of inheritance where possible.&#160; In the book <a href="http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612" target="_blank">Design Patterns &#8211; Elements of Reusable Object-Orineted Software</a> by. Gamma, et. al., we find the following advice:

> Favor object composition over class inheritance

While some of the book’s discussion concerning the advantages of composition over inheritance is only relevant to statically typed, class-based languages (e.g. the inability to change behavior at run time), one of the issues relevant to JavaScript is that of coupling.&#160; When using inheritance, the derived types are coupled with their based types.&#160; This means that changes to the base type may inadvertently affect derived types.&#160; Additionally, composition tends to lead to smaller, more focused objects which are easier to maintain for static and dynamic languages alike.

## It About Behavior, Not Inheritance

Thus far, we’ve discussed the Liskov Substitution Principle within the context of inheritance, which is perfectly appropriate given that JavaScript is an object-oriented language.&#160; However, the essence of the Liskov Substitution Principle isn’t really concerned with inheritance, but behavioral compatibility.&#160; JavaScript is a dynamic language, therefore the contract for an object’s behavior isn’t determined by the type of the object, but by the capabilities expected by the object.&#160; While the Liskov Substitution Principle was originally conceived as a guiding principle for inheritance, it is equally relevant to the design of objects adhering to implicit interfaces.

To illustrate, let’s consider an example taken from the book [Agile Software Development, Principles, Patterns, & Practices](http://www.amazon.com/Software-Development-Principles-Patterns-Practices/dp/0135974445) by Robert C. Martin: the rectangle example.

### The Rectangle Example

Consider that we have an application which uses a rectangle object defined as follows:

```javascript
var rectangle = {
    length: 0,
    width: 0
};
```

Later, it’s determined that the application also needs to work with a square.&#160; Based on the knowledge that a square is a rectangle whose sides are equal in length, we decide to create a square object to use in place of rectangle.&#160; We add length and width properties to match the rectangle object’s definition, but we decide to use property getters and setters so we can keep the length and width values in sync, ensuring it adheres to the definition of a square:

```javascript
var square = {};
(function() {
    var length = 0, width = 0;
    Object.defineProperty(square, "length", {
        get: function() { return length; },
        set: function(value) { length = width = value; }
    });
    Object.defineProperty(square, "width", {
        get: function() { return width; },
        set: function(value) { length = width = value; }
    });
})();
```

Unfortunately, a problem is discovered when the application attempts to use our square in place of rectangle.&#160; It turns out that one of the methods computes the rectangle’s area like so:

```javascript
var g = function(rectangle) {
    rectangle.length = 3;
    rectangle.width = 4;
    write(rectangle.length);
    write(rectangle.width);
    write(rectangle.length * rectangle.width);
};
```

When the method is invoked with square, the product is 16 rather than the expected value of 12.&#160; Our square object violates the Liskov Substitution Principle with respect to the function g.&#160; In this case, the presence of the length and width properties was a hint that our square might not end up being 100% compatible with rectangle, but we won’t always have such obvious hints.&#160; Correcting this situation would likely require a redesign of the shape objects and the consuming application.&#160; A more flexible approach might be to define rectangles and squares in terms of polygons.&#160; Regardless, the important take away from this example is that the Liskov Substitution Principle isn’t just relevant to inheritance, but to any approach where one behavior is being substituted for another.

Next time, we’ll discuss the forth principle in the SOLID acronym: The Interface Segregation Principle.
