---
id: 1031
title: Introducing RabbitBus
date: 2012-06-01T11:29:21+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=1031
permalink: /2012/06/01/introducing-rabbitbus/
dsq_thread_id:
  - "710885539"
categories:
  - Uncategorized
tags:
  - RabbitBus
  - RabbitMQ
---
## What <noindex></noindex> Is It?

RabbitBus is a .Net client API for use with RabbitMQ.&#160; RabbitBus was designed to make working with RabbitMQ easy by providing a fluent-interface which places a focus on discoverability and by providing commonly needed constructs not provided through the official RabbitMQ .Net client API. 

&#160;

## How Do I Use It?

The RabbitBus library was designed to allow for the centralization of all RabbitMQ configuration at application startup, separating the concerns of routing, serialization, and error handling from the central concerns of publishing and consuming messages.

RabbitBus works with object-based messages.&#160; For example, if you have an application from which you would like to publish status update messages, you might model your message using the following class:

<pre class="prettyprint">[Serializable]
public class StatusUpdate
{
  public StatusUpdate(string status)
  {
    Status = status;
  }

  public string Status { get; set; }
}</pre>

After configuring how messages are to be handled, you’ll then use an instance of a _Bus_ type to publish or subscribe to each message.

Configuration of the Bus is handled through a _BusBuilder._&#160; The BusBuilder type provides an API for specifying how serialization, publication, consumption, and other concerns will be handled by the Bus.

If you’re already familiar with RabbitMQ concepts then you should find working with RabbitBus to be fairly easy.&#160; The following demonstrates some of the basic usage scenarios:

#### Message Publication

To configure a producer application to publish messages of type _StatusUpdate_ to a direct exchange named “_status-update-exchange_” on localhost, you would then use the following configuration:

<pre class="prettyprint">Bus bus = new BusBuilder()
  .Configure(ctx =&gt; ctx.Publish&lt;StatusUpdate&gt;()
                         .WithExchange("status-update-exchange"))
  .Build();
bus.Connect();</pre>

To publish a _StatusUpdate_ message, you would then make the following invocation:

<pre class="prettyprint">bus.Publish(new StatusUpdate("OK"));</pre>

#### Message Subscription

To configure a consumer application to subscribe to _StatusUpdate_ messages on localhost, you would use the following configuration:

<pre class="prettyprint">Bus bus = new BusBuilder()
  .Configure(ctx =&gt; ctx.Consume&lt;StatusUpdate&gt;()
                         .WithExchange("status-update-exchange")
                         .WithQueue("status-update-queue"))
  .Build();</pre>

To subscribe to _StatusUpdate_ messages, you would then make the following invocation:

<pre class="prettyprint">bus.Subscribe&lt;StatusUpdate&gt;(messageContext =&gt; { /* handle message */ });</pre>

&#160;

## What Other Features Are Provided?

RabbitBus provides the following features:

  * support of all AMQP 0.9.1 exchange types (i.e. direct, fanout, topic, and headers) 
  * remote procedure calls (RPC) 
  * deadletter queue support 
  * convention based auto-subscription 
  * RabbitMQ push and pull API support 
  * extensible serialization (Binary serialization by default, Json serialization provided by [RabbitBus.Serialization.Json](https://nuget.org/packages/RabbitBus.Serialization.Json)) 
  * customizable error handling 
  * RabbitMQ server restart recovery 
  * configurable offline queuing support 
  * logging 

&#160;

## Where Can I Learn More?

You can find more information about how to use RabbitBus on the [RabbitBus Wiki](https://github.com/derekgreer/rabbitBus/wiki).&#160; Additionally, RabbitBus was developed using Test-Driven Development and care was taken in the implementation of its [executable specification suite](https://github.com/derekgreer/rabbitBus/tree/master/src/RabbitBus.Specs) to maximize demonstration of the API’s intended use.

&#160;

## Where Do I Get It?

RabbitBus is available as a [NuGet package](https://nuget.org/packages/RabbitBus) and the source is available on [Github](https://github.com/derekgreer/rabbitBus).