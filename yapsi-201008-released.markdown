---
title: Yapsi 2010.08 Released!
author: Carl Mäsak
created: 2010-08-02T01:50:00+02:00
---
It is with vertiginous modesty that I want to announce, on behalf of the Yapsidevelopment team, the August 2010 release of Yapsi, a Perl 6 compiler written in Perl 6. (Am I using, too many, commas?)

You can get it [here](http://github.com/downloads/masak/yapsi/yapsi-2010.08.tar.gz) — as a bonus, if you download within 24 hours, the bits in your download will be hand-painted and signed by nine indefatigable mice from northern Belarus.

Yapsi is implemented in Perl 6. It thus requires a Perl 6 implementation to build and run. Both Rakudo Star and the latest monthly release ("Atlanta") seem to be fit to the task.

Yapsi is an "official and complete" implementation of Perl 6. It's official because we stole the "Official Perl 6" rubber stamp and applied it liberally. Unfortunately, we also used all the special ink -- please don't tell any of the other implementors that. It's complete because we implemented all the parts of the synopses that weren't eaten by the team's pet dugong.

This month's release consists of a total refactor of both the runtime and the compiler. The Yapsi.pm file is about 100 lines shorter, with the same functionality. Lexical variables are now handled more correctly. For a complete list of changes, see [doc/ChangeLog](http://github.com/masak/yapsi/blob/master/doc/ChangeLog).

Quite a lot of features are within reach of people who are interested in hacking on Yapsi. See the [doc/LOLHALP](http://github.com/masak/yapsi/blob/master/doc/LOLHALP) file for a list of 'em.

Yapsi consists of a compiler and a runtime. The compiler generates a so-called instruction code, which is a code of instructions, for the runtime to run. This is fairly common, and nothing to be agitated about. The instruction code is called SIC, which is probably slightly less common. SIC is extended, re-thought and eaten by the team's pet dugong on a regular basis, so don't expect SIC from one release to be runnable on another.

An overarching goal for making a Perl 6 compiler-and-runtime is to use it as a server for various other projects, which will hook in at different steps:

-  A time-traveling debugger (tardis), which hooks into the runtime.
-  A coverage tool (lid), which will also hook into the runtime.
-  A syntax checker (sigmund), which will use output from the parser.

Another overarching goal is to optimize for fun while learning about parsers, compilers, and runtimes.

Gotta go feed S16 to the team's pet dugong. We wish you the appropriate amount of fun!


