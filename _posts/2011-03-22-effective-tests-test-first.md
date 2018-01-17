---
id: 469
title: 'Effective Tests: Test First'
date: 2011-03-22T01:08:37+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/default/2011/03/22/effective-tests-test-first/
permalink: /2011/03/22/effective-tests-test-first/
dsq_thread_id:
  - "261607288"
categories:
  - Uncategorized
tags:
  - BDD
  - TDD
  - Testing
---
In <noindex></noindex> <a href="/2011/03/14/effective-tests-a-unit-test-example/" target="_blank">part 2</a> of our series, I presented a simple example of a traditional unit test.&#160; In this article, we’ll discuss a completely different way to use unit tests: Test First Programming.

## Test-First Programming

In the book _Extreme Programming Explained: Embrace Change_ published in October 1999, Kent Beck introduced what was at the time a rather foreign concept to software development: Writing unit tests to drive software design.&#160; Described as _Test-First Programming_, the technique involves first writing a test to verify a small increment of desired functionality and then writing code to make the test pass.&#160; This process is then repeated until no additional functionality is desired.

## Incremental Design

Another practice introduced alongside Test-First Programming was _Incremental Design_.&#160; Incremental design is the process of evolving the design of an application as new functionality is developed and better design approaches are recognized.&#160; Due to the fact that software development is an inherently empirical process,&#160; following a defined model where the the software is designed up front generally leads to waste and rework.&#160; Incremental design postpones design decisions until needed, introducing patterns and frameworks as a byproduct of refactoring as well as eliminating any flexibility that is unused.

## Test Driven Development

Later, these ideas were&#160; refined into a practice known as _Test Driven Development_.&#160; Test-Driven Development can be described simply as the ideas of Test-First Programming coupled with Incremental Design.&#160; In a later book entitled _Test-Driven Development By Example_ published in November of 2002, Beck presents a refined process involving the writing of a failing test, taking the simplest steps possible to make the test pass, and removing any duplication introduced.&#160; This process was described succinctly as _Red/Green/Refactor_, where red and green referred to the colors typically presented by test runners to indicate failing and passing tests respectively.

## Behavior-Driven Development

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="Test Driven Development" border="0" alt="Test Driven Development" src="/wp-content/uploads/2011/03/TestDrivenDevelopment_thumb.png" width="494" height="330" />](/wp-content/uploads/2011/03/TestDrivenDevelopment.png)

&#160;

While the practice of Test-Driven Development utilized unit tests primarily for driving application design, its outgrowth from traditional unit testing practices were still evident in its style,&#160; language, and supporting frameworks. This presented an obstacle for some in understanding and communicating the concepts behind the practice.

In the course of teaching and practicing TDD, Dan North began encountering various strategies for designing tests in a more intent-revealing way which aided in his understanding and ability to convey the practice to others.&#160; As a way to help distinguish the differences between what he saw as an exercise in defining behavior rather than testing, he began describing the practice of Test-Driven Development as _Behavior Driven Development_.&#160; By shifting the language to the discussion about behavior, he found that many of the questions he frequently encountered in presenting TDD concepts became easier to answer.&#160; North went on to introduce a framework incorporating this shift in thinking called _JBehave_.&#160; North’s work on JBehave along with his promotion of the ideas behind Behavior-Driven Development served as the inspiration for the creation of many other frameworks including RSpec, Cucumber, NBehave, NSpec, SpecFlow, MSpec and many others.

## Behavior-Driven Development Approaches

Two main approaches have emerged from BDD practices which I’ll categorize as the Plain-Text Story approach and the Executable Specification approach.

### Plain-Text Stories

With the Plain-Text Story approach, software features are written as a series of plain-text scenarios which the framework associates with executable steps used to drive feature development.&#160; This approach is typically associated with a style referred to _Given/When/Then_ (GWT), named after the grammar set forth by Dan North.&#160; This style is facilitated by popular BDD frameworks such as Cucumber and JBehave.

The following is an example story using [Gherkin](https://github.com/aslakhellesoy/cucumber/wiki/gherkin) grammar, the language used by Cucumber and other Plain-Text Story BDD frameworks:

<font face="Verdana">Feature: Addition <br />&#160;&#160; In order to calculate totals <br />&#160;&#160; As a user <br />&#160;&#160; I want to calculate the sum of multiple numbers</font>

<font face="Verdana">Scenario: Add two numbers <br />&#160;&#160; Given I have entered 10 into the calculator <br />&#160;&#160; And I have entered 50 into the calculator <br />&#160;&#160; When I press add <br />&#160;&#160; Then the result should be 60 on the screen</font>

&#160;

Using Cucumber, the “Given” step in this scenario may then be associated to the following step definition in Ruby:

<pre class="ruby brush:ruby" name="code">Given /I have entered (\d+) into the calculator/ do |n|
  calc = Calculator.new
  calc.push n.to_i
end</pre>

&#160;

Using SpecFlow, the same “Given” step can also be associated to the following step definition in C#:

<pre class="csharp brush:csharp" name="code">[Given("I have entered (.*) into the calculator")]
public void GivenIHaveEnteredSomethingIntoTheCalculator(int number)
{
    _calculator = new Calculator();
    _calculator.Enter(number);
}</pre>

### Executable Specifications

In contrast to the use of plain-text stories, the Executable Specification approach uses source code as the medium for expressing specifications in the language of the business.&#160; Executable Specifications can be written using existing unit-testing frameworks such as JUnit, NUnit, and MSTest, or are written in frameworks designed exclusively to facilitate the Behavior-Driven Development style such as RSpec and MSpec.

One style, referred to as _Context/Specification_, expresses software features in terms of specifications within a given usage context.&#160; In this style, each unique way the software will be used is expressed as a context along with one or more expected outcomes.

While specialized frameworks have been created to support the Context/Specification style, this approach can also be facilitated using existing xUnit frameworks.&#160; This is generally accomplished by creating a Context base class which uses the Template Method pattern to facilitate the desired language.

The following example demonstrates this technique using the NUnit framework:

<pre class="csharp brush:csharp" name="code">[TestFixture]
    public abstract class Context
    {
        [TestFixtureSetUp]
        public void setup()
        {
            establish_context();
            because_of();
        }

        [TestFixtureTearDown]
        public void teardown()
        {
            cleanup();
        }

        protected virtual void establish_context()
        {
        }

        protected virtual void because_of()
        {
        }

        protected virtual void cleanup()
        {
        }
    }</pre>

&#160;

In addition to providing a Contact base class, the NUnit framework’s TestAttribute can also be sub-classed to aid in moving toward specification-oriented language:

&#160;

<pre class="csharp brush:csharp" name="code">public class ObservationAttribute : TestAttribute {}</pre>

&#160;

By extending the Context base class, NUnit can then be used to write Context/Specification style BDD specifications:

&#160;

<pre class="csharp brush:csharp" name="code">public class when_adding_two_numbers : Context
{
    Calculator _calculator;

    protected override void establish_context()
    {
        _calculator = new Calculator();
        _calculator.Enter(2);
        _calculator.PressPlus();
        _calculator.Enter(2);
    }

    protected override void because_of()
    {
        _calculator.PressEnter();
    }

    [Observation]
    public void it_should_return_the_correct_sum()
    {
        Assert.AreEqual(_calculator.Result, 4); 
    }
}</pre>

## Working, Maintainable Software That Matters

The ideas behind Test-First Programming continue to be refined by the software development community and have lead to still other variations in name and style, including Example Driven-Development, Specification-Driven Development, Need-Driven Development, and Story Test-Driven Development.&#160; Regardless of its label or flavor, the refinements of Test-First Programming can be distilled down to the following goal: The creation of working, maintainable software that matters.&#160; That is to say, the software should be functional, should be easily maintainable and should be relevant to needs of our customers.

## Conclusion

This time we discussed Test-First Programming and traced its steps to today’s practice of Behavior-Driven Development.&#160; Next time we’ll discuss Test-Driven Development in more detail and walk through a Test First example using the TDD methodology while incorporating some of the Behavior-Driven Development concepts presented here.