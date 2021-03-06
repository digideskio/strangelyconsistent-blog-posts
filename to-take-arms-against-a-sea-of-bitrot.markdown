---
title: To take arms against a sea of bitrot
author: Carl Mäsak
created: 2010-10-07T00:19:00+02:00
---
My projects are rotting. Fast.

The really big change was the transition to a new Rakudo (known as `ng`) in January, of course. I was one of the people who stuck to the old Rakudo (known, retroactively, as `alpha`) for the longest time, because

* at first, `alpha` simply won in terms of features, and it wasn't feasible to apply workarounds to make code work in `ng`;
* at some point in the middle, it was simply a relief to be coding in `alpha` and not having the carpet move under me for a while;
* finally, at the point where `ng` started to significantly overtake `alpha`, it was the hugeness of the task that daunted.

So far, I've ported over GGE (grammar engine) and Yapsi (Perl 6 implementation). Projects that remain to be ported over include November (wiki engine), Web.pm (web framework), perl6-literate (runnable blog posts), pun (-n and -p flags for Rakudo), and Tardis (time-traveling debugger).

I ported GGE from `alpha` to `ng` on and off over a long period of time. Without the test coverage I have for that project, I probably would have given up. Just to give you an idea of the amount of work it was, the changes to bring GGE from `alpha` to `ng` can be found [here](http://github.com/masak/gge/commit/839bd22dd4745679cb393f1d751df67d1e9560c9), [here](http://github.com/masak/gge/commit/c2a3496ea29f7a050a64b2c86444cd1626133af3), [here](http://github.com/masak/gge/commit/99ee4358bf610aaf05a9bc032c869b5b10ed30dc), [here](http://github.com/masak/gge/commit/18bf009e163ae40b282424612d84e13cc2dc4cb0), [here](http://github.com/masak/gge/commit/5f369ef4a077280231b7e165217d440ca00ab865), [here](http://github.com/masak/gge/commit/29b63609b710e7b4e44274682bf07b1d9fd32efc), [here](http://github.com/masak/gge/commit/e55d0e940b53466d4f792e1f335df7c62f98f033), [here](http://github.com/masak/gge/commit/131525a3523736a651ef0c303a04672526bebf4d), [here](http://github.com/masak/gge/commit/f9583814381b4f5321cc886aa8d8c3bd040b7821), [here](http://github.com/masak/gge/commit/3f4bdd40afa49896988c880547f52e3f37355ace), [here](http://github.com/masak/gge/commit/686e6ed28bd756f77651897323a7236ea1765370), [here](http://github.com/masak/gge/commit/e87e6a6bb6c08f6a799a66a4d0c1b16f9ba46406), [here](http://github.com/masak/gge/commit/d15321558aea55c98daf8a6edfe04a5bae6e323e), [here](http://github.com/masak/gge/commit/beae32c8ab3eeff940995a613bc4344c30b2e090), [here](http://github.com/masak/gge/commit/d8ff454d6f6f1abb4c3e4c0d767f90e8dd8a3c26), [here](http://github.com/masak/gge/commit/a38e8dbb76a500dab321474e3f746bbd209e0b4e), [here](http://github.com/masak/gge/commit/71c611a572bcfcda31a9646e2e6378156aeef833), [here](http://github.com/masak/gge/commit/e263efb4082477a4622e5f6f01aaea20d2d66499), [here](http://github.com/masak/gge/commit/7a77cf0b95256e095cead3e98672bd19075f39a6), [here](http://github.com/masak/gge/commit/712f2a028a5ee3ee9b10a2c0dcb061585033ad04), [here](http://github.com/masak/gge/commit/20751713def6d551064baf19efcb7c64211367c9), [here](http://github.com/masak/gge/commit/fa43d233bc0f3f2d1e97b981553bfb0dddca01d8), [here](http://github.com/masak/gge/commit/466b14fe91b51abf6ff8c06ba4e28ccb5c7ec734), [here](http://github.com/masak/gge/commit/8677a652791bce4cc76d3859f000dc76240686fd), [here](http://github.com/masak/gge/commit/7a46c16f9ea71d235358daf042546912818d607f), and [here](http://github.com/masak/gge/commit/59215db56827ba38b933258d9577ba91ac0335ea). For those who want just the good bits already picked out, I once wrote a [blog post](http://strangelyconsistent.org/blog/gge-now-runs-fine-on-rakudo-master) on the lessons of porting GGE.

I ported Yapsi from `alpha` to `ng` mainly by rewriting the whole of Yapsi over the course of a few days. The change went in as a [single commit](http://github.com/masak/yapsi/commit/62ff73d804541505edd65adb3cced9f9255bfdd2) declaring that "Yapsi now has a new Yapsi". Fitting, since Rakudo now had a new Rakudo.

I've started porting Druid from `alpha` to `ng`. I stopped because I hit some trouble which I had to work around, then hit some more trouble which I had to work around, at which point I hit some serious trouble. And it wasn't that `ng` had bugs or did things wrong either, it was that Druid used aspects of `alpha` (mainly in the grammar engine) that simply didn't exist anymore. I now think that Druid needs a mid-sized redesign; the old design worked under `alpha`, but for `ng` Druid needs something new.

It's not just the `alpha`-to-`ng` transition. Last night while going by car to visit Amsterdam.pm, I picked up a two months old slides-generating script, thinking I might use it to whip up a presentation for the evening. Turned out it had a [regression bug](http://rt.perl.org/rt3/Ticket/Display.html?id=78234) and [another regression bug](http://rt.perl.org/rt3/Ticket/Display.html?id=78232). This is exactly the effect the Jargon File describes as [bit rot](http://www.jargon.net/jargonfile/b/bitrot.html):

<div class="quote">Hypothetical disease the existence of which has been deduced from the observation that unused programs or features will often stop working after sufficient time has passed, even if `nothing has changed'. The theory explains that bits decay as if they were radioactive. As time passes, the contents of a file or the code in a program will become increasingly garbled.</div>

Instead of trying to work around the bugs in the car, I stashed two bug reports and eventually made a talk around the script instead. (And I didn't have to run it.)

Bitrot happens quickly in Rakudo applications. This basically tells me three things:

* I have more Perl 6 code than I'm currently able to maintain.
* As much as I like tests and try to write tests, I have too few of them.
* For some time, I've been dreaming of a nightly tester. It's really getting to be time to write it. With a bit of luck, it would help discover and alleviate bitrot.

It could perhaps be seen as a tad ironic that in wanting to help create a tool chain for Perl 6, I have instead fallen victim to the lack of it. Oh well; live and learn.
