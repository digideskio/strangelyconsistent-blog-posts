---
title: The tests Rakudo doesn't run
author: Carl Mäsak
created: 2010-05-09T17:03:00+02:00
---
Just a short note.

After an [enlightening discussion with mberends](http://irclog.perlgeek.de/perl6/2010-05-05#i_2295885) a couple of days ago, I became curious about how many spectests Rakudo *doesn't* run.

So I wrote a short script which takes a list of all spectest `.t` files, a list of all the files mentioned in Rakudo's `t/spectest.data` (including the commented-out ones), and did hash subtraction on them.

By the way, a common Perl 5 idiom in this situation is difficult to do in Rakudo, because some blocks are still erroneously parsed as hashes:

    $ perl6
    > my @array; my %hash = map { $_ => 1 }, @array;
    No candidates found to invoke
    > say { $_ => 1 }.WHAT
    Hash()


Working around this, I arrived at the number 185. That's out of a total of 722. Here's the [whole list](http://gist.github.com/395182).

Not too surprisingly, upon showing this list to #perl6, [I was quickly informed](http://irclog.perlgeek.de/perl6/2010-05-09#i_2311203) (by the ever-knowledgeable moritz++) that there's already [a Rakudo tool script](http://github.com/rakudo/rakudo/blob/master/tools/update_passing_test_data.pl) which processes exactly this list of tests, and prints the much more useful subset of files with at least one test passing, thus being eligible for inclusion into `t/spectest.data`.

I hope to be able to explore the spectest suite further, in my copious spare time. My long-term goal is to create alluring SVG graphs over the tests currently passing in Rakudo master, Rakudo alpha, and Pugs.


