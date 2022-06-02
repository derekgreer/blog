---
id: 821
title: 'SOLID JavaScript: The Single Responsibility Principle'
date: 2011-12-08T17:54:00+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=821
permalink: /2011/12/08/solid-javascript-single-responsibility-principle/
dsq_thread_id:
  - "632864600"
categories:
  - Uncategorized
tags:
  - Design Principles
  - JavaScript
  - SOLID
---
<div style="border-bottom: black 1px solid; border-left: black 1px solid; float: right; border-top: black 1px solid; border-right: black 1px solid" class="toc">
  <u>Series <noindex></noindex> Table of Contents</u> <br />The Single Responsibility Principle <br /><a href="/2011/12/19/solid-javascript-the-openclosed-principle/">The Open/Closed Principle</a> <br /><a href="/2011/12/31/solid-javascript-the-liskov-substitution-principle/">The Liskov Substitution Principle</a> <br /><a href="/2012/01/08/solid-javascript-the-interface-segregation-principle/">The Interface Segregation Principle</a> <br /><a href="/2012/01/22/solid-javascript-the-dependency-inversion-principle/">The Dependency Inversion Principle</a>
</div>

This is the first installment in the SOLID JavaScript series which will explore the SOLID design principles within the context of the JavaScript language.&#160; In this first installment, we’ll take a look at what principles comprise the SOLID design principles and discuss the first of these principles: The Single Responsibility Principle.

<div style="clear: both">
  &#160;
</div>

## The SOLID Design Principles and JavaScript

SOLID is a mnemonic acronym referring to a collection of design principles which evolved out of the objected-oriented community and were popularized by the writings of Robert C. Martin.&#160;&#160;&#160; These principles are as follows:

  * The Single Responsibility Principle 
  * The Open/Closed Principle 
  * The Liskov Substitution Principle 
  * The Interface Segregation Principle 
  * The Dependency Inversion Principle 

These principles are often discussed within the context of classical, statically typed, objected-oriented languages, and while JavaScript is a prototype-based, dynamically typed language blending concepts from both object-oriented and functional paradigms, programmers can still reap the benefits from the application of these principles to JavaScript.&#160; This article will cover the first of these principles: [The Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle).

## The Single Responsibility Principle

The Single Responsibility Principle relates to the functional relatedness of the elements of a module.&#160; The principle states:

> A class should have only one reason to change

This description can be a little misleading as it would seem to suggest that an object should only do one thing. What is meant by this assertion, however, is that an object should have a cohesive set of behaviors, together comprising a single responsibility that, if changed, would require the modification of the object’s definition.&#160; More simply, an object’s definition should only have to be modified due to changes to a single responsibility within the system.

Adherence to the Single Responsibility Principle helps to improve maintainability by limiting the responsibilities of an object to those which change for related reasons.&#160; When an object encapsulates multiple responsibilities, changes to the object for one of the responsibilities can negatively impact the others.&#160; By decoupling such responsibilities, we can create code that is more resilient to change.

But how do we identify whether a given set of behaviors constitutes a single responsibility?&#160; Would grouping all string manipulation into a single object be a single responsibility?&#160; How about grouping together all the service calls made within an application?&#160; Without an established approach to making these determinations, adhering to the Single Responsibility can be a bit perplexing.

## Object Role Stereotypes

One approach which can aid in the organization of behavior within a system is the use of _Object Role Stereotypes_.&#160; Object Role Stereotypes are a set of general, pre-established roles which commonly occur across object-oriented architectures.&#160; By establishing a set of role stereotypes, developers can provide themselves with a set of templates which they can use as they go through the mental exercise of decomposing behavior into cohesive components.

The concept of Object Role Stereotypes is discussed in the book [Object Design: Roles, Responsibilies, and Collaborations](http://www.amazon.com/Object-Design-Roles-Responsibilities-Collaborations/dp/0201379430) by Rebecca Wirfs-Brock and Alan McKean.&#160; The book presents the following stereotypes::

  * **Information holder** – an object designed to know certain information and provide that information to other objects. 
  * **Structurer** – an object that maintains relationships between objects and information about those relationships. 
  * **Service provider** – an object that performs specific work and offers services to others on demand. 
  * **Controller** – an object designed to make decisions and control a complex task. 
  * **Coordinator** – an object that doesn’t make many decisions but, in a rote or mechanical way, delegates work to other objects. 
  * **Interfacer** – an object that transforms information or requests between distinct parts of a system. 

While not prescriptive, this set of role stereotypes provides an excellent mental framework for aiding in the software design process.&#160; Once you have an established&#160; set of role stereotypes to work within, you’ll find it easier to group behaviors into cohesive groups of responsibilities related to the object’s intended role.

## Single Responsibility Principle Example

To illustrate the application of the Single Responsibility Principle, let’s consider the following example which facilitates the movement of product items into a shopping cart:

```#javascript
  function Product(id, description) {
    this.getId = function() {
        return id;
    };
    this.getDescription = function() {
        return description;
    };
}

function Cart(eventAggregator) {
    var items = [];

    this.addItem = function(item) {
        items.push(item);
    };
}

var products = [
    new Product(1, "Star Wars Lego Ship"),
    new Product(2, "Barbie Doll"),
    new Product(3, "Remote Control Airplane")],
    cart = new Cart();

(function() {
    function addToCart() {
        var productId = $(this).attr('id');
        var product = $.grep(products, function(x) {
            return x.getId() == productId;
        })[0];
        cart.addItem(product);

        var newItem = $('&lt;li&gt;&lt;/li&gt;')
            .html(product.getDescription())
            .attr('id-cart', product.getId())
            .appendTo("#cart");
    }

    products.forEach(function(product) {
        var newItem = $('&lt;li&gt;&lt;/li&gt;')
            .html(product.getDescription())
            .attr('id', product.getId())
            .dblclick(addToCart)
            .appendTo("#products");
    });
})();
```

While not overly complex, this example illustrates a number of unrelated responsibilities which are grouped together within a single anonymous function. Let’s consider each responsibility:

First, we have behavior defined to handle populating the Cart model when an item is double-clicked.

Second, we have behavior defined to add items to the cart view when an item is double-clicked.

Third, we have behavior defined to populate the products view with the initial set of products.

Let’s break these three responsibilities out into their own objects:

&#160;



```javascript
function Event(name) {
    this._handlers = [];
    this.name = name;
}
Event.prototype.addHandler = function(handler) {
    this._handlers.push(handler);
};
Event.prototype.removeHandler = function(handler) {
    for (var i = 0; i &lt; handlers.length; i++) {
        if (this._handlers[i] == handler) {
            this._handlers.splice(i, 1);
            break;
        }
    }
};
Event.prototype.fire = function(eventArgs) {
    this._handlers.forEach(function(h) {
        h(eventArgs);
    });
};

var eventAggregator = (function() {
    var events = [];

    function getEvent(eventName) {
        return $.grep(events, function(event) {
            return event.name === eventName;
        })[0];
    }

    return {
        publish: function(eventName, eventArgs) {
            var event = getEvent(eventName);

            if (!event) {
                event = new Event(eventName);
                events.push(event);
            }
            event.fire(eventArgs);
        },

        subscribe: function(eventName, handler) {
            var event = getEvent(eventName);

            if (!event) {
                event = new Event(eventName);
                events.push(event);
            }

            event.addHandler(handler);
        }
    };
})();

function Cart() {
    var items = [];

    this.addItem = function(item) {
        items.push(item);
        eventAggregator.publish("itemAdded", item);
    };
}

var cartView = (function() {
    eventAggregator.subscribe("itemAdded", function(eventArgs) {
        var newItem = $('&lt;li&gt;&lt;/li&gt;')
            .html(eventArgs.getDescription())
            .attr('id-cart', eventArgs.getId())
            .appendTo("#cart");
    });
})();

var cartController = (function(cart) {
    eventAggregator.subscribe("productSelected", function(eventArgs) {
        cart.addItem(eventArgs.product);
    });
})(new Cart());

function Product(id, description) {
    this.getId = function() {
        return id;
    };
    this.getDescription = function() {
        return description;
    };
}

var products = [
    new Product(1, "Star Wars Lego Ship"),
    new Product(2, "Barbie Doll"),
    new Product(3, "Remote Control Airplane")];

var productView = (function() {
    function onProductSelected() {
        var productId = $(this).attr('id');
        var product = $.grep(products, function(x) {
            return x.getId() == productId;
        })[0];
        eventAggregator.publish("productSelected", {
            product: product
        });
    }

    products.forEach(function(product) {
        var newItem = $('&lt;li&gt;&lt;/li&gt;')
            .html(product.getDescription())
            .attr('id', product.getId())
            .dblclick(onProductSelected)
            .appendTo("#products");
    });
})();
```

In our revised design, we’ve removed our anonymous function and replace it with objects to coordinate each of the separate set of responsibilities.&#160; A cartView was introduced to coordinate the population of the cart display, a cartController was introduced to coordinate the population of the cart model, and a productView was introduced to coordinate the population of the products display.&#160; We also introduced an [Event Aggregator](http://martinfowler.com/eaaDev/EventAggregator.html) to facilitate communication between the objects in a loosely-coupled way.&#160; While this design results in a larger number of objects, each object now focuses on fulfilling a specific role within the overall orchestration with minimal coupling between the objects.

Next time, we’ll discuss the next principle in the SOLID acronym: The Open/Closed Principle.
