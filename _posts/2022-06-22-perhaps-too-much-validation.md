---
title: "Perhaps Too Much Validation"
date: 2022-06-22T08:00:00+00:00
author: Derek Greer
layout: post
---

Several factors have influenced my coding style over the years leaving me with a preference toward lean code syntax. I’ve been developing for quite a while, so it would be hard to pinpoint exactly when, where, or from whom I’ve picked up various preferences, but to name a few, I prefer: code that only includes comments for public APIs or to provide explanation of algorithms; code that is free of the use of regions, explicit default access modifiers, and unused using statements; reliance upon convention over configuration (both to eliminate repetitive tasks, but also just to eliminate unnecessary code); encapsulating excessive parameters into a [Parameter Object](https://wiki.c2.com/?ParameterObject), avoidance of excessive use of attributes/annotations (actually, I’d eliminate them completely if I could), and of course deleting dead code. One special case of dead code that I often see is superfluous validation.

Perhaps you’ve seen code like this:

```csharp
public class MyService
{
    public DoSomething(IDependencyA dependencyA, IDependencyB dependencyB, IDependencyC dependencyC)
    {
        if(dependencyA is null)
        {
            throw new ArgumentNullException(nameof(dependencyA));
        }

        if(dependencyB is null)
        {
            throw new ArgumentNullException(nameof(dependencyB));
        }

        if(dependencyC is null)
        {
            throw new ArgumentNullException(nameof(dependencyC));
        }
    }

    …
}
```

Perhaps you even think this is a best practice. Is it? As with many things, the answer is really: It depends. One of the things that has greatly shaped my views on several aspects of software development over the years is adopting Test-Driven Development. The “test” part of the name is really a hold-over from adapting the practice of writing Unit Tests for driving design. With Unit Testing, you’re _testing_ the code you’ve written. With Test-Driven Development, you’re _constraining the design_ of the code to meet a set of specifications. It’s really quite a difference and one you may not fully appreciate unless you fully buy in to doing it for an extended period of time.

One of the side-effects of practicing TDD is that you don’t write code unless it’s needed to satisfy a failing test. The use of code coverage tools are basically superfluous for TDD practitioners. What, however, does this have to do with validation?

When driving out implementation through a series of executable specifications (i.e. an objective list of exactly how the software should work), we may end up writing code which technically _could_ be called a certain way which would result in exceptions or logical errors, but in _practice_ never is. As it relates to this topic, all code we write can be grouped into two categories: public and private. In this sense I’m not talking about the access modifiers we place upon the code artifacts themselves, but the intended use of the code. Is the code you’re writing going to be used by others, or is it just code we’re calling internally within our applications? If it’s code you’re driving out through TDD which others will be calling then you should have specifications which describe how the code will react when used correctly as well as incorrectly and thus will have the appropriate amount of validation. If it isn’t code anyone else will be, or currently is calling (see also YAGNI), then the components which _do_ call it will have been designed such that they don’t call the component incorrectly rending such validation useless.

Let’s consider our code again:

```csharp
public class MyService
{
    public DoSomething(IDependencyA dependencyA, IDependencyB dependencyB, IDependencyC dependencyC)
    {
        if(dependencyA is null)
        {
            throw new ArgumentNullException(nameof(dependencyA));
        }

        if(dependencyB is null)
        {
            throw new ArgumentNullException(nameof(dependencyB));
        }

        if(dependencyC is null)
        {
            throw new ArgumentNullException(nameof(dependencyC));
        }
    }

    …
}
```

If this is an internal service that isn’t going to be called by any other code except other components within your application, we have 14 lines of code that are unneeded and are just adding noise to our code. I’ve worked in shops where every class in an application or library was coded this way, effectively adding hundreds to thousands of lines of unneeded code. Like regions, comments, or poorly factored code, this adds to the cognitive load required for reading through and understanding the code and ultimately is unnecessary.

We could, of course, lessen the syntax noise by using the `ThrowIfNull()` method of `System.ArgumentNullException` as demonstrated in the following code:

```csharp
using static System.ArgumentNullException;

public class MyService
{
    public DoSomething(IDependencyA dependencyA, IDependencyB dependencyB, IDependencyC dependencyC)
    {
        ThrowIfNull(dependencyA);
        ThrowIfNull(dependencyB);
        ThrowIfNull(dependencyC);
    }

    …
}
```

If we needed validation other than checking for null, we could even write our own custom code that would provide similar terse improvements. This however, misses the point. If you're practicing TDD and this component is only ever called by other code within this project, it's effectively unused code. Sure, it would guard against bad practices such as reusing the code within a different context (potentially excluding the addition of equivalent specifications ensuring the code is used properly), having others modify the project code without using TDD, etc., but that's speculative planning for a future that may never come.

The bottom line is, you _already_ have guards against the code being used incorrectly. It's called the tests!
