---
title: Yapsi 2010.07 Released!
author: Carl Mäsak
created: 2010-07-01T23:48:00+02:00
---
It is with an unwarranted sense of contentment that I want to announce, onbehalf of the Yapsi development team, the July 2010 release of Yapsi, a Perl 6 compiler written in Perl 6.

You can get it [here](http://github.com/downloads/masak/yapsi/yapsi-2010.07.tar.gz) — try it, it's fresh!

Yapsi is implemented in Perl 6. It thus requires a Perl 6 implementation to build and run. We recommend the 'alpha' branch of Rakudo for this purpose. In practice, this means downloading Rakudo Minneapolis (#25) from January 2010.

Yapsi is an "official and complete" implementation of Perl 6. The fact that it's complete follows from a simple mis-applied proof technique: adding to something complete, one certainly doesn't make it less complete. We are making plenty of additions to Yapsi. Hence, Yapsi is complete; QED. It's official because there's no official definition of "official".

Here's how to get Yapsi up and running, once you have it:

- Make sure you have a built Rakudo alpha in your $PATH as 'alpha'.
- (Optionally) run 'make' for a load-time speedup.

This month's release introduces 'if'/'else' statements and 'while' loops. Having Yapsi run all day on your computer just got a little easier. You're welcome.

Quite a lot of features are now within reach of people who are interested in hacking on Yapsi. See the [doc/LOLHALP](http://github.com/masak/yapsi/blob/master/doc/LOLHALP) file for a list of 'em.

Yapsi consists of a compiler and a runtime. The program is compiled down into an instruction code that is "closer to the machine", for some imaginary value of "machine". Compiling down to an instruction code is not strictly necessary; if we wanted, we could compile down to tiny pecan pies. (But that would be NUTS! What we do instead is just plain SIC.) The instruction set changes quite a bit between releases; if you rely on SIC being stable between releases, you are completely bananas.

An overarching goal for making a Perl 6 compiler-and-runtime is to use it as a server for various other projects, which will hook in at different steps:

- A time-traveling debugger (tardis), which hooks into the runtime.
- A coverage tool (lid), which will also hook into the runtime.
- A syntax checker (sigmund), which will use output from the parser.

Another overarching goal is to optimize for fun while learning about parsers, compilers, and runtimes. We wish you the appropriate amount of fun!


