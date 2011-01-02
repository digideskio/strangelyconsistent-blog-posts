---
title: November 8, 2008 &#8212; the joys of discovery
author: Carl Mäsak
created: 2008-11-09T00:30:00+01:00
---
113 years ago today, German physics professor [Wilhelm Conrad Röntgen](http://en.wikipedia.org/wiki/Wilhelm_R%C3%B6ntgen) discovered a new type of [electromagnetic radiation](http://xkcd.com/273/) while experimenting with electricity, playing with a couple of tubes. Since it was a new, hitherto unknown type of rays, he referred to them as "X" (as in "unknown quantity"), and the name stuck. In some countries, the rays are called "Röntgen rays".

[Wikipedia](http://en.wikipedia.org/wiki/X-ray) gives this account of the discovery, which is partly uncertain "because Röntgen had his lab notes burned after his death":

<blockquote><div><p>R&#246;ntgen was investigating cathode rays with a fluorescent screen painted with barium platinocyanide and a Crookes tube which he had wrapped in black cardboard so the visible light from the tube wouldn't interfere. He noticed a faint green glow from the screen, about 1 meter away. The invisible rays coming from the tube to make the screen glow were passing through the cardboard. He found they could also pass through books and papers on his desk. R&#246;ntgen threw himself into investigating these unknown rays systematically. Two months after his initial discovery, he published his paper.</p></div></blockquote>

The discovery got him the first Nobel Prize in Physics in 1901.

<p class='separator'>&#10086;</p>

Just like the discovery of Röntgen, my work today is itself invisible to the human eye. I've been slowly building the test suite for the [mediawiki-markup](http://github.com/viklund/november/commits/mediawiki-markup) tests. Tedious work, but also a bit meditative. The restless brain runs ahead and thinks about how to implement all the features tested for.

Implementing all of MediaWiki's markup will be non-trivial, and in some cases it might be impossible to follow it exactly. Basically, I've been looking at [Help:Wikitext_examples](http://en.wikipedia.org/wiki/Help:Wikitext_examples) and taking what I found to be useful and converting it to tests.

After three test files, I took a break and stuck my head in #parrot to complain about [RT #60304](http://rt.perl.org/rt3/Ticket/Display.html?id=60304) which made it hard to track down spelling errors. bacek++ patched it at once for me. *Ура* for a responsive community!


