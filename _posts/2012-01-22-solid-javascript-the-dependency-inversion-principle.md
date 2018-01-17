---
id: 854
title: 'SOLID JavaScript: The Dependency Inversion Principle'
date: 2012-01-22T01:13:21+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=854
permalink: /2012/01/22/solid-javascript-the-dependency-inversion-principle/
dsq_thread_id:
  - "632869920"
categories:
  - Uncategorized
tags:
  - Design Principles
  - JavaScript
  - SOLID
---
<div style="border-bottom: black 1px solid; border-left: black 1px solid; float: right; margin-left: 4px; border-top: black 1px solid; border-right: black 1px solid" class="toc">
  <u>Series <noindex></noindex> Table of Contents</u> <br /><a href="/2011/12/08/solid-javascript-single-responsibility-principle/">The Single Responsibility Principle</a> <br /><a href="/2011/12/19/solid-javascript-the-openclosed-principle/">The Open/Closed Principle</a> <br /><a href="/2011/12/31/solid-javascript-the-liskov-substitution-principle/">The Liskov Substitution Principle</a> <br /><a href="/2012/01/08/solid-javascript-the-interface-segregation-principle/">The Interface Segregation Principle</a> <br />The Dependency Inversion Principle
</div>

This is the fifth and final installment in the SOLID JavaScript series which explores the SOLID design principles within the context of the JavaScript language.&#160; In this final installment, we’ll examine the Dependency Inversion Principle.

<div style="clear: both">
  &#160;
</div>

## The Dependency Inversion Principle

The Dependency Inversion Principle relates to the stability and reusability of higher-level components within an application.&#160; The principle states:

> A. High-level modules should not depend on low-level modules.&#160; Both should depend on abstractions.
> 
> B. Abstractions should not depend upon details.&#160; Details should depend upon abstractions.

The primary concern of the Dependency Inversion Principle is to ensure that the main components of an application or framework remain decoupled from the ancillary components providing low-level implementation details.&#160; This ensures that the important parts of an application or framework aren’t affected when the low level components need to change.

The first part of the principle concerns the method of coupling between high level modules and low level modules.&#160; With traditional layered architecture, high level modules (components encapsulating the core business logic of the application) take dependencies upon low level modules (components providing infrastructure concerns).&#160; When adhering to the Dependency Inversion Principle, this relationship is reversed (i.e. _inverted_).&#160; Rather than high level modules being coupled to low level modules, low level modules are coupled to interfaces declared by the high level modules.&#160; For example, given an application with persistence concerns, a traditional design might contain a core module which relies upon the API defined by a persistence module.&#160; Refactoring toward the Dependency Inversion Principle, the persistence module would be modified to conform to an interface defined by the core module.

The second part of the Dependency Inversion Principle concerns the proper relationship between abstractions and details.&#160; To understand this portion of the principle, it helps to consider its applicability to the language from whence the principle was conceived: the C++ language. 

Unlike some statically typed languages, C++ doesn’t provide a language level construct for defining interfaces.&#160; What it does provide is the separation of class definition from class implementation.&#160; In C++, classes are defined using a header file which lists the member methods and variables of a class along with a source file which contains the implementation of any member methods.&#160; Since any member variables and private methods are declared within the header file, it’s possible for classes intended to be used as abstractions to become dependent upon the implementation details of the class.&#160; This is overcome by defining classes which only contain abstract methods (known in C++ as pure abstract base classes) to serve as interfaces for implementing classes.

## DIP and JavaScript

As a dynamic language, JavaScript doesn’t require the use of abstractions to facilitate decoupling.&#160; Therefore, the stipulation that _abstractions shouldn&#8217;t depend upon details_ isn’t particularly relevant to JavaScript applications.&#160; The stipulation that _high level modules shouldn’t depend upon low level modules_ is, however, relevant.

When discussing the Dependency Inversion Principle in the context of statically-typed languages, the concern of coupling is both **semantic** and **physical**.&#160; That is to say, if a high level module is coupled to a low level module, it is coupled both to the semantic interface as well as the physical definition of the interface defined within the low level module.&#160; The implication is that high level module dependencies should be inverted both for dependencies upon 3rd party libraries as well as native low level modules. 

To explain, consider a .Net application which might encapsulate useful high level modules which have a dependency upon a low level module providing persistence concerns. While the author is likely to have expressed a similar API for the persistence interface whether the DIP was adhered to or not, the high level module would not be capable of being reused in another application without bringing along the dependency upon the low level module where the persistence interface is defined. 

In JavaScript, the applicability of the Dependency Inversion Principle is relevant only to the semantic coupling of high level modules to low level modules.&#160; As such, adherence to the DIP can be achieved simply by expressing the semantic interface in terms of the application’s needs as opposed to coupling to the implicit interface defined by whatever implementation is chosen for a low level module.

To illustrate, consider the following example:

<pre class="prettyprint">$.fn.trackMap = function(options) {
    var defaults = { 
        /* defaults */
    };
    options = $.extend({}, defaults, options);

    var mapOptions = {
        center: new google.maps.LatLng(options.latitude,options.longitude),
        zoom: 12,
        mapTypeId: google.maps.MapTypeId.ROADMAP
    },
        map = new google.maps.Map(this[0], mapOptions),
        pos = new google.maps.LatLng(options.latitude,options.longitude);

    var marker = new google.maps.Marker({
        position: pos,
        title: options.title,
        icon: options.icon
    });

    marker.setMap(map);

    options.feed.update(function(latitude, longitude) {
        marker.setMap(null);
        var newLatLng = new google.maps.LatLng(latitude, longitude);
        marker.position = newLatLng;
        marker.setMap(map);
        map.setCenter(newLatLng);
    });

    return this;
};

var updater = (function() {    
    // private properties

    return {
        update: function(callback) {
            updateMap = callback;
        }
    };
})();

$("#map_canvas").trackMap({
    latitude: 35.044640193770725,
    longitude: -89.98193264007568,
    icon: 'http://bit.ly/zjnGDe',
    title: 'Tracking Number: 12345',
    feed: updater
});</pre>

In this listing, we have a small library which converts a div target into an map used to show the current location of a item being tracked.&#160; The trackMap function has two dependencies: the 3rd party Google Maps API and a location feed.&#160; The responsibility of the feed object is to simply invoke a callback (supplied during the initialization process) with a new latitude and longitude position when the icon location should be updated.&#160; The Google Maps API is used to do the actual rending of the map to the screen. 

While the interface of the feed object may or may not have been designed in terms of the trackMap function, the fact that its role is simple and focused makes it easy to substitute different implementations.&#160; Not so with the Google Maps dependency.&#160; Since the trackMap function is semantically coupled to the Google Maps API, switching to a different mapping provider would require the trackMap function to be rewritten or an adapter to be written to adapt another mapping provider to Google’s specific interface.

To invert the semantic coupling to the Google Maps library, we need to redesign the trackMap function to have a semantic coupling to an implicit interface which abstractly represents the functionality needed by a mapping provider.&#160; We would then need to implement an object which adapts this interface to the Google Maps API.&#160; The following shows this alternate version of the trackMap function:

<pre class="prettyprint">$.fn.trackMap = function(options) {
    var defaults = { 
        /* defaults */
    };

    options = $.extend({}, defaults, options);

    options.provider.showMap(
        this[0],
        options.latitude,
        options.longitude,
        options.icon,
        options.title);

    options.feed.update(function(latitude, longitude) {
        options.provider.updateMap(latitude, longitude);
    });

    return this;
};


$("#map_canvas").trackMap({
    latitude: 35.044640193770725,
    longitude: -89.98193264007568,
    icon: 'http://bit.ly/zjnGDe',
    title: 'Tracking Number: 12345',
    feed: updater,
    provider: trackMap.googleMapsProvider
});</pre>

In this version, we’ve redesigned the trackMap function to express its needs in terms of a generic mapping provider interface and have moved the implementation details out into a separate googleMapsProvider component which can be bundled as a separate JavaScript module.&#160; Here’s our googleMapsProvider implementation:

<pre class="prettyprint">trackMap.googleMapsProvider = (function() {
    var marker, map;

    return {
        showMap: function(element, latitude, longitude, icon, title) {
            var mapOptions = {
                center: new google.maps.LatLng(latitude, longitude),
                zoom: 12,
                mapTypeId: google.maps.MapTypeId.ROADMAP
            },
                pos = new google.maps.LatLng(latitude, longitude);

            map = new google.maps.Map(element, mapOptions);

            marker = new google.maps.Marker({
                position: pos,
                title: title,
                icon: icon
            });

            marker.setMap(map);
        },
        updateMap: function(latitude, longitude) {
            marker.setMap(null);
            var newLatLng = new google.maps.LatLng(latitude,longitude);
            marker.position = newLatLng;
            marker.setMap(map);
            map.setCenter(newLatLng);
        }
    };
})();</pre>

With these changes, our trackMap function is now more resilient to the changes that might occur to the Google Maps API and is capable of being reused with another mapping provider.&#160; That is, as long as it’s API can be adapted to the needs of our application.

## Whither Dependency Injection?

While not particularly related, the concept of Dependency Injection is often confused with the Dependency Inversion Principle due to a similarity in terminology.&#160; For this reason, a discussion of the differences between the two concepts may prove helpful for some.

Dependency Injection is a specific form of <a href="http://en.wikipedia.org/wiki/Inversion_of_control" target="_blank">Inversion of Control</a> in which the concern being inverted is how a component obtains its dependencies.&#160; When using Dependency Injection, dependencies are supplied to a component rather than the component obtaining the dependency by means of creating an instance of the dependency, requesting the dependency through a factory, requesting the dependency from a Service Locator, or any other means of initiation by the component itself.&#160; Both the Dependency Inversion Principle and Dependency Injection are concerned with dependencies and both use the notion of inversion to contrast an alternate approach to a presumed standard approach.&#160; However, the Dependency Inversion Principle isn’t concerned with how components obtains their dependencies, but with the decoupling of high level components from low level components.&#160; In a sense, the Dependency Inversion Principle might be said to be another form of Inversion of Control where the concern being inverted is which module defines the interface.

## Conclusion

This brings us to the end of our series.&#160; While in the course of our examination we saw variations in how the SOLID design principles apply to JavaScript over other languages, each of the principles were shown to have some degree of applicability within JavaScript development.