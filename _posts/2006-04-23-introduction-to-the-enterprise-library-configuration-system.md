---
id: 25
title: Introduction to the Enterprise Library Configuration System
date: 2006-04-23T05:46:00+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/2006/04/introduction-to-the-enterprise-library-configuration-system/
permalink: /2006/04/23/introduction-to-the-enterprise-library-configuration-system/
dsq_thread_id:
  - "630154127"
categories:
  - Uncategorized
---
The goal of this article is to provide a high level introduction to the Enterprise Library configuration system. A thorough discussion of this topic and the underlying dependency injection subsystem is deserving of a series of detailed articles, but hopefully this article will provide a base foundation for understanding Enterprise Library’s new approach to configuration.

## Overview

The <noindex></noindex> Enterprise Library configuration system is built upon ObjectBuilder, a dependency injection/Inversion of Control (IoC) framework, and provides a standard approach for the creation of the main components within the Enterprise Library Application Blocks. While the Enterprise Library strategy for object creation relies upon the ObjectBuilder architecture developed within the Composite Application Block (CAB), Enterprise Library introduces its own set of Strategies, Policies, and helper components for working with ObjectBuilder which merits discussion apart from the ObjectBuilder system proper.

## Main Components

Enterprise Library uses a consistent approach internally for instantiating all of its main application block components which is necessary to understand if attempting to develop application blocks consistent with the Enterprise Library architecture.

The main classes used in the object creation lifecycle are as follows:

  * Static Factory Classes
  * Instance Factory Classes
  * The EnterpriseLibraryFactory Class
  * The ObjectBuilder
  * Custom Factory Classes
  * Assemblers

While these are certainly not all the components used, these represent the main classes within Enterprise Library’s object building strategy.

The basic call chain is as follows:

  1. A method within a static factory class is called to create one of the desired components within Enterprise Library (e.g. DatabaseFactory).
  2. The static factory class calls an instance based factory class (e.g. DatabaseProviderFactory) which has been statically created with the default configuration source location.
  3. The instance factory calls an Enterprise Library helper factory named EnterpriseLibraryFactory which interfaces with the ObjectBuilder dependency injection subsystem.
  4. ObjectBuilder in turn calls a custom factory which is created as part of the ConfiguredObjectStrategy.
  5. The custom factory then creates a class known as an Assembler which performs the logic necessary to construct the desired object.

The following is a brief discussion of each of the main elements within the configuration system:

## Static Factories

Static factories within Enterprise Library provide static default and named instance factory methods for creating the desired objects from the default configuration source location. These factories are intended for the most commonly used scenarios where configuration information is obtained from the default location.

Examples of these factories within Enterprise Library include the CacheFactory, DatabaseFactory, LogWriterFactory, and the AuthorizationFactory. All of these factories will internally instantiate an “Instance Factory” for creating the desired objects.

## Instance Factories (NameTypeFactoryBase Factories)

Instance factories provide default and named instance factory methods for obtaining the desired objects from the specified configuration source. These factories are generally only used directly when a specific configuration source location is desired. In all cases within the Enterprise Library application blocks, specific instance factories only serve to provide strongly typed instances of type NameTypeFactoryBase. The instance factories, by virtue of extending NameTypeFactoryBase, interface with the EnterpriseLibraryFactory class which facilitates the calls to ObjectBuilder with the Enterprise Library defined Strategies.

Examples of these factories within Enterprise Library include the CacheManagerFactory, DatabaseProviderFactory, LogWriterFactory, and the AuthorizationProviderFactory.

## EnterpriseLibraryFactory

The EnterpriseLibraryFactory performs all the interaction with the underlying dependency injection subsystem. It provides static methods which interface with ObjectBuilder, and performs the initial setup of the standard Strategies and Policies used with the DI subsystem during the object creation life cycle. The key strategy used is the “ConfiguredObjectStrategy” which is discussed further in the Custom Factories section below.

## ObjectBuilder

The ObjectBuilder is a dependency injection (DI) / inversion of control (IoC) object creation pipeline framework which allows for highly customizable object creation strategies to be developed. The basic concept surrounds the use of Strategies which allows for tailoring what takes place during different stages of an object’s creation life-cycle. Enterprise Library has developed Strategies for the ObjectBuilder component to allow for configuration driven object creation among other specialized needs.

## Custom Factories

Custom factories are special factories designed to work with the Enterprise Library object creation system. Unlike the static and instance factories, custom factories are not intended to be used directly by code consuming the Enterprise Library application blocks.
  
At a high level, custom factories are used by the Enterprise Library object creation system to fulfill the work of creating the actual instance of the object.

Custom factories are used by the “ConfiguredObjectStrategy” of Enterprise Library which facilitates the delegation of the object’s creation to a factory class. The ConfiguredObjectStrategy is one of the PreCreation ObjectBuilder strategies used by Enterprise Library which maps the type being created with a factory for that type. This factory class is derived through reflection from the CustomFactoryAttribute of the type being created. Each custom factory is responsible for knowing the specific configuration section needed for the creation of the desired type and delegates the actual creation of the object to another class type known as an “Assembler”.

## Assemblers

Assemblers are at the end of the call chain and are the actual classes which create the desired type. These classes are used by the custom factories and are derived through reflection from the AssemblerAttribute of the ConfigurationElement type representing the type’s configuration data. The assembler is specific to a given type and knows how to use the configuration data to call specific constructors of the type being created.

## Conclusion

Hopefully this brief article has served as a good starting point for understanding the Enterprise Library configuration strategy. For those wishing to read a more detailed treatment of this topic, please refer to Brian Button’s blog entry: [Enterprise Library and Object Builder](http://www.agileprogrammer.com/oneagilecoder/archive/2006/01/03/10564.aspx).