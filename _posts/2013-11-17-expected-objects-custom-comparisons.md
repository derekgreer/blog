---
id: 1081
title: Expected Objects Custom Comparisons
date: 2013-11-17T21:34:15+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=1081
permalink: /2013/11/17/expected-objects-custom-comparisons/
dsq_thread_id:
  - "1975373020"
categories:
  - Uncategorized
tags:
  - BDD
  - TDD
  - Testing
---
<a href="http://www.nuget.org/packages/ExpectedObjects/" target="_blank">ExpectedObjects</a> <noindex></noindex> is a testing library I developed a few years ago to facilitate using the <a href="http://lostechies.com/derekgreer/2011/06/24/effective-tests-expected-objects/" target="_blank">Expected Objects pattern</a> within my specifications to avoid obscure tests.&#160; You can find the original introduction to the library <a href="http://lostechies.com/derekgreer/2011/06/28/introducing-the-expected-objects-library/" target="_blank">here</a>.

As of version 1.1.0, the ExpectedObjects library has been updated to include a feature called Custom Comparisons.&#160; The standard behavior of the library is to traverse a strategy chain (which is itself configurable) to determine which comparison strategy is to be used for each type of object encountered within the object graph.&#160; The Custom Comparisons feature allows you to override this behavior for specific properties.

For example, let’s say we’re writing a end-to-end test which validates a Receipt class as follows:

<pre class="prettyprint">public class Receipt

{
    public string Name { get; set; }
    public DateTime TransactionDate { get; set; }
    public string VerificationCode { get; set; }
}</pre>

&#160;

Given the following class, the VerificationCode property would probably not be a value you could anticipate.&#160; In such a case, while you can’t verify that the property has a specific value, you may care that it at least has some value.&#160; This is where the Custom Comparisons feature can help.&#160; We can verify that the actual Receipt received matches the expected receipt structure using the following expected object configuration:

<pre class="prettyprint">var expected = new
{
	Name = "John Doe",
	DateTime = DateTime.Today,
	VerificationCode = Expect.NotNull()
}.ToExpectedObject();


var actual = new Receipt
{
	Name = "John Doe",
	DateTime = DateTime.Today,
	VerificationCode = "ABC123"
};



expected.ShouldMatch(actual);</pre>

In the event that the VerificationCode property is null, the library will raise an exception with the following message:

<pre class="prettyprint">For Receipt.VerificationCode, expected a non-null value but found [null].</pre>

The ExpectedObjects library currently provides a static Expect class which&#160; includes convenience methods to check for null, not null, and an Any<T> comparison for checking that an object is of a specific type (e.g. Expect.Any<Receipt>()).&#160; To supply your own comparisons, simply implement the IComparsion interface which defines the custom comparison and the text to include within any exception messages raised (e.g. “For SomeType.SomeProperty, expected [_text you supply here_] but found “42”).