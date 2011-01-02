---
title: Yapsi 2010.09 Released!
author: Carl Mäsak
created: 2010-09-02T00:31:00+02:00
---
It is with a peevish exultation of spirit that I announce on behalf ofthe Yapsi development team the September 2010 release of Yapsi -- soon to be a major motion picture -- a Perl 6 compiler written in Perl 6.

You can download it [here](http://github.com/downloads/masak/yapsi/yapsi-2010.09.tar.gz) (or, if you happen to be on an [avian-carrier-based](http://en.wikipedia.org/wiki/IP_over_Avian_Carriers) network, you can "pidgeon" it [here](http://github.com/downloads/masak/yapsi/yapsi-2010.09.tar.gz)).

Yapsi is implemented in Perl 6. It thus requires a Perl 6 implementation to build and run. This release of Yapsi has been confirmed to work on both Rakudo Star 2010.08 and Rakudo Star 2010.07.

Yapsi is an "official and complete" implementation of Perl 6. Yapsi's official status has been publicly confirmed by Patrick Michaud, the Rakudo pumking. The claim about Yapsi being complete... well, it might just be what PR people sometimes refer to as "a slight exaggeration". On the bright side, it's becoming less so with each new release.

This month's release brings you `unless` and `until`, as well as `our`-scoped variables:

    $ yapsi -e 'my $a = 0; unless $a { say 42 }'
    42

    $ yapsi -e 'my $a = 0; until $a { say ++$a }'
    1

    $ yapsi -e 'our $a = 42; { my $a = 5; { say our $a } }'
    42

For a complete list of changes, see [doc/ChangeLog](http://github.com/masak/yapsi/tree/master/doc/ChangeLog).

Quite a lot of features are within reach of people who are interested in hacking on Yapsi. See the [doc/LOLHALP](http://github.com/masak/yapsi/tree/master/doc/LOLHALP) file for a list of 'em. In fact, that's how isBEKaml++ implemented 'unless' this month. (After which he exclaimed "that was easy!" and tackled 'until'.) If you're wondering whether you're qualified to help with the Yapsi project, that probably means you are.

Yapsi consists of a compiler and a runtime. The compiler generates instruction code which the runtime then interprets. In Yapsi, that instruction code (unfortunately) is called SIC[!]. Until further notice, SIC as a format changes with each monthly release for various, mostly good reasons. However, if you write a downstream tool that makes assumptions about the SIC format, someone might change it just out of spite. SIC is explicitly not compatible with later, earlier, or present versions of itself.

An overarching goal for making a Perl 6 compiler-and-runtime is to use it as a server for various other projects, which will hook in at different steps:

-  A time-traveling debugger (tardis), which hooks into the runtime.
-  A coverage tool (lid), which will also hook into the runtime.
-  A syntax checker (sigmund), which will use output from the parser.

Another overarching goal is to optimize for fun while learning about parsers, compilers, and runtimes.

Have the appropriate amount of fun!


