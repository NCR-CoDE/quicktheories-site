---
layout: page
title: Further Features
permalink: quickstart/advanced/
---
<h3>Subjects</h3>

It is likely that you will want to construct instances of your own types. You could do this within each check, but this would result in a lot of code duplication.

Instead you can define a conversion function. This can be done inline, or placed somewhere convenient for reuse.

<pre><code> @Test
  public void someTheoryOrOther(){
    qt()
    .forAll(integers().allPositive()
          , integers().allPositive())
    .as((width,height) -> new Widget(width,height)) // <-- convert to our own type here
    .check( widget -> widget.isValid());
  }
</code></pre>

This works well for simple cases, but there are two problems.

1. We cannot refer to the original width and height integers in our theory. So we couldn't (for example) check that the widget had the expected size.
2. If our widget doesn't define a toString method it is hard to know what the falsifying values were

Both of these problems are solved by the [asWithPrecursors](/examples/precursors-examples) method.

Defining the values that make up the valid domain for your objects might not be straightforward and could result in a lot of repeated code between theories. This could be tackled by extracting methods to describe the values that make up the domain and the construction of custom types. Alternatively, we could reuse Subjects.

Subjects are very easy to create, in fact we've been implicitly creating subjects in all of our examples so far.

To reuse them, all we need to do is extract a method with the repeated code:

<pre><code>
  @Test
  public void theory1() {
    forAllCylinders()
    .check(cylinder -> xxx);
  }

  @Test
  public void theory2() {
    forAllCylinders()
    .check(cylinder -> yyy);
  }

  private Subject1 &lt;Cylinder&gt; forAllCylinders() {
    return qt()
    .forAll(radii(), heights())
    .as((radius,height) -> new Cylinder(radius,height));
  }

  private Source&lt;Integer&gt; heights() {
    return integers().from(79).upToAndIncluding(1004856);
  }

  private Source&lt;Integer&gt; radii() {
    return integers().allPositive();
  }
  </code></pre>

Subjects are similar to Sources except that they cannot be combined together to create new theories in the way that a Source can be. For example, we could not reuse our Cylinder subject in a theory about boxes that contain cylinders, we would need to create our boxes of cylinders out of Sources.

<br/>
<h3>Creating new Generators and Sources</h3>

If you want to create theories that combine one or more custom types you may need to create custom Sources that can be combined via the DSL to create new Subjects.

A Source is the combination of a Generator and a Shrinker. 

As Generators are just simple functions, creating new ones is easy:
<pre><code>(prng,step) -> new Point(prng.nextInt(10000), prng.nextInt(10000))
</code></pre>
The second parameter (labelled "step") provides a count of how many examples have been created - for most Generators it can be ignored.

Although raw Generators are easy enough to create, it's usually easier to combine the existing ones:
<pre><code>private Generator&ltCylinder&gt cylinders() {
    return integers().from(0).upTo(10000)
        .combine(integers().allPositive(), (radius,height) -> new Cylinder(radius,height));
  }
</code></pre>

A Generator can be converted into a Source easily, using the static method from the Source class `of`. However, Sources created in this way will never support shrinking. To enable shrinking we need to supply a [custom shrinker](/examples/shrinker-examples) - which is a function from one value of a type to a stream of smaller values of that type.

<br/>
<h3>Modifying the falsification output</h3>

Values produces by the Sources DSL should provide clear falsification messages.

If you are working with your own Sources, or would like to modify the defaults, you can supply your own function to be used when describing the falsifying values.

Custom description functions will be retained when converting to a type with precursors. A description function for the converted type can be optionally passed to the `asWithPrecursor` function. A description function can also be provided for a type converted without precursors. Examples can be found [here](/examples/falsification-examples).












