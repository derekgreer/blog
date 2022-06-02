---
id: 842
title: 'SOLID JavaScript: The Interface Segregation Principle'
date: 2012-01-08T03:58:35+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=842
permalink: /2012/01/08/solid-javascript-the-interface-segregation-principle/
dsq_thread_id:
  - "638997238"
categories:
  - Uncategorized
tags:
  - Design Principles
  - JavaScript
  - SOLID
---
<div style="border-bottom: black 1px solid; border-left: black 1px solid; float: right; margin-left: 4px; border-top: black 1px solid; border-right: black 1px solid" class="toc">
  <u>Series <noindex></noindex> Table of Contents</u> <br /><a href="/2011/12/08/solid-javascript-single-responsibility-principle/">The Single Responsibility Principle</a> <br /><a href="/2011/12/19/solid-javascript-the-openclosed-principle/">The Open/Closed Principle</a> <br /><a href="/2011/12/31/solid-javascript-the-liskov-substitution-principle/">The Liskov Substitution Principle</a> <br />The Interface Segregation Principle <br /><a href="/2012/01/22/solid-javascript-the-dependency-inversion-principle/">The Dependency Inversion Principle</a>
</div>

This is the forth installment in the SOLID JavaScript series which explores the SOLID design principles within the context of the JavaScript language. In this installment, we’ll be covering the Interface Segregation Principle.

<div style="clear: both">
  &#160;
</div>

## The Interface Segregation Principle 

The Interface Segregation Principle relates to the cohesion of interfaces within a system. The principle states: 

> Clients should not be forced to depend on methods they do not use. 

When clients depend upon objects which contain methods used only by other clients, or are forced to implement unused methods with degenerate functionality (potentially leading to <a href="http://freshbrewedcode.com/derekgreer/2011/12/31/solid-javascript-the-liskov-substitution-principle/" target="_blank">Liskov Substitution Principle</a> violations), this can lead to fragile code. This occurs when an object serves as an implementation for a non-cohesive interface. 

The Interface Segregation Principle is similar to the <a href="http://freshbrewedcode.com/derekgreer/2011/12/08/solid-javascript-single-responsibility-principle/" target="_blank">Single Responsibility Principle</a> in that both deal with the cohesion of responsibilities. In fact, the ISP can be understood as the application of the SRP to an object’s public interface. 

## JavaScript Interfaces

So, what does this principle have to do with JavaScript? After all, JavaScript doesn’t have interfaces, right? If by interfaces we mean some abstract type provided by the language to establish contracts and enable decoupling, then such an assertion would be correct. However, JavaScript does have another type of interface. In the book <a href="http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612" target="_blank">Design Patterns &#8211; Elements of Reusable Object-Oriented Software</a>, Gamma et al., we find the following definition of an interface: 

> Every operation declared by an object specifies the operation’s name, the objects it takes as parameters, and the operation’s return value. This is known as the operation’s **signature**. The set of all signatures defined by an object’s operations is called the **interface** to the object. An object’s interface characterizes the complete set of requests that can be sent to the object. 

Regardless of whether a language provides a separate construct for representing interfaces or not, all objects have an implicit interface comprised of the set of public properties and methods of the object. For example, consider the following library: 

```javascript
var exampleBinder = {};
exampleBinder.modelObserver = (function() {
    /* private variables */
    return {
        observe: function(model) {
            /* code */
            return newModel;
        },
        onChange: function(callback) {
            /* code */
        }
    }
})();

exampleBinder.viewAdaptor = (function() {
    /* private variables */
    return {
        bind: function(model) {
            /* code */
        }
    }
})();

exampleBinder.bind = function(model) {
    /* private variables */
    exampleBinder.modelObserver.onChange(/* callback */);
    var om = exampleBinder.modelObserver.observe(model);
    exampleBinder.viewAdaptor.bind(om);
    return om;
};
```

This listing presents a library named _exampleBinder_ whose purpose is to facilitate two-way data-binding. The public interface of the library is represented by the bind method. The responsibilities of change notification and view interaction have been separated into the objects _modelObserver_ and _viewAdaptor_ respectively to allow for alternate implementations. These objects represent the implementations of the **interfaces** expected by the _bind_ method. Any object adhering to the semantic behavior represented by these interfaces may be substituted for the default implementations. 

Though the JavaScript language may not provide interface types to aid in specifying the contract of an object, the object’s implicit interface still serves as a contract to clients within an application. 

## ISP and JavaScript 

The following sections discuss some of the consequences to Interface Segregation Principle violations in JavaScript. As you’ll see, while the ISP is applicable to the design of JavaScript applications, the language features offer a little more resiliency to non-cohesive interfaces than in statically-typed languages. 

### Degenerate Implementations

In statically-typed languages, one issue that arises from violations of the ISP is the need to create _degenerate implementations_. In languages like Java and C#, all methods declared by an interface must be implemented. For cases where a particular interface is required, but only a subset of the behavior is relevant for a given usage scenario, the unused methods are generally stubbed out with empty implementations or with implementations which simply throw an exception indicating the method isn’t actually implemented. In JavaScript, cases where only a subset of an object’s interface is utilized doesn’t end up posing the same issues since a substituting object need only provide the expected properties to conform to the consumed portion of the object’s interface. Nevertheless, such implementations can still lead to Liskov Substitution Violations. 

For instance, consider the following example: 

```javascript
var rectangle = {
    area: function() { 
        /* code */
    },
    draw: function() { 
        /* code */
    }
};

var geometryApplication = {
    getLargestRectangle: function(rectangles) { 
        /* code */
    }
};

var drawingApplication = {
    drawRectangles: function(rectangles) {
       /* code */
    }
};
```

While a substitute rectangle could be created with only an area() method in order to meet the needs of geometryApplication’s getLargestRectangle method, such an implementation would represent a LSP violation with respect to drawingApplication’s drawRectangles method. 

### Static Coupling

Another issue that arises in statically-typed languages is the issue of static coupling. In statically-typed languages, interfaces play a significant role in the design of loosely-coupled applications. In both static and dynamic languages, there are times when an object may need to collaborate with multiple clients (e.g. in cases where shared state may be required). For statically-typed languages, it’s considered a best practice for clients to interact with objects using <a href="http://martinfowler.com/bliki/RoleInterface.html" target="_blank">Role Interfaces</a>. This allows clients to interact with an object which may require the intersection of multiple roles as an implementation detail without coupling the client to unused behavior. In JavaScript, this isn’t an issue since objects are decoupled by virtue of the language’s dynamic capabilities. 

### Semantic Coupling

One consequence that’s equally relevant between JavaScript and statically-typed languages is the issue of semantic coupling. Semantic coupling is the interdependency that occurs when one portion of an object’s behavior is coupled to another. When an object’s interface specifies non-cohesive responsibilities, changes to adapt to the evolving needs of one client may inadvertently affect another client resulting from changes made to shared state or behavior. This is also one of the potential consequences to violating the Single Responsibly Principle, though this is typically viewed from the perspective of the impact non-cohesive behavior has on collaborating objects. From an ISP perspective, this consequence is extended to extensions via inheritance or object substitution. 

### Extensibility

Another important role interfaces play in JavaScript applications is in the area of extensibility. The most prevalent example of this can be seen in the use of callbacks where a function conforming to the expected interface is passed to a library to be invoked at some point in the future (e.g. upon a successful AJAX request). The Interface Segregation Principle becomes relevant when such interfaces require the implementation of an object with multiple properties and/or methods. When an interface begins to require the implementation of a significant number of methods, this makes working with the consuming library more difficult and is likely an indication that the interfaces represent non-cohesive responsibilities. This is often described as “fat” interfaces. 

In summary, the dynamic capabilities of JavaScript leave us with a few less consequences to the occurrence of non-cohesive interfaces than with statically-typed languages, but the Interface Segregation Principle has its place in the design of JavaScript applications nonetheless. 

Next time, we’ll take a look at the final principle in the SOLID acronym: The Dependency Inversion Principle.
