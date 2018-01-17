---
id: 928
title: 'RabbitMQ for Windows: Introduction'
date: 2012-03-05T13:00:00+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=928
permalink: /2012/03/05/rabbitmq-for-windows-introduction/
dsq_thread_id:
  - "627766022"
categories:
  - Uncategorized
tags:
  - RabbitMQ
---
If <noindex></noindex> you’re interested in getting started with distributed programming and you develop on the Microsoft Windows platform, <a href="http://www.rabbitmq.com" target="_blank">RabbitMQ</a> may be what you’re looking for.&#160; RabbitMQ is an open source, standards-based, multi-platform message broker with client libraries available for a host of development platforms including .Net.&#160;&#160; 

This series will provide a gentle introduction to getting started with RabbitMQ for .Net developers, including a Windows environment installation guide along with an introduction to basic concepts and features through the use of examples in C#.&#160; In this first installment, we’ll cover installation and basic configuration. 

## Installation 

The first thing to know about RabbitMQ installation is that RabbitMQ runs on the [Erlang](http://en.wikipedia.org/wiki/Erlang_(programming_language)) virtual runtime.&#160; “_What is Erlang_”, you ask, and “_Why should I ask our admins to install yet another runtime engine on our servers_”?&#160; Erlang is a functional language which places a large emphasis on concurrency and high reliability.&#160; Developed by&#160; Joe Armstrong, Mike Williams, and Robert Virding to support telephony applications at Ericsson, Erlang’s flagship product, the Ericsson AXD301 switch, is purported to have achieved a reliability of nine ["9"s](http://en.wikipedia.org/wiki/Nines_(engineering)). 

A popular quote among Erlang adherents is Verding’s “First Rule of Programming”: 

> “Any sufficiently complicated concurrent program in another language contains an ad hoc informally-specified bug-ridden slow implementation of half of Erlang.” &#8211; Robert Verding 

Sound like the perfect platform to write a message broker in?&#160; The authors of RabbitMQ thought so too.

With that, let’s get started with the installation. 

### &#160;

### Step 1: Install Erlang

The first step will be to download and install Erlang for Windows.&#160; You can obtain the latest installer from [here](http://www.erlang.org/download.html) (version R15B at the time of this writing) . 

After downloading and completing the Erlang installation wizard, you should have a new ERLANG_HOME environment variable set.&#160; If not, you’ll want to set this now so RabbitMQ will be able to locate your installation of Erlang. 

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="ErlangEnv" border="0" alt="ErlangEnv" src="http://aspiringcraftsman.com/wp-content/uploads/2012/03/ErlangEnv_thumb.png" width="395" height="438" />](http://aspiringcraftsman.com/wp-content/uploads/2012/03/ErlangEnv.png)

&#160;

### &#160;

### Step 2: Install RabbitMQ

Next, download and install the latest version of RabbitMQ for Windows from [here](http://www.rabbitmq.com/download.html) (version 2.7.1 at the time of this writing). 

### &#160;

### Step 3: Install the RabbitMQ Management Plugin 

By default, the RabbitMQ Windows installer registers RabbitMQ as a Windows service, so technically we’re all ready to go.&#160; In addition to the command line utilities provided for managing and monitoring our RabbitMQ instance, a Web-based management plugin is also provided with the standard Windows distribution.&#160; The following steps detail how to get the management plugin up and going. 

First, from an elevated command prompt, change directory to the sbin folder within the RabbitMQ Server installation directory (e.g. %PROGRAMFILES%\RabbitMQ Server\rabbitmq\_server\_2.7.1\sbin\). 

Next, run the following command to enable the rabbitmq management plugin: 

<pre class="prettyprint">rabbitmq-plugins.bat enable rabbitmq_management </pre>

&#160;

Lastly, to enable the management plugin we need to reinstall the RabbitMQ service.&#160; Execute the following sequence of commands to reinstall the service: 

<pre class="prettyprint">rabbitmq-service.bat stop 
rabbitmq-service.bat install 
rabbitmq-service.bat start </pre>

&#160; 

To verify management plugin is up and running, start your favorite browser and navigate to <http://localhost:55672/mgmt/>.&#160; If everything went ok, you should see a screen similar to the following: 

&#160; 

&#160;[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="RabbitManagement" border="0" alt="RabbitManagement" src="http://aspiringcraftsman.com/wp-content/uploads/2012/03/RabbitManagement_thumb.png" width="640" height="302" />](http://aspiringcraftsman.com/wp-content/uploads/2012/03/RabbitManagement.png)

From here, you’ll be able to configure and monitor your RabbitMQ instance. 

That concludes our installation guide.&#160; Next time, we’ll walk through writing our first RabbitMQ C# application. 

</b>