---
layout: page
permalink: /examples/seed-examples/
---
<h2>Setting the Seed:</h2>
<h1><a name = "seed_DSL" style="color:inherit; text-decoration: none !important;">Directly using the DSL:-</a></h1>

The seed can be set using the method `withFixedSeed`. When using the default random number generator, if the provided seed is zero it will automatically be updated to the value one.

<pre><code> qt()
  .withFixedSeed(3456)
  .forAll( . . .)
</code></pre>


