---
title: November 12, 2008 &#8212; blowing stuff up
author: Carl Mäsak
created: 2008-11-13T00:29:00+01:00
---
38 years ago today, the [Oregon Highway Division](http://en.wikipedia.org/wiki/Oregon_Department_of_Transportation) tried to get rid of a rotting beached sperm whale by blowing it up. This event has become known as the [exploding whale](http://en.wikipedia.org/wiki/Exploding_whale) incident. [Wikipedia](http://en.wikipedia.org/wiki/Exploding_whale) has the story:

<blockquote><div><p>The explosion caused large pieces of blubber to land some distance away from the beach, some of which caused heavy damage to a nearby car. The explosion disintegrated only some of the whale, most of which remained on the beach for the Oregon Highway Division workers to clear away.</p></div></blockquote>

There's a [video](http://youtube.com/watch?v=PZhn28_Z9wc).

<blockquote><div><p>For several years, the story of the exploding whale was commonly disbelieved and thought to be an urban legend. However, it was brought to widespread public attention by popular writer Dave Barry in his Miami Herald column of May 20, 1990, when he reported that he had footage of the event. Barry wrote, "Here at the Institute we watch it often, especially at parties."</p></div></blockquote>

<p class='separator'>&#10086;</p>

In an astounding act of symmetry, Rakudo blows up on me today, 38 years after the whale exploded and landed on hundreds of innocent people. And I didn't even use explosives.

What I was going to write about today was a workaround that I figured out for the bug I encountered yesterday. (I seem to get some secret satisfaction from writing 10-line workarounds for 1-line things.) Well, guess what? The *workaround* uncovered another, even nastier bug.

But all's well that ends well. This bug is now [part of the spectest suite](http://svn.pugscode.org/pugs/t/spec/integration/substr-after-match-in-gather-in-for.t), and pmichaud++ is working on tracking down the source of the problem.

Meanwhile, I'll see if I can find a workaround to the workaround. I have a couple of ideas.


