---
title: Yapsi 2011.03 Released!
author: Carl Mäsak
created: 2011-03-06T22:35:00+01:00
---
Dear members of the committee,

I hereby present for your scrutiny the fruits of a month's intense labor.
I call it "Yapsi", and label it the March 2011 release.

You can peruse it [here](http://github.com/downloads/masak/yapsi/yapsi-2011.03.tar.gz).

You will recall my earlier submissions on the same topic. To my great
disappointment, they were rejected by the esteemed fellows of the
committee, on the grounds that they didn't advance the state of the art
of Computer Science. Though immensely saddened by this verdict, I am
compelled, in retrospect, to agree. The previous submissions were
all "official and complete", though hardly ground-breaking.

However, this month I bring before you an invention that I believe
has the potential to change how we think about computing machines and
the entering of program code to same. Let me take a few moments to
explain my new device to you.

In essence, what I have created is a tool for abstracting a sequence of
program instructions into self-contained "routines", each of which is
identified by a name. I know this may seem like an odd idea to some of
you; why not (you may ask) list the program instructions from start to
finish, in one single chain of thought? Though on the surface the new
routines may not seem to add anything of value, I submit to you that
they nevertheless have distinct advantages. I will enumerate some of
these advantages for you, but let me first instruct you on how to employ
a routine in practice.

A computer program may have zero, one, or even several routines. The names
of the routines must be unique &mdash; exactly why this is will become clear in a
few moments. The flow of program instruction evaluation does not
automatically "visit" the routines; they are standalone and apart from
the main flow of the program instructions. Instead, flow is moderated
through a mechanism called "invocation", which I will now describe.

(Pardon? Oh, yes please. Milk, two lumps.)

At any point in the normal instruction flow, one may then refer to one of
the routines by name, and that routine will be executed in the place of
that invocation. I'm sure you all see the immediate benefit of such a
scheme; instead of having the same sequence of program statements be
repeated in different parts of a vast program, one may now instead simply
repeat the same (relatively short) routine identifier. The actual code,
which is invoked from different places, is then written down only once,
inside the routine. This is in accordance with a general principle I've
evolved, that of "Not Repeating Oneself".

However, there is also a secondary psychological benefit to this
scheme: if the name of the routine is well-chosen, it may act as a
mnemonic and make the computer program easier to read and understand.
(Thus, for example, a routine to perform multiplication might be named
"perform_multiplication".) The abstraction, one might say, works not only
on the level of the computer, but also on the level of the human reader.
It is yet unclear to me exactly how important a role this secondary
effect will play, but I suspect it will be quite noticeable.

After the routine has finished running, the instruction evaluation
proceeds from the point after the invocation. Thus, one might say, the
program behaves as if the routine had actually been positioned in the
place of the invocation. This is the nature of my abstraction.

A few points of clarification are in order:

* It is necessary for some amount of computer memory to be put aside
  for the purpose of keeping track of the position of the invocation, so
  that the instruction evaluation can proceed from there after the
  completion of the routine.

* In fact, the scheme that allows for routines to be called from the main
  program may easily be extended to allow for routines to be called from
  other routines. This would require a push-down data structure &mdash; something
  that I've tentatively chosen to call a "stack" because of the way you
  add and remove things from the top of it, like with a stack of plates
  &mdash; to keep track of all the invocations that have been made but not
  yet completed at a given point in the computer program.

* It is even a possibility for a routine to invoke *itself*, or for two
  or more routines to contain invocations of each other. While this may
  seem like an especially perverted idea, early investigations reveal
  that this phenomenon (which I have chosen to call "invocation
  reccurrence") might actually have real-world uses. I propose we leave
  the ability in for now rather than dismiss it outright.

* There is also no *a priori* restriction on a routine being declared
  inside another routine. This notion, however, truly *is* perverse and
  has no possible merit whatsoever.

* I have an additional idea about equipping the invocation with
  "arguments", values which might then be passed into the routine. This
  would further increase the range and utility of routines, by making
  them into a kind of "templates" that can then be filled in for each
  particular invocation. The idea appeals to me a great deal, but I
  haven't had time to try it out in practice. I am currently wrestling
  with a theoretical framework for this based on something called
  "the bondage of signatures".

In showing these ideas to a few select people, I have come to understand
that there is some concern as to the speed impact of maintaining the
above-mentioned "stack". While I concede that there is indeed an
unavoidable cost involved at the boundaries of each routine, I believe
that it can be made very small, such that only the most speed-stipulating
computer programmers will care for the difference. In the long run,
I predict that the benefits of the abstraction mechanism itself will dwarf
concerns about performance.

I believe that routines in computer programs are here to stay. In a
few decades, we might see them as something commonplace, almost mundane.

In the present release, I also wish to highlight the excellent work of
my associate, Tadeusz Sośnierz (tadzik++). He has provided Yapsi with a
feature called a "REPL", which I also believe will be of importance.

For a complete set of changes to this release, please refer to
doc/ChangeLog.

In fact, if you will allow me to perform an "instantaneous
demonstration"... you will? Oh good.

    $ bin/yapsi
    >>> sub foo { say 42 }; foo; foo
    42
    42
    >>> my $i = 10; sub foo { say $i; if --$i { foo } }; foo
    10
    9
    8
    7
    6
    5
    4
    3
    2
    1

With that, I bid the esteemed colleagues of the committe a good evening,
with what I believe to be fitting farewell:

Have the appropriate amount of fun! \o/
