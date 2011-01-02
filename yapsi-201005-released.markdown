---
title: Yapsi 2010.05 Released!
author: Carl Mäsak
created: 2010-05-01T23:47:00+02:00
---
It is with undue sprightliness that I want to announce, on behalf of the Yapsidevelopment team, the May 2010 release of Yapsi, a Perl 6 compiler written in Perl 6.

You can get it [here](http://github.com/downloads/masak/yapsi/yapsi-2010.05.tar.gz).

Yapsi is implemented in Perl 6. It thus requires a Perl 6 implementation to build and run. We recommend the 'alpha' branch of Rakudo for this purpose.

Yapsi is an 'official and complete' Perl 6 release. It's official because we have little labels with the word 'official' on them that we glue to each download that takes off from github. (That much gluing takes a lot of work, but we think of it as a community service.) It's complete in the sense that all of the things it implements, it does. With gusto.

Instructions for getting Yapsi up and running:

1. Make sure you have a built Rakudo alpha in your $PATH as 'alpha'.
2. Download Yapsi from [here](http://github.com/masak/yapsi/downloads).
3. ...
4. Profit!

The third (optional) step consists of running 'make' in the Yapsi directory. This will precompile Yapsi, making startup times much more bearable.

The big change since last month is that Yapsi now does immediate blocks.

    $ export PERL6LIB=`pwd`/lib
    
    $ ./yapsi -e 'my $a = 42; { my $a = 7; say $a }; say $a'
    7
    42


For other changes, see the doc/ChangeLog file.

This release, as opposed to the last one, contains hidden bugs. We encourage you to help us find them; if not for the fame and glory, than at least for the Schadenfreude.

Yapsi consists of a compiler and a runtime. The program is compiled down into an instruction code, which can be stored and executed weeks, years or even days [*sic*] later. If you want, you can execute the same piece of SIC several times over. All SIC listings start with a versioning line saying e.g. "This is SIC v2010.05". At this point during Yapsi development, we provide no guarantees about backwards compatibility whatsoever. Expect breakage between monthly releases.

An overarching goal for making a Perl 6 compiler-and-runtime is to use it as a server for various other projects, which will hook in at different steps:

- A time-traveling debugger (tardis), which will hook into the runtime.
- A coverage tool (lid), which will also hook into the runtime.
- A syntax checker (sigmund), which will use output from the parser.

Another overarching goal is to optimize for fun while learning about parsers, compilers, and runtimes. We wish you the appropriate amount of fun!


