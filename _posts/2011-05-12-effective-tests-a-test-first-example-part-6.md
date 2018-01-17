---
id: 611
title: 'Effective Tests: A Test-First Example - Part 6'
date: 2011-05-12T13:01:31+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=611
permalink: /2011/05/12/effective-tests-a-test-first-example-part-6/
dsq_thread_id:
  - "628323884"
categories:
  - Uncategorized
tags:
  - TDD
  - Testing
---
In <noindex></noindex> the [last](http://www.aspiringcraftsman.com/2011/05/01/effective-tests-a-test-first-example-part-5/) installment of our series, we continued our Test-First example by addressing issues filed by the QA team. While I thought we had covered the reported defects pretty well, I wanted to do a little smoke testing against the full application to ensure we hadn’t missed anything. It’s probably good that I did, because I ended up finding one last case that didn’t met the original requirements.

In the course of my testing, I discovered that there was an additional way for a player to beat the game by setting up multiple winning paths. Here’s the steps I took:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="tic-tac-toe-ms" border="0" alt="tic-tac-toe-ms" src="/wp-content/uploads/2011/05/tic-tac-toe-ms_thumb2.png" width="598" height="108" />](/wp-content/uploads/2011/05/tic-tac-toe-ms2.png)

In the depicted steps, my first move was to choose the right edge of the board. This happens to be a strategy the articles I consulted with advised against and for which no counter strategy was provided. By choosing a second position which avoided triggering the game&#8217;s existing defensive strategies, I was able to set up multiple winning paths by countering the next two choices by the game. The game should be able to counter this strategy by blocking at intersections, so let&#8217;s fix this one last issue.

First, let&#8217;s start with a new test which describes the behavior we want:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
   [TestClass]
   public class When_the_player_has_two_paths_which_intersect_at_an_available_position
   {
       [TestMethod]
       public void it_should_select_the_intersecting_position()
       {
       }
   }
<br />
</pre>
</div>

&nbsp;

Next, let’s setup the context and assertion that reflects the way I was able to beat the game:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
   [TestClass]
   public class When_the_player_has_two_paths_which_intersect_at_an_available_position
   {
       [TestMethod]
       public void it_should_select_the_intersecting_position()
       {
           GameAdvisor gameAdvisor = new GameAdvisor();
           var selection = gameAdvisor.WithLayout("OXO\0\0XX\0\0").SelectBestMoveForPlayer('O');
           Assert.AreEqual(8, selection);
       }
   }
<br />
</pre>
</div>

&nbsp;

Now, let’s run our test suite:

<div style="background: red">
  &nbsp;
</div>

<pre><div style="overflow:auto;background-color: #FFFFF;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  When_the_player_has_two_paths_which_intersect_at_an_available_position
  Failed	it_should_select_the_intersecting_position
  Assert.AreEqual failed. Expected:&lt;8>. Actual:&lt;9>.
  
</div></pre>



&nbsp;

Let’s get this to pass quickly by returning the expected position for this exact layout:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
       class PositionSelector : IPositionSelector
       {
           …

           public int SelectBestMoveForPlayer(char player)
           {
               <b>if (_layout == "OXO\0\0XX\0\0")
                   return 8;</b>

               return GetPositionThreateningPlayer(player) ??
                      GetNextWinningMoveForPlayer(player) ??
                      Enumerable.Range(1, 9).First(position => _layout[position - 1] == Game.EmptyValue);
           }

           ...
       }
<br />
</pre>
</div>

&nbsp;

<div style="background:#3C0">
  &nbsp;
</div>

&nbsp;

To remove the duplication of the layout, let’s start taking small steps toward a final solution. First, let’s create a new DefensiveStrategy for dealing with positions that might allow the opponent to set up multiple winning paths:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
       class PositionSelector : IPositionSelector
       {
           …

           int? GetPositionThreateningPlayer(char player)
           {
               return new DefensiveStrategy[]
                          {
                              PathCompletionStrategy,
                              SimpleBlockStrategy,
                              FirstMoveCounterCenterStrategy,
                              SecondMoveDiagonalCounterStrategy,
                              <b>MultiPathCounterStrategy</b>
                          }
                   .Select(strategy => strategy(player)).FirstOrDefault(p => p.HasValue);
           }

           <b>int? MultiPathCounterStrategy(char player)
           {
               if (_layout == "OXO\0\0XX\0\0")
                   return 8;

               return null;
           }</b>


           ...
       }
<br />
</pre>
</div>

&nbsp;

Next, let’s work through the steps we’ll need to arrive at this value. First, we’ll need to get a list of all the available paths for the opponent:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
           int? MultiPathCounterStrategy(char player)
           {

               <b>char opponentValue = GetOpponentValue(player);

               List&lt;int[]> opponentPaths = GetAvailablePathsFor(opponentValue);</b>

               if (_layout == "OXO\0\0XX\0\0")
                   return 8;

               return null;
           }
<br />
</pre>
</div>

&nbsp;

Next, we want to filter this list down to the paths the opponent has already started:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
           int? MultiPathCounterStrategy(char player)
           {

               char opponentValue = GetOpponentValue(player);

               List&lt;int[]> opponentPaths = GetAvailablePathsFor(opponentValue);

               <b>IEnumerable&lt;int[]> startedPaths =
                   opponentPaths.Where(path => new string(path.Select(p => _layout[p - 1]).ToArray())
                                                   .Count(value => value == opponentValue) >= 1);</b>

               if (_layout == "OXO\0\0XX\0\0")
                   return 8;

               return null;
           }
<br />
</pre>
</div>

&nbsp;

Lastly, we need to compare all the paths to each other, find the ones that have positions in common, and pick the position in common for the first pair. Since we’ll be calling this after our other strategy for dealing with multiple winning paths as the result of the opponent choosing opposite corners, this should only ever find one pair. This logic seems a little more complicated, so I’m going to write it in LINQ rather than using extension methods this time:

<div style="overflow:auto;background-color: #FFFFE0;border: 1px solid black;border-collapse: collapse;color: black;float: none;font-family: Consolas, 'Bitstream Vera Sans Mono', 'Courier New', Courier, monospace;font-size: 12px;font-style: normal;font-variant: normal;font-weight: normal;line-height: 20px;padding: 5px;text-align: left;vertical-align: baseline">
  <pre>
           int? MultiPathCounterStrategy(char player)
           {

               char opponentValue = GetOpponentValue(player);

               List&lt;int[]> opponentPaths = GetAvailablePathsFor(opponentValue);

               IEnumerable&lt;int[]> startedPaths =
                   opponentPaths.Where(path => new string(path.Select(p => _layout[p - 1]).ToArray())
                                                   .Count(value => value == opponentValue) >= 1);

               <b>return (from path in startedPaths
                       from position in path
                       from otherPath in startedPaths.SkipWhile(otherPath => otherPath == path)
                       from otherPosition in otherPath
                       where otherPosition == position
                       where _layout[position - 1] == Game.EmptyValue
                       select new int?(position)).FirstOrDefault();</b>
           }
<br />
</pre>
</div>

&nbsp;

Let’s run the tests again and see what happens:

<div style="background:#3C0">
  &nbsp;
</div>

&nbsp;

It passes! We can now release the new version of our component to be integrated into the next build.

&nbsp;

## Conclusion

We’ve finally come to the conclusion of our Test-First example. Along the way, we followed the Test-Driven Development practice of writing a failing test first, making the test pass as quickly as possible, and refactoring to remove duplication and to clarify intent. When we wrote a failing test, we made sure each test was properly verifying the expected results before attempting to add new behavior to the system. To get our tests passing quickly, we used obvious implementations when we were confident about our approach and felt we could achieve it quickly, but used fake implementations when we weren’t as confident or felt the implementation might take some time. When we wrote a new test to capture new requirements that were already present in the system, we temporarily disabled the behavior of the system to ensure our tests were validating the expected behavior correctly. Throughout our effort, we weren’t afraid to take small steps and strove to do simple things.

While we made some mistakes along the way and discovered opportunities for further improvement, using the Test Driven Development approach aided in our ability to produce working, maintainable software that matters.

Next time, we’ll discuss concepts and strategies for writing tests for collaborating components.
