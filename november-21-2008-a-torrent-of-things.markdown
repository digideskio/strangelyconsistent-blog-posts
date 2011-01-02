---
title: November 21, 2008 &#8212; a torrent of things
author: Carl Mäsak
created: 2008-11-22T00:44:00+01:00
---
28 years ago today, the plug was pulled on [Lake Peigneur](http://en.wikipedia.org/wiki/Lake_Peigneur) when Texaco drilled a hole in the roof of a mine by mistake. There's a bit of confusion as to whether this happened on November 20 or November 21, but I wouldn't want you to miss this, so I'm reproducing it today. [Wikipedia](http://en.wikipedia.org/wiki/Lake_Peigneur#Disaster):

<blockquote><div><p>It is difficult to determine exactly what occurred, as all of the evidence was destroyed or washed away in the ensuing maelstrom. The now generally accepted explanation is that a miscalculation by Texaco regarding their location resulted in the drill puncturing the roof of the third level of the mine. This created an opening in the bottom of the lake, similar to removing the drain plug from a bathtub. The lake then drained into the hole, expanding the size of that hole as the soil and salt were washed into the mine by the rushing water, filling the enormous caverns left by the removal of salt over the years. The resultant whirlpool sucked in the drilling platform, eleven barges, many trees and 65 acres (260,000 m&#178;) of the surrounding terrain. Leonce Viator, Jr., a local fisherman, was able to drive his small boat to the shore and tie it up to a tree, and get out, to later watch it and the tree get sucked down. So much water drained into those caverns that the flow of the Delcambre Canal that usually empties the lake into Vermilion Bay was reversed, making the canal a temporary inlet. This backflow created, for a few days, the tallest waterfall ever in the state of Louisiana, at 164 feet (50 m), as the lake refilled with salt water from the Delcambre Canal and Vermilion Bay. The water downflowing into the mine caverns displaced air which erupted as compressed air and then later as 400-foot (120 m) geysers up through the mineshafts.</p></div></blockquote>

Oops.

<p class='separator'>&#10086;</p>

Time has caught up with me today, so I'll cop out and do the descriptive post I thought I'd be doing yesterday before I got pulled into a renegade hacking spree. (I'm not a native English speaker, so I don't know if the phrase "renegade hacking spree" makes sense, even metaphorically. For some reason, the more obstacles I run into with Rakudo and Parrot, the more I feel like a guerilla leader fighting with the weather, the terrain, the weapons and the mission objectives. It's fun.)

When I [last spoke about branches](http://strangelyconsistent.org/blog/lovely-patchers), we had two. Now we have two handfuls. Since each of them has a story, they're a decent topic for a blog post. Here goes.

-  **master**. git gives you this branch for free. For all intents and purposes, this is the November wiki engine at any given moment. All the other branches are experiments of different sorts. In other words, it's like svn's "trunk", except that the name "master" sounds cooler.
-  **new-html-template**. An old branch which blocks on the infamous [#58392](http://rt.perl.org/rt3/Ticket/Display.html?id=58392). Since there are insistent rumors that this bug will get fixed Real Soon Now, I'm re-familiarizing myself with it a bit as we speak. As the name implies, it contains a shiny new HTML::Template module, which uses grammars in an admirable way, instead of regexes in a sloppy way as we currently do it.
-  **tests** was part of a drive to add tests to some parts of November. As far as I know, this branch has filled its purpose and been merged back into master, but it still hangs around on github.
-  **markup** contains a few improvements on our current minimal markup engine (called `Text::Markup::Wiki::Minimal`) made by ihrd++. It adds links parsing, something we've been wanting to make work on the server for a while.
-  **mediawiki-markup** is where I hammer on the new markup engine `Text::Markup::Wiki::MediaWiki`, which will replace `Minimal` as soon as it surpasses it in functionality, which is soon.
-  **janitor-work** was a branch I started in order to convert items in the [docs/JANITORS](http://github.com/viklund/november/tree/master/docs%2FJANITORS) file into actual p5w features. This is actually pretty pleasant work, which can be done with Perl 5 and Perl 6 programmers alike. Patches welcome.
-  **dispatch**. Created by ihrd++ who wanted November to be able to dispatch on nice-looking URLs. He declared success on the mailing list the other day, and I look forward to reviewing what he's done.
-  **asterisk** was also created by ihrd++, experimenting with making (the new) `HTML::Template` use action methods, which pmichaud told us worked in Rakudo. It has currently stalled on something, mainly the total lack of idioms for this kind of code. We all agree it's a cool way of working, but we're not sure how to make it work yet.
-  **plugin** is a new branch by viklund++ in which he beats `Minimal` into recognizing `<plugin>` tags (à la MediaWiki) which will then delegate the parsing to the respective plugin. Seems like interesting work.

It's exciting to realize that five months ago, November was just an idea tossed around in a couple of emails. Three months ago, it was viklund and I demoing a shaky prototype at YAPC::EU. Today, we're working on eight branches in parallel, with four regulars (not counting the three non-human participants) on the IRC channel, 14 people watching us on github, and 19 members on the mailing list, averaging 2.5 emails a day. We're growing.


