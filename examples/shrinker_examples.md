---
layout: page
permalink: /examples/shrinker-examples/
---

<h2>Creating Custom Shrinkers</h2>


A shrinker is a function from one value of a type to a stream of smaller values of that type. In the example below, cylinders will be shrunk deterministically by decreasing both the radius and the height by 1.

This isn't a particularly good shrink function as it does not explore a large amount of the space of possible cylinders. It may also create cylinders with negative radii and heights.

If a theory explicitly assumes that heights and radii are positive these values will be filtered out, but if not they will be reported and might obscure the real falsifying values.

<pre><code>
  private Source&lt;Cylinder&gt; anyCylinder() {
    return Values.of(cylinders()).withShrinker(shrinkCylinder());
  }
  
  private Shrink&lt;Cylinder&gt; shrinkCylinder() {
    return (original,context) ->  IntStream.range(1, context.remainingCycles())
                                           .mapToObj(i -> new Cylinder(original.radius - i, original.height - i));
  }
  
  private Generator&lt;Cylinder&gt; cylinders() {
    return integers().from(0).upTo(10000)
        .combine(integers().allPositive(), (radius,height) -> new Cylinder(radius,height));
  }
</code></pre>



Be careful when creating custom shrinkers.
