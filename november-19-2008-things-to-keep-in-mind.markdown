---
title: November 19, 2008 &#8212; things to keep in mind
author: Carl Mäsak
created: 2008-11-20T00:13:00+01:00
---
18 years ago today, some pop duo called [Milli Vanilli](http://en.wikipedia.org/wiki/Milli_Vanilli) was stripped of their Grammy Award because their first album, [All or Nothing](http://en.wikipedia.org/wiki/All_or_Nothing_(Milli_Vanilli_album)), advertised them as the singers, when in fact it was not them at all, but some [session musicians](http://en.wikipedia.org/wiki/Session_musician). This all occurred four days after the recording of the voices got jammed during a live performance. [Wikipedia](http://en.wikipedia.org/wiki/Milli_Vanilli#Media_backlash) tells it thus:

<blockquote><div><p>In 1990, during a live performance recorded by MTV at the Lake Compounce theme park in Bristol, Connecticut, the recording of the song "Girl You Know It's True" jammed and began to skip, repeating the partial line "Girl, you know it's-" over and over. According to the premiere episode of VH1's Behind the Music which profiled Milli Vanilli, fans attending the concert didn't seem to notice or even care and the concert continued as if nothing unusual had happened, but critics did notice and questioned Rob and Fab in their concert reviews.</p></div></blockquote>

Must have been awful, not only being found out and stripped of their Grammy, but actually having to put someone else's voices on the recording in the first place. In 1998, ten years after Milli Vanilli's debut, one of the two members was found dead in a Frankfurt hotel of an apparent drug overdose.

<p class='separator'>&#10086;</p>

Since today has been a day of little things — reporting insignificant bugs minor TODOs, fixing small errors in S29, writing short sentences on IRC — I thought I'd instead mention three insignificant CSS design principles that I wish everyone on the web would follow. We can't all be visual designers or artists, but we can all take a step back from our creations, look closely at it, think "man, that sucks", and *actually fix it*. The things I describe are all easily fixed.

1.  **Give yourself some space.** Put in margins and padding, and a reasonable line height, letting the contents breathe a little. Maybe programmers tend to think that the more contents you can cram into the same area, the better, but only optimising for content density soon leads to a cluttered feeling. I find that well-thought-out layouts usually manage both to show a lot of content and have a lot of free space.
2. Make sure that different element of the page **don't collide.** I'm looking at use.perl's layout as I write this, and at my particular settings and sizes in Firefox, the last link in one of the link rows at the top runs into my user info box. In this case, I'm lucky, and the link is shown above the user info box, so were I to click it, I would probably succeed. It might just as easily have been below the box instead, unclickable. Pages which prevent you from navigating by hiding vital links or article text from you are no fun. It's especially frustrating when article text is hidden by advertising sidebars.
3. If an element changes when you hover over it with the mouse pointer **doesn't change size.** I see this sometimes: you hover over something, and the browser goes into a very rapid loop of rendering the item alternately in its hovered and non-hovered states, simply because the rendering of one causes the state to flip. Sometimes things further down on the page dance around too. This doesn't happen if one makes sure from the start that the size of the object is not affected by hovering.

I'm thinking of these sorts of thing now that I'm designing my own layout. After writing the above, I took a critical look at [november-wiki.org](http://www.november-wiki.org/). Well, the design *does* follow the above three rules — the second two trivially by not having elements that can overlap, nor elements that do funny things when hovered over — but it still isn't particularly sexy. I like to be inspired by layouts at [CSS Zen Garden](http://www.csszengarden.com/), they are often stunningly beautiful, and very inspiring.

Aesthetics is very important in our daily lives, including on web pages.


