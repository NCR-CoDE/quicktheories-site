---
layout: page
permalink: /examples/sources-examples/
---
<h2>Using Multiple Sources</h2>
We can creates theories about any number of values between 1 and 4. In the example below, we use three Sources - as you can see, QuickTheories provides ways of generating most common types.

<pre><code>  @Test
  public void someTheoryOrOther(){
    qt()
    .forAll(integers().allPositive()
          , strings().basicLatinAlphabet().ofLengthBetween(0, 10)
          , lists().allListsOf(integers().all()).ofSize(42))
    .check((i,s,l) -> l.contains(i) && s.equals(""));
  }
</code></pre>
