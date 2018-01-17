---
id: 815
title: Getting Started With RequireJS
date: 2011-11-28T07:00:22+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=815
permalink: /2011/11/28/getting-started-with-requirejs/
dsq_thread_id:
  - "628323340"
categories:
  - Uncategorized
---
Have <noindex></noindex> you ever found yourself struggling to find your bearings in an unorganized and unfamiliar JavaScript code-base? Without a systematic method of organizing your code, Javascript can get complex pretty quickly.

One of the drawbacks to the JavaScript language is its lack of native module support. This omission can be mitigated to some degree through the use of the Module Pattern in conjunction with the separation of modules into separate script files.

The following is a simple example demonstrating how a JavaScript code base can be organized into separate files in conjunction with the Module Pattern.

Consider a project with the following folder structure:

<pre  class="prettyprint">project-directory/
project.html
scripts/
customerModule.js
orderModule.js
projectModule.js
</pre>

In this example, project.html depends upon three modules: projectModule, customerModule, and orderModule. The modules are included using script tags for each file:

<pre class="prettyprint"><code>
&lt;!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
&lt;html>
&lt;head>
    &lt;title>Example&lt;/title>
    &lt;script language="JavaScript1.5" type="text/javascript" src="scripts/customerModuleModule.js">&lt;/script>
    &lt;script language="JavaScript1.5" type="text/javascript" src="scripts/orderModule.js">&lt;/script>
    &lt;script language="JavaScript1.5" type="text/javascript" src="scripts/projectModule.js">v/script>
&lt;/head>
&lt;body>
&lt;!-- body here -->
&lt;/body>
&lt;/html>
</code>
</pre>

The customerModule.js file defines a global customerModule which is assigned to an anonymous object containing a getCustomers function through the use of a self-executing anonymous function:

<pre class="prettyprint">var customerModule = function () {
    return {
        getCustomers: function() {
            // do something to get customers ...
        }
    }
}();
</pre>

The orderModule.js file follows the same pattern to define a global orderModule object:

<pre class="prettyprint">var orderModule = function () {
    return {
        getOrders: function() {
            // do something to get orders ...
        }
    }
}();
</pre>

The projectModule.js file for contains a self-executing anonymous function expression initialized using the the customerModule and orderModule:

<pre class="prettyprint">(function(c, o) {
       
    var customers = c.getCustomers();
    // do something with customers ...
    
    var orders = o.getOrders();
    // do something with orders ...

}(customerModule, orderModule));
</pre>

Organizing Javascript using this approach can significantly improve the maintainability of a code base, but this approach does have a few drawbacks.

First, each of the modules required by the project.html file have to be declared within a script tag. Generally, some of the referenced scripts will be specific to the project while others will be transitive dependencies providing supporting infrastructure needs (i.e. jquery, knockout, underscore, etc.). As a code base evolves over time, it may be difficult to know which of these dependencies are still used. Referencing required dependencies in a file other than the requiring module can also make it more difficult to reuse those modules across projects. 

Second, this approach requires that the script tags be declared in a particular order. In the previous example, the customerModule and orderModule variables have to be defined prior to the definition of the projectModule.

Third, this approach pollutes the global name space by defining the module variables globally (though, this problem can be reduced to some degree by the use of a single application namespace).

While not horrible drawbacks, there is an alternative.

## RequireJS

[RequireJS](http://requirejs.org) is an implementation of the [Asynchronous Module Definition](https://github.com/amdjs/amdjs-api/wiki/AMD) (AMD) specification which defines a mechanism for loading modules asynchronously along with a declaration of dependencies by each module.

Let’s consider our previous example implemented using RequireJS. With this implementation, we’ll replace projectModule.js with a main.js which will serve as our main script entry point. With this change, our project.html file only requires the use of a single script tag to declare the main entry point:

<pre class="prettyprint">&lt;!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
&lt;html>
&lt;head>
    &lt;title>Example Application&lt;/title>
    &lt;script data-main="scripts/main" src="scripts/require.js">&lt;/script>
&lt;/head>
&lt;body>
&lt;h1>Example Application&lt;/h1>
&lt;/body>
&lt;/html>
</pre>

Note the data-main attribute which is used by require.js as an indicator of the initial script to load.

The main.js script contains the following:

<pre class="prettyprint">require(["customerModule", "orderModule"], function(c, o) {
    
    var customers = c.getCustomers();
    // do something with customers ...

    var orders = o.getOrders();
    // do something with orders ...    
});
</pre>

In main.js, the require method is used to first define the module dependencies as an array and a callback function once the modules have been loaded.

The customerModule.js file is modified to the following:

<pre class="prettyprint">define(function() {
    return {
         getCustomers: function() {
            // do something to get customers ...
        }
    }
});
</pre>

Similarly, the orderModule.js file is modified to the following:

<pre class="prettyprint">define(function() {
    return {
         getOrders: function() {
            // do something to get orders ...
        }
    }
});
</pre>

Rather than defining objects on the global namespace, the define function is used to define the customer and order modules.

Pretty simple, eh? In addition to these rather simple use cases, RequireJS also supports more advance scenarios such as adaptation to [CommonJS](http://www.commonjs.org/) modules (the module specification used by node.js), dealing with circular dependencies, support for internationalization, and more. RequireJS also provides an optimizer which, among other things, combines and minifies your scripts for optimal performance.

For a more in-depth look at the topic of writing modular JavaScript, I suggest checking out an excellent article entitled [Writing Modular JavaScript With AMD, CommonJS & ES Harmony](http://addyosmani.com/writing-modular-js/).