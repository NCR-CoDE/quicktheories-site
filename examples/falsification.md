---
layout: page
permalink: /examples/falsification-examples/
---
<h2>Using describedAs</h2>

Using the method `describedAs`, we can alter how generated values will appear in potential falsification methods:

<pre><code>@Test
  public void someTestInvolvingCylinders() {
      qt()
      .forAll(integers().allPositive().describedAs(r -> "Radius = " + r)
             , integers().allPositive().describedAs(h -> "Height = " + h))
      .check((i,j) -> ... );
  }
</code></pre>

If we would rather work with Cylinder objects, but haven't defined a `toString` method, we can add a description function for the converted type in two ways:

1) We can pass this function to the `asWithPrecursor` function:

<pre><code>  @Test
  public void someTestInvolvingCylinders() {
      qt()
      .forAll(integers().allPositive().describedAs(r -> "Radius = " + r)
             , integers().allPositive().describedAs(h -> "Height = " + h))
      .asWithPrecursor((r,h) -> new Cylinder(r,h)
                       , cylinder -> "Cylinder r =" + cylinder.radius() + " h =" + cylinder.height())        
      .check((i,j,k) -> whatever);
  }
</pre></code>

2) We can use the describedAs method:

<pre><code>  @Test
  public void someTestInvolvingCylinders() {
      qt()
      .forAll(integers().allPositive().describedAs(r -> "Radius = " + r)
             , integers().allPositive().describesAs(h -> "Height = " + h))
      .as( (r,h) -> new Cylinder(r,h))
      .describedAs(cylinder -> "Cylinder r =" + cylinder.radius() + " h =" + cylinder.height())
      .check(l -> whatever);
  }
</code></pre>
