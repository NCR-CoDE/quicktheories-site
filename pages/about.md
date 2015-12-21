---
layout: page
title: About
permalink: /about/
---

<h2><a name = "property_testing" style="color:inherit; text-decoration: none !important;">What is property based testing?</a></h2>

Traditional unit testing is performed by specifying a series of concrete examples and asserting on the outputs/behaviour of the unit under test.

Property based testing moves away from concrete examples and instead checks that certain properties hold true for all possible inputs. It does this by automatically generating a random sample of valid inputs from the possible values.

This can be a good way to uncover bad assumptions made by you and your code.

If the word random is making you feel a little nervous, don't worry QuickTheories provides ways to keep your tests repeatable.

<br /> 
<h2>Design Goals</h2>
QuickTheories was written with the following design goals
<ol>
    <li>Random by default, but builds must be repeatable</li>
    <li>Support for shrinking</li>
    <li>Independent of test api (JUnit, TestNG etc)</li>
</ol>

It turned out that number 2 was the hard bit as it had many implications for the design.

<br /> 
<h2>Who produced QuickTheories?</h2>

QuickTheories was produced at [NCR Edinburgh](http://ncredinburgh.com/) as part of our graduate training program.

We like to do training a little differently - our [new graduates](https://github.com/katyrae) get to work on an interesting project for a weeks with a more worn in and [weathered member of our staff](https://github.com/hcoles). Our motto for these projects is "software that can fail" - so we get to play with interesting ideas that may come to nothing.

We're happy to share the results as open source when we think they're successful.
