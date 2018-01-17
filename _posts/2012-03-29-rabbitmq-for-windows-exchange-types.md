---
id: 966
title: 'RabbitMQ for Windows: Exchange Types'
date: 2012-03-29T01:03:24+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=966
permalink: /2012/03/29/rabbitmq-for-windows-exchange-types/
dsq_thread_id:
  - "628116575"
categories:
  - Uncategorized
tags:
  - RabbitMQ
---
This <noindex></noindex> is the fourth installment to the series: RabbitMQ for Windows.&#160; In the [last installment](http://aspiringcraftsman.com/2012/03/18/rabbitmq-for-windows-hello-world-review/), we reviewed our Hello World example and introduced the concept of Exchanges.&#160; In this installment, we’ll discuss the four basic types of RabbitMQ exchanges. 

## Exchange Types

Exchanges control the routing of messages to queues.&#160; Each exchange type defines a specific routing algorithm which the server uses to determine which bound queues a published message should be routed to. 

RabbitMQ provides four types of exchanges: Direct, Fanout, Topic, and Headers. 

### Direct Exchanges 

The Direct exchange type routes messages with a routing key equal to the routing key declared by the binding queue.&#160; The following illustrates how the direct exchange type works: 

&#160; [<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="DirectExchange_thumb37" border="0" alt="DirectExchange_thumb37" src="http://aspiringcraftsman.com/wp-content/uploads/2012/03/DirectExchange_thumb37_thumb.png" width="600" height="219" />](http://aspiringcraftsman.com/wp-content/uploads/2012/03/DirectExchange_thumb37.png) 

&#160; 

The Direct exchange type is useful when you would like to distinguish messages published to the same exchange using a simple string identifier.&#160; This is the type of exchange that was used in our Hello World example.&#160; As discussed in part 3 of our series, every queue is automatically bound to a default exchange using a routing key equal to the queue name.&#160; This default exchange is declared as a Direct exchange.&#160; In our example, the queue named “hello-world-queue” was bound to the default exchange with a routing key of “hello-world-queue”, so publishing a message to the default exchange (identified with an empty string) routed the message to the queue named “hello-world-queue”. 

### Fanout Exchanges

The Fanout exchange type routes messages to all bound queues indiscriminately.&#160; If a routing key is provided, it will simply be ignored.&#160; The following illustrates how the fanout exchange type works: 

&#160; 

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="FanoutExchange_thumb[2]" border="0" alt="FanoutExchange_thumb[2]" src="http://aspiringcraftsman.com/wp-content/uploads/2012/03/FanoutExchange_thumb2_thumb.png" width="600" height="220" />](http://aspiringcraftsman.com/wp-content/uploads/2012/03/FanoutExchange_thumb2.png) 

&#160; 

The Fanout exchange type is useful for facilitating the [publish-subscribe pattern](http://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern).&#160; When using the fanout exchange type, different queues can be declared to handle messages in different ways.&#160; For instance, a message indicating a customer order has been placed might be received by one queue whose consumers fulfill the order, another whose consumers update a read-only history of orders, and yet another whose consumers record the order for reporting purposes. 

### Topic Exchanges

The Topic exchange type routes messages to queues whose routing key matches all, or a portion of a routing key.&#160; With topic exchanges, messages are published with routing keys containing a series of words separated by a dot (e.g. “word1.word2.word3”).&#160; Queues binding to a topic exchange supply a matching pattern for the server to use when routing the message.&#160; Patterns may contain an asterisk (“\*”) to match a word in a specific position of the routing key, or a hash (“#”) to match zero or more words.&#160; For example, a message published with a routing key of “honda.civic.navy” would match queues bound with “honda.civic.navy”, “\*.civic.\*”, “honda.#”, or “#”, but would not match “honda.accord.navy”, “honda.accord.silver”, “\*.accord.*”, or “ford.#”.&#160; The following illustrates how the fanout exchange type works: 

&#160; 

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="TopicExchange_thumb[2]" border="0" alt="TopicExchange_thumb[2]" src="http://aspiringcraftsman.com/wp-content/uploads/2012/03/TopicExchange_thumb2_thumb.png" width="600" height="220" />](http://aspiringcraftsman.com/wp-content/uploads/2012/03/TopicExchange_thumb2.png) 

&#160; 

The Topic exchange type is useful for directing messages based on multiple categories (e.g. product type and shipping preference ), or for routing messages originating from multiple sources (e.g. logs containing an application name and severity level). 

### Headers Exchanges

The Headers exchange type routes messages based upon a matching of message headers to the expected headers specified by the binding queue.&#160; The headers exchange type is similar to the topic exchange type in that more than one criteria can be specified as a filter, but the headers exchange differs in that its criteria is expressed in the message headers as opposed to the routing key, may occur in any order, and may be specified as matching any or all of the specified headers.&#160; The following illustrates how the headers exchange type works: 

&#160; 

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="HeadersExchange_thumb[2]" border="0" alt="HeadersExchange_thumb[2]" src="http://aspiringcraftsman.com/wp-content/uploads/2012/03/HeadersExchange_thumb2_thumb.png" width="600" height="220" />](http://aspiringcraftsman.com/wp-content/uploads/2012/03/HeadersExchange_thumb2.png) 

&#160; 

The Headers exchange type is useful for directing messages which may contain a subset of known criteria where the order is not established and provides a more convenient way of matching based upon the use of complex types as the matching criteria (i.e. a serialized object). 

## Conclusion

That wraps up our introduction to each of the exchange types.&#160; Next time, we’ll walk through an example which demonstrates declaring a direct exchange explicitly and take a look at the the push API. 

</b>