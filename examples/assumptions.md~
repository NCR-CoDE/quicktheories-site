---
layout: page
permalink: /examples/assumption-examples/
---
<h2>Using Assumptions</h2>
The Sources DSL provides a way to put constraints on the values we generate (e.g we will only generate Strings of length between 0 and 10, and the lists in this example will only be of size 42).

If you need to constrain the domains in ways that the DSL cannot express, assumptions provide a way to further constrain the values which form the subject of the theory.


<pre><code>  @Test
  public void someTheoryOrOther() {
    qt()
    .forAll(integers().allPositive()
          , strings().basicLatinAlphabet().ofLengthBetween(0, 10)
          , lists().allListsOf(integers().all()).ofSize(42))
    .assuming((i,s,l) -> s.contains(i.toString())) // <-- an assumption
    .check((i,s,l) -> l.contains(i));
  }
</code></pre>


Although we could always replace the constraints we created in the Sources DSL with assumptions, this would be very inefficient. QuickTheories would have to spend a lot of effort just trying to find valid values before it could try to invalidate a theory.

As difficult to find values probably represent a coding error, QuickTheories will throw an error if less than 10% of the generated values pass the assumptions:

<pre><code>  @Test
  public void badUseOfAssumptions() {
    qt()
    .forAll(integers().allPositive())
    .assuming(i -> i < 30000)
    .check( i -> i < 3000);
  } </code></pre>

Gives:

<pre>
java.lang.IllegalStateException: Gave up after finding only 107 example(s) matching the assumptions
    at org.quicktheories.quicktheories.core.ExceptionReporter.valuesExhausted(ExceptionReporter.java:20)
</pre>

This assumption could have been replaced by the following:

<pre><code>  @Test
  public void goodUseOfSource(){
    qt().forAll(integers().from(1).upTo(30000))
    .check( i -> i < 3000);
  }  </code></pre>

Which gives the following failure message:

<pre><code>java.lang.AssertionError: Property falsified after 1 example(s) 
Smallest found falsifying value(s) :-
3000
Other found falsifying value(s) :- 
13723
13722
13721
13720
13719
13718
13717
13716
13715
13714

Seed was 2563360080237
</code></pre>
