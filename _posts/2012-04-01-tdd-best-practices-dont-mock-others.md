---
id: 983
title: 'TDD Best Practices: Don&rsquo;t Mock Others'
date: 2012-04-01T07:00:00+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=983
permalink: /2012/04/01/tdd-best-practices-dont-mock-others/
dsq_thread_id:
  - "632332908"
categories:
  - Uncategorized
tags:
  - TDD
---
[Test <noindex></noindex> Doubles](http://lostechies.com/derekgreer/2011/05/15/effective-tests-test-doubles/) play an important role in the practice of Test-Driven Development for establishing a controlled context, facilitating outside-in design, verifying component interaction, and providing overall test stability through isolation.&#160; While isolating components from their dependencies is a Good Thing, some of the advantages of using test doubles and Test-Driven Development itself can be thwarted by substituting the wrong components.

One design principle which is particularly relevant to the practice of using test doubles (mocks, stubs, spies, fakes, etc.) is the [Dependency Inversion Principle](http://en.wikipedia.org/wiki/Dependency_inversion_principle).&#160; Stated simply, the Dependency Inversion Principle pertains to the decoupling of the stuff you most care about (like your domain layer) from the stuff you care least about (like your low-level infrastructure code and third-party libraries).&#160; In relation to the practice of using test doubles, it’s the coupling to other people’s stuff that is most relevant.

When you provide test doubles for types you don’t own, this indicates that you have a Dependency Inversion Principle violation.&#160; If you’re injecting concrete, abstract, or interface types for a third party dependency then that means your stuff can’t be used without their stuff.&#160; While not ideal, you might ignore this design principle due your personal stance of: “They’ll pry framework XYZ from my cold, dead hands”.&#160; Fair enough.&#160; Nevertheless, there are a few other reasons why you might want to consider following this principle.

When using test doubles for third-party libraries, you make assumptions about how the third-party library works.&#160; Perhaps the library you’re using has no bugs and you thoroughly understand all of its behavior, so you know that the behavior you are substituting behaves exactly like the real library will work in production once you put it all together.&#160; If you aren’t confident of this, however, you might find that your design doesn’t interact with the third-party component in quite the way you imagined.&#160; Let’s say it does though.&#160; What about the next version?&#160; When we design our systems using test doubles for third-party libraries, we create a false sense of security around the soundness and stability of our design.

Another problem caused by the use of test doubles for third-party components is the limitation it places on emergent design.&#160; Coupling to third-party components will often lead to making design concessions to accommodate your dependencies rather than allowing your own design to emerge through the feedback received through the TDD process.

If we shouldn’t couple our design to third-party libraries (thereby removing the need to supply test doubles for third-party libraries), how then do we make use of such libraries and ensure our code works correctly?&#160; The answer is to write component/unit level tests which guide the design of your own code which expresses its needs through its own interfaces (whose behavior is defined through the use of test doubles), and write integration tests which validate the behavior of adaptors which implement your design’s interfaces using the desired third-party components.

For example, you might design your components to rely upon an ILogger, IMapper, or IBus using test doubles to define their expected behavior and have integration tests which validate the implementation of these interfaces with log4net, AutoMapper, or ØMQ respectively.

In summary, don’t use test doubles for types you don’t own.&#160; Instead, let any dependencies you take on be an outgrowth of your emergent design and provide integration tests to validate the expected behavior of any third-party libraries you use to facilitate the required behavior.