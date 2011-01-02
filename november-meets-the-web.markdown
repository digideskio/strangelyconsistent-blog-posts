---
title: November meets the Web
author: Carl Mäsak
created: 2008-08-24T17:37:00+02:00
---
Two major things have happened during the week: first off, pmichaud++ implemented *precompiled modules* for Rakudo, i.e. when including a module through the `use` keyword, Rakudo now looks first for `Module.pbc` and `Module.pir` before falling back to the usual `Module.pm`.

We made use of this, precompiling all our modules, even moving the main wiki code into its own module `Wiki.pm` so we could precompile it, too. The resulting speedup in a "view page" request was 17-fold: instead of 23 seconds, it now takes 1.3 seconds!

The second major thing was that we got November running on [feather](http://feather.perl6.nl/), the dedicated development server of the Perl 6 community. We also got our own domain name, [november-wiki.org](http://www.november-wiki.org/), to match.

That's Perl 6 you see running there.

Kudos to (Frederik Schwarzer)++ and moritz++ for submitting patches to the project! And kudos also to jhorwitz++, who is looking into making November and mod_perl6 work together.


