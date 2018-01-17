---
id: 773
title: How To Produce Bug-Free Software
date: 2011-10-18T13:20:00+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=773
permalink: /2011/10/18/how-to-produce-bug-free-software/
dsq_thread_id:
  - "634234334"
categories:
  - Uncategorized
tags:
  - Testing
---
Many <noindex></noindex> are resigned to the fact that all software is destined to contain some “bugs”, but did you know it’s possible (and arguably pretty easy) to always produce “bug-free” software?&#160; In this article, I’ll explain how.

## Terms

To begin, let’s consider the definition of a software “bug”.&#160; Merriam-Webster’s dictionary defines a bug as follows:

> **bug** &#8211; “an unexpected defect, fault, flaw, or imperfection”

This definition may be fine for casual use, but you certainly wouldn&#8217;t want to use this as the basis for any contractual obligations.&#160; The problem with this definition is that it lacks objectivity.&#160; The terms ‘defect’, ‘fault’, ‘flaw’, and ‘imperfection’ are all relative terms, but what are they relative to?&#160; Based upon this definition, these designations are made based upon some deviation from an unstated set of expectations, but upon what are these expectations based?&#160; What if different expectations are held independently by your users, testing groups, mangers, product owners and yourself?&#160; Should all differences between any of these expectations and the actual behavior be considered bugs?&#160; Clearly a more objective definition is needed if we are to ever be capable of producing bug-free software.

To this end, I submit the following definition:

> **bug** &#8211; “a deviation from an objective set of specifications set forth at the outset of a development effort.”

In order for a development team to consistently write bug-free software, an objective set of specifications must exist by which the software may be measured by prior to delivery.&#160; Unfortunately, such specifications are rarely created … not in any objective form.&#160; This problem often stems from the application of an incorrect process control model.

## Process Control Theory

There are two primary approaches to controlling processes:&#160; The [defined process control model](http://en.wikipedia.org/wiki/Defined_process) and the [empirical process control model](http://en.wikipedia.org/wiki/Empirical_process_(process_control_model)).

The **defined process control model** is applicable to processes where all the work involved is completely understood at the outset of production and the result of each stage of production is predictable and repeatable.&#160; An assembly-line production of automobiles is a example where a defined process model might be applied.

The **empirical process control model** is applicable to processes where the product of the work has a high degree of unpredictability and/or the product’s specifications aren’t completely defined and understood at the outset of production.&#160; This model is characterized by the use of frequent inspection and adaptation during the production process.&#160; The research and initial formulation of medications to treat diseases are examples where an empirical process control model would be applied.

Unfortunately, it’s been the defined process control model which has been the predominate approach to software development throughout its history.&#160; The most prevalent manifestation of the defined process control model has been the&#160; [Waterfall model](http://en.wikipedia.org/wiki/Waterfall_model), a process control model characterized by sequential stages of development which include requirements gathering, analysis, design, implementation, and verification.&#160; Fortunately some within the industry have come to understand that software development requires an empirical process control model and the rest of the industry appears to be slowly coming around<sup>1</sup>.

In the book _Agile Software Development with Scrum_, Ken Schwaber discusses some feedback he received upon consulting with process theory experts concerning his lack of success in applying commercial methodologies within his company.&#160; The following is his account of this feedback:

> _They inspected the systems development processes that I brought them.&#160; I have rarely provided a group with so much laughter.&#160; They were amazed and appalled that my industry, systems development, was trying to do its work using a completely inappropriate process control model.&#160; They said systems development had so much complexity and unpredictability that it had to be managed by a process control model they referred to as “empirical.”&#160; They said this was nothing new, and all complex processes that weren’t completely understood required the empirical model.&#160; They helped me go through a book that is the Bible of industrial process control theory, Process Dynamics, Modeling and Control [Tunde] to understand why I was off track._ 
> 
> _In a nutshell, there are two major approaches to controlling any process.&#160; The “defined” process control model requires that every piece of work be completely understood.&#160; Given a well-defined set of inputs, the same outputs are generated every time. … Tunde told me the empirical model of process, on the other hand, expects the unexpected.&#160; It provides and exercises control through frequent inspection and adaptation for processes that are imperfectly defined and generate unpredictable and unrepeatable outputs._

When software development is correctly understood to be an inherently empirical process in need of frequent inspection and adjustment, many naturally conclude that an objective, repeatable inspection process is need.&#160; Enter executable specifications &#8230;

## Executable Specifications

In the [Scientific Method](http://en.wikipedia.org/wiki/Scientific_method), empirical data is collected through repeatable processes to guard against mistakes or confusion by experimenters.&#160; To ensure our software is defect-free, we need to employ a similar set of repeatable processes.&#160; Not only should such processes guard against mistakes or confusion during the development effort, but such processes can and should themselves be the agreed-upon specifications.&#160; In the software development world, these sets of controlled processes can be fulfilled by Executable Specifications.&#160; 

Simply, executable specifications are a set of automated tests which encapsulate the context, actions, and observable outcomes defined and agreed upon between a customer and a development team. Without such specifications, no objective measure exists by which to weigh the integrity of a software system.&#160; With such specifications, the development team has an objective measure by which to weigh the software behavior against prior to it being delivered to a customer … thus equipping the development team with the ability to always produce bug-free software.

&#160;

* * *

1. &#8216;Agile&#8217; Development Winning Over &#8216;Waterfall&#8217; Method &#8211; <http://itmanagement.earthweb.com/entdev/article.php/3841636/Agile-Development-Winning-Over-Waterfall-Method.htm>