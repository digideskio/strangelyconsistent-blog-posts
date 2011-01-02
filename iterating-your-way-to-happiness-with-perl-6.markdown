---
title: Iterating your way to happiness with Perl 6
author: Carl Mäsak
created: 2010-07-11T23:55:00+02:00
---
I thought I'd have an easy time today, just regurgitating what [S04](http://perlcabal.org/syn/S04.html) says about "Loop statements". Perl 5 already got this part pretty right already. Actually, even C got it pretty right. So what new does Perl 6 have to offer? That's what this post is about.

So, nothing much has changed about the `while` and `until` loops that we know and love.

    while EXPR { ... }
    until EXPR { ... }
    

Then there's the kind of loop when you want to test the condition *after* the block has run, rather than before. In Perl 5, that looks like this:

    do { ... } while EXPR;
    do { ... } until EXPR;
    

This construct tends to cause fresh Perl programmers a lot of grief, since `do` isn't really a loop construct. There's some wording about this in [perldoc perlsyn](http://perldoc.perl.org/perlsyn.html):

<div class='quote'><p>Note also that the loop control statements described later will NOT work in this construct, because modifiers don&#8217;t take loop labels.  Sorry.</p></div>

Perl 6 solves this by

- recognizing and disallowing `while` and `until` after `do` blocks, and
- introducing the `repeat` block.

So now you write it like this instead:

    repeat { ... } while EXPR;
    repeat { ... } until EXPR;
    

And you get two bonus features from this: first, since the `while` or `until` is mandatory, you can put it on its own line. Generally, closing line-ending curlies act like they have implicit semicolons after them in Perl 6, but here the parser is smart enough to expect a `while` or `until`, so it doesn't put one in.

Second, you're allowed to put the condition up front if you want:

    repeat while EXPR { ... }
    repeat until EXPR { ... }
    

Even though the condition is *before* the loop here, it'll still not be run until *after* each iteration, because it's a `repeat` loop, and they work like that.

Then there's the loop construct that loops forever, aptly named `loop`:

    loop { ... }


In C, we'd have that as `for (;;) { ... }`. And, symmetrically, you can also write it like this in Perl 6:

    loop (;;) { ... }


Or, more generally, you can do any C-style for loop, if you just spell it `loop`:

    loop (EXPR; EXPR; EXPR) { ... }


And what was known alternately in Perl 5 as `for` and `foreach` becomes just `for` in Perl 6.

The syntax for `for` in Perl 6 is what you'd expect:

    for EXPR { ... }


But it packs a lot more power underneath. Or rather, the whole *language* is geared towards packing `for` with a lot more power. Some examples:

- If you want to name the item you're currently looping over, just prefix the block with `-> $a`. (If you don't, you'll find the item in `$_`, as usual.)
- If you want to loop two items at a time, prefix the block with `-> $a, $b`.
- Neither of the above are special syntaxes belonging to the `for` construct as such; rather, the `->` arrows belong to the block, making it, in the prevailing terminology, a "pointy block". In fact, *all* of the loop constructs I've brought up (except the C-style loop) can be given pointy blocks; the expression will then be bound to the variable and usable from within the block. You can do it with `if` statements, too! It's quite useful.
- If you want to loop over two lists simultaneously, you can use the `infix:<Z>` operator to interleave the lists. Loop one item at a time, and you'll get alternating elements from the two lists. Loop two at a time, and `$a` (or whatever) will always be from the first list, and `$b` from the second. Gone are the days when you had to do manual trickery with indexes and stuff because the `for` loop wasn't powerful enough.
- You can even eliminate nested loops sometimes with the `infix:<X>` operator, which does a Cartesian product (known in SQL circles as a "cross join") of two lists. So if you planned to do `for @a { for @b { ... } }` anyway, you might as well do `for @a X @b { ... }` and save yourself one level of indentation.
- All of the above is lazy, so with a sensible Perl 6 implementation, there's no huge memory waste from building up all these big lists; it's all generated on-the-fly. (This, incidentally, means that we also read files with `for` loops in Perl 6, rather than with a magical `while` construct as in Perl 5.)

That's it for today. I forgot to mention the looping construct that only loops over *one* item... but you can look that one up yourself. Oh, and Perl 5.10 also has it.

Actually, I got to thinking about all this, since I figured out the other day how to do the `FIRST` and `LAST` phasers in Yapsi. But this blog post felt like a natural precursor to the one I wanted to write. Hopefully soon. 哈哈


