---
title: Yapsi 2011.02 Released!
author: Carl Mäsak
created: 2011-02-05T19:06:00+01:00
---
It is with extreme...

...hm...

I would like to digress a bit and tell a story. A week ago, I put on my
running shoes for the first time in my new hometown. I had already seen
out a suitable route on Google Maps, committing the more important street
names to memory. It was a cloud-free day, and the sun stood as high in
the sky as it ever will in January in Sweden. Out I went.

Before half an hour had passed, I was completely lost. I didn't see any
of the streets I had memorized, nor did I run into any of the big roads
I knew I would run into if I ran too far.

Gradually, I found myself out on the countryside. That wasn't part of
the plan at all. Fields stretched out in all directions. Airplanes
criscrossed the sky, their exhaust trails leaving nice patterns behind,
reminiscent of some CS books about graph theory.

I started down the country road in the direction back to town, only to
have the road slowly curve back in the other direction. It was like one
of those text adventure games where you exit one location to the north
but end up entering the next location from the northwest! Not conducive
to getting somewhere at all.

The the sun went down. At this point, I had been running for over an
hour, and was wondering whether I would sleep in a bed that night. I
was getting cold and a little bit miserable. The battery of my mp3
player died.

Things got gradually better, though. I found a bigger road, and a sign
pointed back to my city, saying it was only five kilometers away. My
speed had dropped a bit due to hopelessness, but now it picked up again.
I passed a suburb, a mall, a school, a number of unfamiliar blocks, some
familiar blocks, and then I was home again. Exhausted. But grateful.

The take-home message is, I hope, crystal clear. A refactor, just like
a run, is a process whereby you hope to end up in the same place as you
started. Oh, and sunsets can be very pretty.

Anyway.

It is with an exhausted but satisfied feeling that I announce on behalf
of the Yapsi development team the February 2011 release of Yapsi, a Perl
6 compiler written in Perl 6.

You can download it [here](http://github.com/downloads/masak/yapsi/yapsi-2011.02.tar.gz).

Yapsi is implemented in Perl 6. It thus requires a Perl 6 implementation to
build and run. This release of Yapsi has been confirmed to work on all
releases of Rakudo Star to date. The test files only work flawlessly on Rakudo
Star 2011.01, though, due to s/done_testing/done/.

Yapsi is an "official and complete" implementation of Perl 6. This has been
confirmed, documented, jokingly referred to, and lamented in a number of
places online and offline.

This month's release is a bit late, for which I'm either terribly sorry,
or hereby announce that from as of this release, Yapsi will release on the
first Saturday of every month. Haven't decided yet.

This month's release could be called a "developer release", but let's not
go that far. Suffice it to say that Yapsi behaves the same as last month,
but the internals are now much more hackable than last month, so if you've
secretly been thinking of becoming a contributor, now's an excellent time
to pick some low-hanging fruit. For example, the daughter project
'sigmund', mentioned at the bottom of every Yapsi release announcement,
is now feasible; it wasn't really before.

Also, Yapsi has the cutest AST output of all the Perl 6 implementations:

    $ bin/yapsi --target=FUTURE -e 'my $a; { $a = 42 }; say $a'
    Block -- B0 [$a]
      Var -- $a
      Block -- B2
        Assign
          Var -- $a
          Val -- 42
      Call -- &say
        Var -- $a

It's so cute, it almost looks like Ruby!

Structurally, FUTURE is a re-thinking of PAST. FUTURE is more streamlined
than PAST, because it gets rid of the Stmts type. FUTURE is also more
versatile than PAST, because it adds 7 new types. In most other respects,
FUTURE is like PAST.

For a complete list of changes, see doc/ChangeLog.

Yapsi consists of a compiler and a runtime. The compiler processes a piece
of source code, turns it into an annotated tree structure known as FUTURE,
and then serializes this tree into a sort of assembler code for a virtual
machine. (The virtual machine, being virtual, doesn't really exist. Which,
all things considered, is probably a good thing.) The SIC is then...
consumed... by the runtime which does its thing and executes it.

With each new release of Yapsi, the old SIC format is thrown out the door,
and a new one, sometimes very similar, sometimes identical to the old one,
is employed instead. This process is codified for the purpose of keeping
people on edge. FUTURE, however, abides by a sofisticated deprecation
policy, which in short declares that the format never changes, except in
very rare cases when it does.

An overarching goal for making a Perl 6 compiler-and-runtime is to use it as
a server for various other projects, which hook in at different steps:

* A time-traveling debugger (tardis), which hooks into the runtime.
  Already underway, see <http://github.com/masak/tardis>
* A coverage tool (lid), which will also hook into the runtime.
* A syntax checker (sigmund), which will use output from the parser.

Another overarching goal is to optimize for fun while learning about parsers,
compilers, and runtimes.

Have the appropriate amount of fun! \o/
