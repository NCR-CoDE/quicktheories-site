---
layout: page
permalink: /examples/check-assert-examples/
---

<h2><a name = "check_assert" style="color:inherit; text-decoration: none !important;">Using checkAssert:</a></h2>

The checkAssert method is a final method taking any block of code that returns void as an argument.

<pre><code>  @Test
  public void someTheory() {
    qt().forAll(longs().all())
        .checkAssert(i -> assertThat(i).isEqualsTo(42));
  }
</code></pre>

