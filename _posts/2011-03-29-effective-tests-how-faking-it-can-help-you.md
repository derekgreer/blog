---
id: 506
title: 'Effective Tests: How Faking It Can Help You'
date: 2011-03-29T17:58:30+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/2011/03/29/effective-tests-how-faking-it-can-help-you/
permalink: /2011/03/29/effective-tests-how-faking-it-can-help-you/
dsq_thread_id:
  - "628323759"
categories:
  - Uncategorized
tags:
  - TDD
  - Testing
---
In <noindex></noindex> <a href="http://lostechies.com/derekgreer/2011/03/28/effective-tests-a-test-first-example-part-1/" target="_blank">part 4</a> of our series, I presented a Test-Driven Development primer before beginning our exercise.&#160; One of the techniques I’d like to discuss a little further before we continue is the TDD practice of using fake implementations as a strategy for getting a test to pass.&#160; 

While not discounting the benefits of using the _Obvious Implementation_ first when a clear and fast implementation can be achieved, the recommendation to “_Fake It (Until You Make It)_” participates in several helpful strategies, each with their own unique benefits:

&#160;

## Going Green Fast

Faking it serves as one of the strategies for passing the test quickly. This has several benefits:

One, it provides rapid feedback that your test will pass when the expected behavior is met. This can be thought of as a sort of counterpart to "failing for the right reason".

Second, it has psychological benefits for some, which can aid in stress reduction through taking small steps, receiving positive feedback, and providing momentum. 

Third, it facilitates a "safety net" which can be used to provide rapid feedback if you go off course during a refactoring effort. 

&#160;

## Keeping Things Simple

Faking it serves as one of the strategies for writing _maintainable_ software.

Ultimately, we want software that works through the simplest means possible. The "Fake It" strategy, coupled with Refactoring (i.e. eliminating duplication) or Triangulation (writing more tests to prove the need for further generalization), leads to an additive approach to arriving at a solution that accommodates the needs of the specifications in a maintainable way.&#160; Faking It + Refactoring|Triangulation is a disciplined formula for achieving emergent design.

&#160;

## Finding Your Way

Faking it serves as a strategy for reducing mental blocks.&#160; 

As the ultimate manifestation of “_Do the simplest thing that could possibly work_", quickly seeing how the test can be made to pass tends to shine a bit of light on what the next step should be. Rather than sitting there wondering how to implement a particular solution, faking it and then turning your attention to the task of eliminating duplication or triangulating the behavior will push you in the right direction. 

&#160;

## Identifying Gaps

Faking it serves as a strategy for revealing shortcomings in the existing specifications. 

Seeing first hand how easy it is to make your tests pass can help highlight how an implementation might be modified in the future without breaking the existing specifications.&#160; Part of the recommended strategy for keeping your code maintainable is to remove unused generalization.&#160; Generalization&#160; which eliminates duplication is needed, but your implementation may include generalization for which the driving need isn’t particularly clear.&#160; Using a fake implementation can help uncover behavior you believe should be explicitly specified, but isn’t required by the current implementation.&#160; Faking it can lead to such questions as: “_If I can make it pass by doing anything that produces this value, what might prevent someone from altering what I’m thinking of doing to eliminate this duplication?_”

&#160;

## Conditioning

Lastly, faking it helps to condition you to seeing the simplest path first.&#160; When you frequently jump to the complex, robust, flexible solution, you’ll tend to condition yourself to think that way when approaching problems.&#160; When you frequently do simple things, you’ll tend to condition yourself to seeing the possible simplicity in the solution provided.

&#160;

## Conclusion

While we should feel free to use an _Obvious Implementation_ when present, The Test-Driven Development strategy of “_Fake It (Until You Make It)_” can play a part in several overlapping strategies which help us to write <u>working, maintainable software that matters</u>.