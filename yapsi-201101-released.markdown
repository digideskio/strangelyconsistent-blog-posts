---
title: Yapsi 2011.01 Released!
author: Carl Mäsak
created: 2011-01-02T00:50:00+01:00
---
It is with inordinate pleasure that I announce on behalf of the Yapsi
development team the January 2011 release of Yapsi, a Perl 6 compiler
written in Perl 6. Ta-daa!

You can download it [from Github](http://github.com/downloads/masak/yapsi/yapsi-2011.01.tar.gz).

Yapsi is implemented in Perl 6. It thus requires a Perl 6 implementation to
build and run. This release of Yapsi has been confirmed to work on all
releases of Rakudo Star to date.

Yapsi is an "official and complete" implementation of Perl 6. It is official
by virtue of sheer chuztpah, and complete by virtue of utter disconnectedness
from reality.

This month's release adds a roadmap! You should [check it out](http://github.com/masak/yapsi/blob/master/doc/ROADMAP).

The roadmap will be updated either (a) irregularly or (b) whenever reality
imposes itself in such a way as to make updates a necessity or (c) both.

For a complete list of changes, see [doc/ChangeLog](http://github.com/masak/yapsi/blob/master/doc/ChangeLog).

Yapsi consists of a compiler and a runtime. The compiler and runtime are
two separate components communicating through a protocol called SIC. Actually,
it's more of a standard than a protocol. Hm, it's really more of a syntax.
An instruction format, that's what it is. Anyway, the compiler sends a bit
of SIC to the runtime, and the runtime follows the instructions therein.

With each new release of Yapsi, the old SIC format is declared obsolete and
completely unworkable. If you're wondering why you should upgrade your
Yapsi implementation to this month's version, this is the reason: your old
SIC files won't work with this month's release, due to this deprecation
policy. That's right, you should upgrade because of our deprecation policy!
We also suggest that you choose to upgrade because of fear, uncertainty, and
doubt. (Our PR department suggested that we create advertisements with
dinosaurs in an office setting, but they were overruled by our Sanity And
Common Sense department, for better or worse.)

An overarching goal for making a Perl 6 compiler-and-runtime is to use it as
a server for various other projects, which hook in at different steps:

* A time-traveling debugger (tardis), which hooks into the runtime.
  Already underway, see <http://github.com/masak/tardis>
* A coverage tool (lid), which will also hook into the runtime.
* A syntax checker (sigmund), which will use output from the parser.

Another overarching goal is to optimize for fun while learning about parsers,
compilers, and runtimes. \o/

Have the appropriate amount of fun!
