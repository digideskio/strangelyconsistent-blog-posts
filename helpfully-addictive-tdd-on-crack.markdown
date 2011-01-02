---
title: Helpfully addictive: TDD on crack
author: Carl Mäsak
created: 2009-09-17T15:08:00+02:00
---
I'm going to introduce a fine piece of vaporware today. It's a harness I've started writing several times in different situations, one I've never let really bloom out into its full potential. Actually, I'm not sure what'll happen when it does. This time I intend to find out, for better or worse.

Time to **write a truly addictive TDD harness**, and see what happens.

I've never been addicted to [World of Warcraft](http://en.wikipedia.org/wiki/World_of_Warcraft) or any such online multiplayer games. My closest vice is probably [Angband](http://rephial.org/), a well-crafted ASCII-based one-player cave-exploring game. Two months ago, I picked up Angband again, and had a few weeks of heavy playing. During that time, I started thinking about the psychology of the game, the dynamics. What makes it so alluring? Why does it pull me in and keep me in?

Well, a few things. It's open-ended. It reacts to things I do. There are small victories all the time: winning a battle, finding gold and items, learning new spells... the whole game is geared so that I constantly put in things to evolve my character, making new opportunities open up. No messages "now you got a little further" are required; the rewards take place in your brain, which recognizes progress all by itself. And that kind of reward, the one where your own brain recognizes partway goals and triumphs, is much more efficient. (That's not to say that little encouraging messages don't work. Angband has leveling-up messages, too. And many different quantitative scores.)

Perhaps most important: it just goes on and on. It's certainly possible to win in Angband, but it's a very far-off goal. Most of the game consists of getting a little better all the time. What keeps me playing it for hours on end is that the game is finely tuned so that there's always a small goal within reach. Just going to get the treasure on this level. Just going to kill that group of ogres. Just going to get back to town. The game effectively discourages turning it off, because you always have something to do.

I don't think I've ever been happier as a programmer than when I applied this type of addiction-inducing style to my coding. I don't think I've been more efficient, either. It started with a small script that sat in a terminal window on its own next to the one I was coding in — I was running [blackbox](http://bb.lnix.net/screenshots/blackbox_lappy_uniq.jpg) at the time, and my screen literally showed only those two terminal windows — and every time I saved a class file to disk in my editor, the script did an automatic build. Wow! Already, the result was liberating. It was eliminating mode-switches for me, and thereby moments where I could think "hm, maybe it's time for dinner", or similar useful but distracting thoughts.

The script quickly evolved into runnning the test suite after a successful build. And then, if the tests succeeded to a greater extent than previously, the script even made a commit. Triple wow! Now I never needed to leave the editor, because my intelligent friend in the other terminal window just kept Doing The Right Thing for me, all the time... forever.

What I've now come to realize is that I didn't go far enough. I still had to write the commit message manually, for example. And in order for the test-writing step not to require too much deliberation, it's probably good practice to make a small outline of which tests you're planning to write. Such an outline could then be hooked into the test-writing process, feeding you one idea at a time. Lastly, my tool was too specialized; ideally, one should be able to hook the harness up to any development project, so it can't have hard-coded paths, like mine did. Instead, one could optimize it for the standard project directory structure, or some such.

So now, I'll introduce [tote](http://github.com/masak/tote), an addictively helpful TDD harness for Perl 6. (It's named after a [strategy based on feedback loops](http://en.wikipedia.org/wiki/T.O.T.E.).) It does all my old harness did, but it's honed to perfection to such an extent that you have to make sure someone can snap you out of your development-induced stupor to occasionally eat and sleep. Otherwise you might just go on for days until you faint from weakness.

Oh, and I haven't actually written it yet. I haven't even started. (Hence "vaporware".) But I have a [nice graph](http://github.com/masak/tote/blob/master/diagram/tote-workflow.png) showing the general workflow. Again, the nice (and addictive thing about this harness is that the pink boxes are fully automated. Tote just does the right thing for you. Even the yellow box, marking a point where you've progressed in some way, can be fully automated if you want it. Only the blue boxes require human intervention, but they are quite small tasks, and the harness *tells you what you need to do*.

Man, I feel addicted already.


