---
title: t3: Addition chains
author: Carl Mäsak
created: 2012-03-14T22:24:00+01:00
---
<div class="quote"><code>&lt;arnsholt&gt; Heh. NP-complete problems in a
competition. That's just mean ^_^</code></div>

Ok, we're in the midst of reviewing [Perl 6 Coding Contest
2011](http://strangelyconsistent.org/blog/the-2011-perl-6-coding-contest)
code submissions, and the turn has come to the third task: addition chains.

    For a positive integer N, an addition chain for N is a sequence
    starting with 1, each subsequent element being the sum of two
    earlier elements (possibly the sum of the same element twice),
    and ending with N.
    
    For example for N = 9, this is a possible addition chain:
    
        (1, 2, 4, 5, 8, 9)
    
    because 2 = 1 + 1, 4 = 2 + 2, 5 = 1 + 4, etc.
    
    But a minimal solution would be:
    
        (1, 2, 3, 6, 9)
    
    Write a program that reads numbers N from standard input, one per line,
    and outputs a minimal addition chain like the one above.
    
    Sometimes there will be several possible solutions of minimal length.
    That's fine; just pick one of them.

Addition chains are interesting from a computing standpoint, because of a
multiplication technique called [addition-chain
exponentiation](http://en.wikipedia.org/wiki/Addition-chain_exponentiation),
by which you can use an addition chain for a certain `N` to do a minimum
number of multiplications; the addition chain implicitly encodes a sequence of
multiplications to perform. So there's a genuine interest in finding shortest
addition chains.

This is a hard problem. Finding addition chains is easy, but finding a
*minimal* addition chain is not. Depsite arnsholt's quote above, it hasn't
been *proven* NP-complete. Slightly more general problems have, but not this
exact one. We know it's tricky, though.

Wikipedia has this to say about [the
problem](http://en.wikipedia.org/wiki/Addition_chain#Methods_for_computing_addition_chains):
"There is no known algorithm which can calculate a minimal addition chain for
a given number with any guarantees of reasonable timing or small memory
usage." That's what we're looking for in this contest: problems that are easy
to state, and that look quite straightforward, but that have hidden depth.

Someone may look at the problem and think "aha! dynamic programming!" &mdash;
but, alas, as Wikipedia patiently explains:

<div class='quote'><p>Note that the problem of finding the shortest addition
chain cannot be solved by <a href='http://en.wikipedia.org/wiki/Dynamic_programming'>dynamic
programming</a>, because it
does not satisfy the assumption of <a href='http://en.wikipedia.org/wiki/Optimal_substructure'>optimal
substructure</a>. That is, it
is not sufficient to decompose the power into smaller powers, each of which is
computed minimally, since the addition chains for the smaller powers may be
related (to share computations). For example, in the shortest addition chain
for <i>a<sup>15</sup></i> [...] the subproblem for <i>a<sup>6</sup></i> must
be computed as <i>(a<sup>3</sup>)<sup>2</sup></i> since <i>a<sup>3</sup></i>
is re-used (as opposed to, say, <i>a<sup>6</sup> =
a<sup>2</sup>(a<sup>2</sup>)<sup>2</sup></i>, which also requires three
multiplies).</p></div>

This is probably why the problem looks approachable, because it sort of feels
like a dynamic-programming problem. But it ain't.

People sent in solutions. [Go check them
out](http://strangelyconsistent.org/p6cc2011/).

I was a bit concerned that people might find [Knuth's
solution](http://www-cs-faculty.stanford.edu/~uno/programs/achain0.w)
and just transcribe it into Perl 6. (Which would've been OK, if a bit boring
if everyone did it.) But no-one did that; instead, people started from
well-known algorithms, either
[breadth-first](http://en.wikipedia.org/wiki/Breadth-first_search) or
[depth-first](http://en.wikipedia.org/wiki/Depth-first_search) search.

Perhaps the most remarkable things that can be recounted about the solutions
are the cases where they deviate from correctness in various ways. One solution
produced the right results for the first 76 chain lengths, but with `N = 77`,
it went awry due to internal optimizations which turned out to be less than
innocent. (The first rule of optimization? "Make sure you don't get caught.")

Then there were two submitted algorithms that generated Brauer chains. "What's
a Brauer chain?", I hear you asking. Hold on tight and I'll tell you. A Brauer
chain is an addition chain where each new element is formed as the sum of the
*previous* element and some element (possibly the same). Thus, of the two
examples from the description,

    (1, 2, 4, 5, 8, 9)
    
and
    
    (1, 2, 3, 6, 9)

The latter is a Brauer chain, but the former isn't, because you can't get 8 by
summing 5 and some element in the chain.

The task is to generate minimal addition chains. If some algorithm looks only
among the Brauer chains, will it ever omit some shorter chain from its search?
The answer, it turns out, highlights exactly why I like mathematics.

A Brauer-based algorithm will fail the first time at `N = 12509`. (See [this
reference](http://books.google.se/books?id=1AP2CEGxTkgC&pg=PA169&lpg=PA169&dq=brauer+chain+12509&source=bl&ots=TjkugXGNnE&sig=wsVTOZ0OVyFruRFnpqgInngdUaw&hl=en&sa=X&ei=XrhgT_HbBqjj4QSC0NXTDg&ved=0CB4Q6AEwAA#v=onepage&q=brauer%20chain%2012509&f=false),
provided by hakank++).

Now, you might of course argue that failing at `N = 77` is more wrong than
failing at `N = 12509`.

> Sheldon: *More* wrong? "Wrong" is an absolute state and not subject to gradation.
> 
> Stuart: Of course it is! It's a little wrong to say a tomato is a vegetable. It's very wrong to say it's a suspension bridge.

More precisely, there are infinitely many `N` for which no Brauer chain is
minimal. 12509 just happens to be the smallest one.

This task, understandably, is a tricky one to judge. We've tried to go easy on
the contestants (and non-contestants) in the reviews. After all, the problem
*is* hard.

Now, who wants to translate Knuth's solution to Perl 6? 哈哈
