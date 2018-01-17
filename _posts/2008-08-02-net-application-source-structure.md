---
id: 18
title: .Net Application Source Structure
date: 2008-08-02T20:20:00+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/2008/08/net-application-source-structure/
permalink: /2008/08/02/net-application-source-structure/
dsq_thread_id:
  - "630537704"
categories:
  - Uncategorized
tags:
  - Application Structure
---
Developers new to .Net development often have questions about how best to structure and partition their projects and solutions. A good source of information on this topic is the [Team <noindex></noindex> Development with Visual Studio Team Foundation Server](http://www.codeplex.com/TFSGuide/) guide which contains a section outlining various recommended options for structuring projects and solutions based upon team and project size. The discussion on structuring solutions is beneficial even for teams not using Team Foundation Server as their version control system.

While the aforementioned guide provides guidance on partitioning large applications into multiple solutions, there is no detailed treatment given to the internal structure of solutions and the partitioning of applications into multiple projects. The following discusses some of the benefits to partitioning applications into multiple projects and discusses some approaches of how teams might structure projects within solutions.

Within .Net development, each project corresponds to an assembly. While all the code for an application may be contained in a single project which produces a single assembly, the following lists some advantages to partitioning applications into multiple assemblies.

  * **Architecture Decoupling/Abstraction** – enables the Interface pattern (PoEAA) and other patterns requiring the separation of interface from implementation.
  * **Solution Flexibility** – multiple solutions can be created for large projects to reduce the developer’s view of any given portion of the code base.
  * **Layer Promotion** – while logical layers can be achieved without compiling into separate assemblies, the physical boundaries dictated by separate assemblies provides an aid in both understanding and maintaining strict separation between logical application layers.
  * **Internal Type Support** – enables the designation of types only intended for use within the defined layer. This further promotes proper separation between logical application layers by preventing developers from the direct use of types not intended by the original designer.
  * **Deployment Efficiency** – only the delta need be deployed both to a deployment server and client machines.
  * Build Efficiency – only those assemblies which have changed need be recompiled during a build.
  * **Independent Versioning** – provides flexible management of components as well as enabling features such as Shadow Copying for portions of an application.
  * **Facilitates AppDomain Isolation** – enables the ability to load and unload smaller sections of your application (assemblies) in separate AppDomains while also providing isolation and access control.
  * **Solution Folder Organization** – enables the use of solution folders to logically group related projects (i.e. a “Business Layer” solution folder might contain separate projects for business entities, business workflows, and business components).

For client and service solutions, one common approach to partitioning is to create separate projects for each logical layer within the application. For example, an application following the guidelines outlined in the Microsoft publication: [Application Architecture for .NET: Designing Applications and Services](http://msdn2.microsoft.com/en-us/library/ms954595.aspx) might contain the following projects:

  * MyCompany.MyApplication.UI
  * MyCompany.MyApplication.Services
  * MyCompany.MyAppliation.Business
  * MyCompany.MyApplication.ServiceAgents
  * MyCompany.MyApplication.ServiceProxies
  * MyCompany.MyApplication.DataAccess
  * MyCompany.MyApplication.Common

For common infrastructure component libraries such as the functionality offered by the Patterns & Practices [Enterprise Library](http://www.codeplex.com/entlib) guidance, the best approach is to partition each logical component into its own project. This enables consumers of the infrastructure components to reference and deploy only the components required by their application.