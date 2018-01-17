---
id: 931
title: 'TDD Best Practices: Write Assertions First'
date: 2012-03-05T02:07:00+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=931
permalink: /2012/03/05/tdd-best-practices-write-assertions-first/
dsq_thread_id:
  - "629448664"
categories:
  - Uncategorized
tags:
  - BDD
  - TDD
---
When <noindex></noindex> practicing Test-Driven Development, developers often organize their tests following a style first described by Bill Wake as _Arrange, Act, Assert_.&#160; This division between the setup, the exercising the System Under Test, and the assertions are also reflected in the Context/Specification and Given/When/Then (GWT) styles of Behavior-Driven Development (with the GWT style often containing multiple “Act” steps).&#160; This division is generally a Good Thing, as it guides the developer toward the authoring of well-organized and readable tests which provide a good template for modeling most specifications.&#160; Unfortunately, developers are often lead to implement tests in the order of Arrange, Act, Assert which serves to undermine the purpose of practicing TDD: to allow the needs of the test to guide the design of the system.

When transforming a requirement into an executable specification, after establishing the logical class or object which will contain the test implementation, the first step should be to determine how the specification (i.e. the test) is to be validated.&#160; That is to say, the first step is to write an assertion.

The goal of Test-Driven Development is to produce clean code that works.&#160; By ‘clean’, we mean code that’s free of unnecessary behavior or abstractions.&#160; The TDD techniques of first doing the simplest thing that could possibly work, refactoring to remove duplication, and using triangulation for identifying the need for generalizations are set forth to _restrain_ developers from writing code that isn’t actually needed.&#160; This leads to simpler, more maintainable code.&#160; Starting with the setup implementation before determining how you’re going to verify the desired outcome is putting the cart before the horse.&#160; How do you know a particular setup implementation will be needed to test the desired outcome before you’ve established what that is?&#160; Perhaps you’ll be building upon the existing type you’re jumping ahead to setup, but perhaps it should be a new type altogether.&#160; Figure out how you’re going to verify the desired outcome first and then determine what components make sense to fulfill the desired outcome.

To help illustrate this flow, let’s consider the following example.&#160; Let’s say we’re writing an application which allows employee’s at a company to register for a training class.&#160; When an employee registers for a class, they should be provided with a registration receipt.&#160; Let’s start by establishing the shell of our specification:

<pre class="prettyprint">public class when_registering_for_a_class
{
  Establish context;

  Because of;

  It should_return_a_registration_receipt;
}</pre>

Next, we need to determine how we want to verify that the logical condition “_It should return a registration receipt_” will be fulfilled.&#160; Let’s assert that a variable named _registrationReceipt is not null:

<pre class="prettyprint">public class when_registering_for_a_class
{
  Establish context;

  Because of;

  It should_return_a_registration_receipt = () =&gt; _registrationReceipt.ShouldNotBeNull();
}</pre>

It’s at this point some may have an objection to typing out an assertion without the aid of auto-completion (i.e. Intellisense).&#160; Some are so dependent upon auto-completion that they’ll actually stop after typing _registrationReceipt, and go declare a variable just so their auto-completion will be there.&#160; Resist that urge.&#160; Assertions shouldn’t be difficult and this will actually force you to keep it simple.

Next, we need to decide how we are going to assign our variable, so we’ll move to our Because delegate.&#160; At this point, we’ll also determine what component and usage API we’d like to use to retrieve our receipt:

<pre class="prettyprint">public class when_registering_for_a_class
{
  Establish context;

  Because of = () =&gt; _registrationReceipt = _registrar.Register(EmployeeId, ClassId);

  It should_return_a_registration_receipt = () =&gt; _registrationReceipt.ShouldNotBeNull();
}</pre>

Next, we need to establish our context.&#160; We’ll initialize the _registrar to a non-existent type named Registrar:

<pre class="prettyprint">public class when_registering_for_a_class
{
  Establish context = () =&gt; { _registrar = new Registrar(); };

  Because of = () =&gt; _registrationReceipt = _registrar.Register(EmployeeId, ClassId);

  It should_return_a_registration_receipt = () =&gt; _registrationReceipt.ShouldNotBeNull();
}</pre>

At this point, we can use ReSharper to generate a field name _registrar, generate the Registrar class, and generate the Registrar.Register() method.&#160; Once the Register() method is generated, we need to choose the type we’ll use for the return value and parameters and then replace the throw statement with a return of null so our assertion will fail for the right reason later.&#160; Let’s make the receipt be a RegistrationReceipt type and use integers for our Ids:

<pre class="prettyprint">public class when_registering_for_a_class
{
  static Registrar _registrar;

  Establish context = () =&gt; { _registrar = new Registrar(); };

  Because of = () =&gt; _registrationReceipt = _registrar.Register(EmployeeId, ClassId);

  It should_return_a_registration_receipt = () =&gt; _registrationReceipt.ShouldNotBeNull();
}

class Registrar
{
  public RegistrationReceipt Register(int employeeId, int classId)
  {
  }
}</pre>

Continuing, we generate the RegistrationReceipt type, _registrationReceipt field, and create some constants for our Ids:

<pre class="prettyprint">public class when_registering_for_a_class
{
  const int EmployeeId = 1;
  const int ClassId = 2;
  static Registrar _registrar;
  static RegistrationReceipt _registrationReceipt;

  Establish context = () =&gt; { _registrar = new Registrar(); };
  
  Because of = () =&gt; _registrationReceipt = _registrar.Register(EmployeeId, ClassId);

  It should_return_a_registration_receipt = () =&gt; _registrationReceipt.ShouldNotBeNull();
}

class Registrar
{
  public RegistrationReceipt Register(int employeeId, int classId)
  {
    return null;
  }
}

class RegistrationReceipt
{
}</pre>

&#160;

At this point everything compiles cleanly.&#160; Running our test with the ReSharper test runner produces the following:

<pre class="prettyprint">should return a registration receipt : Failed
Should be [not null] but is [null]</pre>

We’re now ready to make our test pass (which we can do by just returning a new RegistrationReceipt from our Register method), factor out any duplication, and move on to our next assertion or specification either to flesh out this design or to move on to our next feature.

By starting with our assertion first, moving to the execution of our System Under Test, and ending with our context setup, we’ve allowed our test to guide our design rather than allowing an existing design (coded or in our heads) to guide the implementation of our test.

In summary, organize your tests using _Arrange, Act, Assert_, but implement them in the order of _Assert, Act, Arrange_.