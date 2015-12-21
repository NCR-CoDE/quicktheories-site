---
layout: page
title: Miscellaneous Examples
permalink: /examples/extra/
---
<h4>How to set up a simple property test:</h4>
<pre><code>
import static org.quicktheories.quicktheories.QuickTheory.qt;
import static org.quicktheories.quicktheories.generators.SourceDSL.*;

public class SomeTests {

  @Test
  public void addingTwoPositiveIntegersAlwaysGivesAPositiveInteger(){
    qt()
    .forAll(integers().allPositive()
          , integers().allPositive())
    .check((i,j) -> i + j > 0); 
  }

}
</code></pre>

If we run this test we get something like :-

<pre><code>java.lang.AssertionError: Property falsified after 1 example(s) 
Smallest found falsifying value(s) :-
{840226137, 1309274625}
Other found falsifying value(s) :- 
{848253830, 1320535400}
{841714728, 1317667877}
{840894251, 1310141916}
{840226137, 1309274625}
 
Seed was 29678088851250	
</code></pre>
<br/>
<h4>Greatest Common Divisor example:</h4>

An example of multiple tests for code that claims to find the greatest common divisor between two integers. The first property test fails due to a java.lang.StackOverflowError error (caused by attempting to take the absolute value of Integer.MIN_VALUE).

<pre><code>
 @Test
  public void shouldFindThatAllIntegersHaveGcdOfOneWithOne() {
    qt().forAll(integers().all()).check(n -> gcd(n, 1) == 1); // fails on
                                                              // -2147483648
  }

  @Test
  public void shouldFindThatAllIntegersInRangeHaveGcdOfOneWithOne() {
    qt().forAll(integers().between(-Integer.MAX_VALUE, Integer.MAX_VALUE))
        .check(n -> gcd(n, 1) == 1);
  }

  @Test
  public void shouldFindThatAllIntegersHaveGcdThemselvesWithThemselves() {
    qt().forAll(integers().between(-Integer.MAX_VALUE, Integer.MAX_VALUE))
        .check(n -> gcd(n, n) == Math.abs(n));
  }

  @Test
  public void shouldFindThatGcdOfNAndMEqualsGcdMModNAndN() {
    qt().forAll(integers().between(-Integer.MAX_VALUE, Integer.MAX_VALUE)
               ,integers().between(-Integer.MAX_VALUE, Integer.MAX_VALUE))
        .check((n, m) -> gcd(n, m) == gcd(m % n, n));
  }

  private int gcd(int n, int m) {
    if (n == 0) {
      return Math.abs(m);
    }
    if (m == 0) {
      return Math.abs(n);
    }
    if (n < 0) {
      return gcd(-n, m);
    }
    if (m < 0) {
      return gcd(n, -m);
    }
    if (n > m) {
      return gcd(m, n);
    }
    return gcd(m % n, n);
  }
</code></pre>



