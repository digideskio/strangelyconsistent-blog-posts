---
title: Where in the world is the package lexpad?
author: Carl Mäsak
created: 2010-08-22T23:39:00+02:00
---
*(This post isn't very punny. For those of you who need puns to survive, try to figure out why jnthn++ named the IRC logs "the hottest footwear" recently. The answer, as with all good puns, is highly unsatisfying.)*

My quest for a Perl 6 implementation takes me ever deeper into the esoterics of lexpads, runtimes, and a far-more-than-everything-you-needed-to-know mindset. Today some random firings in my brain turned into the following conversation on #perl6.

During the conversation, I proposed two theories, both of which turned out to be wrong. (pmichaud++ shone the necessary light both times.) Being wrong felt less important than getting my mental model fixed.

I first thought of presenting the results of the below conversation as a simple tutorial ("How `our` declarations work. The complete guide."), but now I think that the conversation, minimally edited, manages to be such a tutorial on its own.

Besides, blogging things in their raw and undigested form is a sign of the times. Enjoy!

<pre><code>&lt;masak&gt; I have a question. is there a need for a special "package lexpad" containing 'our'-declared variables, or can the package lexpad simply be equated to the topmost lexpad in the package?

&lt;masak&gt; my suspicion is the latter, but I might be missing something.

&lt;pmichaud&gt; the package lexpad can't be the same as the top most lexical

&lt;pmichaud&gt; <b>module XYZ { my sub abc() { ... } };   # abc should not appear in the package</b> 

&lt;masak&gt; oh!

&lt;masak&gt; right.

&lt;masak&gt; so, separate one, then.

&lt;jnthn&gt; Additionally, lexpads are meant to be static by the time we hit runtime, and you're allowed to shove stuff into the package dynamically. Not quite sure how those two hold together.

&lt;pmichaud&gt; well,  <b>module XYZ { ... }</b>   creates a lexical XYZ entry that holds the package entries

&lt;jnthn&gt; Aha!

&lt;pmichaud&gt; and it's just a hash, really.

&lt;masak&gt; does inserting the package lexpad below the outside lexpad (and above the topmost lexpad) make sense? that way, Yapsi wouldn't need any special opcodes for doing 'our'-variable lookups.

&lt;pmichaud&gt; the package lexpad is an entry in the outside lexpad, yes.

&lt;pmichaud&gt; I'm not sure it encapsulates the nested lexpad, though.

&lt;masak&gt; hm.

&lt;masak&gt; if it doesn't, I don't really see how it's visible from inside the package.

&lt;masak&gt; I've more or less convinced myself that sandwiching it between outer and topmost is what I want to do for Yapsi.

&lt;pmichaud&gt; <b>our &amp;xyz</b>  can make an entry in both the package and in the lexical.

&lt;pmichaud&gt; this is what rakudo does now.

&lt;pmichaud&gt; we have to do similar things for methods already, too.

&lt;masak&gt; sure. it makes entries in both.

&lt;pmichaud&gt; by having entries in both, that's how it's visible inside the package

&lt;masak&gt; hm, indeed.

&lt;masak&gt; no need to have the package lexpad visible from inside.

&lt;pmichaud&gt; anyway, sandwiching might work too.  haven't quite gotten to that point in Rakudo thinking yet.  And it can get a bit tricky with multis.

&lt;masak&gt; no need to sandwich it in, either. it can sit in limbo outside the tree of scopes.

&lt;pmichaud&gt; oh, I know why it perhaps shouldn't (or should) be visible:

&lt;pmichaud&gt; <b>my $x = 'lexical';   module XYZ { say $x;  { our $x = 'package'; } }</b> 

&lt;masak&gt; ...yes?

&lt;pmichaud&gt; I'm pretty sure "say $x" needs to grab the 'lexical' $x, not the one that might be "sandwiched" in a package.

&lt;masak&gt; of course.

&lt;masak&gt; that falls out from ordinary scope nesting and shadowing.

&lt;masak&gt; innermost block binds its lexical to the container in the package lexpad.

&lt;masak&gt; so, that speaks out against sandwiching.

&lt;masak&gt; pmichaud++
</code></pre>

So there you go. There's a separate package scope, and it isn't sandwiched.

*(Answer: The missing link is the "IR clogs" meme from #parrot. I can hear you groaning... I did warn you.)*


