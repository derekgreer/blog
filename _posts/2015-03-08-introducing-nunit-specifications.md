---
id: 1108
title: Introducing NUnit.Specifications
date: 2015-03-08T09:24:03+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=1108
permalink: /2015/03/08/introducing-nunit-specifications/
dsq_thread_id:
  - "3577739666"
categories:
  - Uncategorized
tags:
  - BDD
  - TDD
  - Testing
---
&nbsp;

I <noindex></noindex> recently started working with a new team that uses NUnit as their testing framework.  While I think NUnit is a solid framework, I don’t think the default API and style lead to [effective tests](http://lostechies.com/derekgreer/2011/03/07/effective-tests-introduction/).

As an advocate of Test-Driven Development, I’ve always appreciated how [context/specification-style](http://www.codemag.com/article/0805061) frameworks such as [Machine.Specifications (MSpec)](https://github.com/machine/machine.specifications) allow for the expression of executable specifications which model how a system is expected to be used rather than the typical unit-test style of testing which tends to obscure the overall purpose of the system.

To facilitate a context/specification-style API, I created a base class which makes use of the hooks provided by the NUnit testing framework to emulate MSpec.  I’ve published this code under the project name [NUnit.Specifications](https://www.nuget.org/packages/NUnit.Specifications/).

The following is an example NUnit test written using the ContextSpecification based class from NUnit.Specifications using the [Should](https://github.com/erichexter/Should) assertion library:

[<img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image01" src="http://aspiringcraftsman.com/wp-content/uploads/2015/03/image01.png" alt="image01" width="574" height="379" border="0" />](http://aspiringcraftsman.com/wp-content/uploads/2015/03/image01.png){.thickbox}

One nice benefit of building on top of NUnit is the wide-spread tool support available.  Here is the test as seen through various test runners:

**Resharper Test Runner:**

[<img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image03" src="http://aspiringcraftsman.com/wp-content/uploads/2015/03/image03_thumb.png" alt="image03" width="574" height="196" border="0" />](http://aspiringcraftsman.com/wp-content/uploads/2015/03/image03.png){.thickbox}

**TestDriven.Net:** (see notes below)

[<img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image04" src="http://aspiringcraftsman.com/wp-content/uploads/2015/03/image04_thumb.png" alt="image04" width="574" height="47" border="0" />](http://aspiringcraftsman.com/wp-content/uploads/2015/03/image04.png){.thickbox}

**NUnit Test Runner:**

[<img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image00" src="http://aspiringcraftsman.com/wp-content/uploads/2015/03/image00_thumb.png" alt="image00" width="574" height="175" border="0" />](http://aspiringcraftsman.com/wp-content/uploads/2015/03/image00.png){.thickbox}

**NUnit Test Adaptor for Visual Studio:**

[<img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image02" src="http://aspiringcraftsman.com/wp-content/uploads/2015/03/image02_thumb.png" alt="image02" width="574" height="519" border="0" />](http://aspiringcraftsman.com/wp-content/uploads/2015/03/image02.png){.thickbox}

&nbsp;

<table border="1" width="400" cellspacing="0" cellpadding="2">
  <tr>
    <td valign="top" width="400">
    </td>
  </tr>
</table>

<table border="1" width="400" cellspacing="0" cellpadding="2">
  <tr>
    <td valign="top" width="400">
    </td>
  </tr>
</table>

<div class="note">
  <p>
    One caveat I discovered with the TestDriven.Net runner is it’s failure to recognize tests without the specification referencing types from the NUnit.Framework namespace (e.g. TestFixtureAttribute, CategoryAttribute, use of Assert, etc.).  That is to say, it didn’t seem to be enough that the spec inherited from a base type with NUnit attributes, but something in the derived class had to reference a type from the NUnit.Framework namespace for the test to be recognized.  Therefore, the TestDriven.Net results shown above were actually achieved by annotating the class with [Category(“component”)] explicitly.
  </p>
</div>

&nbsp;

**Other Stuff**

As a convenience, NUnit.Specifications also provides attributes for denoting categories of Unit, Component, Integration, Acceptance, and Subcutaneous as well as a Catch class (as provided by the MSpec library) for working with exceptions.

You can obtain the NUnit.Specifications from [NuGet](https://www.nuget.org/packages/NUnit.Specifications/1.0.1) or grab the source from [github](https://github.com/derekgreer/nunit.specifications).