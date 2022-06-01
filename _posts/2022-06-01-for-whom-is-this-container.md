---
title: "For Whom is this Container?"
date: 2022-06-01T08:00:00+00:00
author: Derek Greer
layout: post
---

Several of the Messaging platforms in the .Net space have pretty rudimentary APIs (e.g. RabbitMq, Kafka) which require quite a bit of boiler-plate code to be written to get a simple message published and subscribed. You could turn to one of the Conforming Abstraction libraries such as NServiceBus or MassTransit, but perhaps you don’t really want a lowest-common denominator API, you don’t like something about how it creates the messages or topic/queue artifacts, or you simply want a fluent API expressed in terms of the native platform’s nomenclature and behavior. This might lead you down the road of creating your own KafkaBus, SQSBus, RabbitMqBus, etc. that feels like the API you _wish_ the original development team had just provided for you to begin with. Ah, but now you have a dilemma: Frameworks such as this tend to require a number of components you’ll need to compose, many of which you may want to allow users to configure (e.g. serialization needs, consumer class conventions, logging, produce and consume pipelines, etc.). You could write hand-rolled factories, builders, singletons, etc. to facilitate the configuration and building of instances of your components, but you know that using a dependency injection container would make both development and long-term maintenance of your library much easier. But now you have another dilemma: Are you going to tie your project to some open-source container? If so, which one? Should you support a handful of the most popular ones? Should you just rely upon the Service Locator pattern and provide configuration for end users should they want to resolve from their own containers?

This was essentially the dilemma the ASP.Net Core team found themselves in when they set out to develop .Net Core. They had a fairly sizable framework with a lot of moving parts, many of which they wanted to allow the end user to configure. Earlier versions of ASP.Net MVC were built using a Service Locator pattern implementation which facilitated the ability to configure resolving from an open-source container of your choice. This, however, would have no doubt presented various design limitations in addition to the resulting lack of elegance to the resulting codebase, so the team decided to build the new platform from the ground up using dependency injection. They couldn’t, however, feasibly decide to couple their framework to one of the already mature and successful open source DI containers for various reasons. This prompted them to write their own.

One of the keys to understanding the capabilities offered by .Net Core’s container compared to other libraries is recognizing that they built it for their needs, not yours. There is no doubt that there was recognition of the usefulness for some developers to have an out-of-the-box DI container, but they didn’t set out to build a container to compete with the already extremely mature frameworks such as Autofac, StructureMap, or Ninject. For instance, because they weren’t developing user interactive client-facing applications, they didn’t have needs such as convention-based scanning registration, multi-tenancy support, the need for decorators, etc. Their needs pretty much were limited to known types with lifetime scopes of transient, singleton, or scoped per request.

Oddly, there is now a whole new generation of .Net developers which have never used a DI container other than that provided by the Microsoft Extensions suite which are missing out on being exposed to solutions to problems for which containers like Autofac, Lamar, and others facilitate fairly easily, largely I believe because no one has ever really told them: Microsoft didn’t _really_ write that for you.