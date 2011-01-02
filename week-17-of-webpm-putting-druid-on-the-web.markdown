---
title: Week 17 of Web.pm &#8212; putting Druid on the Web
author: Carl Mäsak
created: 2009-10-12T06:00:00+02:00
---
<dl>
<dd> <i>OK so iz liek teh nites and Jesus camez out, and he iz srsly wlking ontop of teh waterz. Teh dscpls sawed him and ar scured and tehy sayed "OMG haxx!" But Jesus sez "O hai! Iz just me. lol." "Hay dood. dat u?" sez Peter, "I can comez ontop teh waterz plz?" "yah lol," sez Jesus. Den Peter camed ontop teh waterz and iz goes to Jesus. But Peter sawed teh windz and he got scareded and he fallz in teh waterz and wus liek "Halp! Invisible sidewalk! DO NOT WANT!" Jesus halps Peter and sez "U n00b, ur doin it rong. Why U not beleev?"</i> &#8212; Matthew 14:25-31</dd>
</dl>

I decided to spend this week getting [Druid](http://github.com/masak/druid) online. It's been a while since I spent tuits on Druid, but getting it into a web app has been a goal for a long time.

- What's the first time that greets a Perl 6 developer who has been away from a project for a few months? Bitrot. This time, it was `int()` and `infix:</>` that needed to be `.Int` and `infix:<div>`, respectively.
- Next step was to make a game position survive from one web request to the next. We're not fortunate enough yet to have technology which lets us run the same Rakudo process for the whole lifespan of a web application. Instead, we start a new process for each web request. Consequently, the game state needs to be persisted somewhere.
- Now, [Squerl](http://strangelyconsistent.org/blog/week-16-of-webpm-more-squerl-work) would be perfect for this: just make up a table and stick the game state in there. But I chose to do something simpler for the time being: write to file. That's what Squerl will have to do anyway. As the Druid web application grows, and needs to persist more things than just one game state, Squerl will naturally step up to buffer the added complexity.
- What, then, is running under the hood to make Druid show up as a web app? First off, it's using the core Web.pm interface with `Web::Request` and `Web::Response` to produce [the actual web application](http://github.com/masak/druid/blob/master/lib/Druid/Webapp.pm). This is working fairly well nowadays. Separate from this is a [small wrapper script](http://github.com/masak/druid/blob/master/bin/web-druid) which connects the web app to a `Web::Handler`, an adapter to a specific webserver backend. In this case, we're using the handler [Web::Handler::HTTPDaemon](http://github.com/masak/web/blob/master/lib/Web/Handler/HTTPDaemon.pm) which connects to mberends++' `http-daemon`. It's quite satisfying to see all that infrastructure in place to support creating a web app in a simple way.
- Oh, and I found another piece of bit rot in `Web::Handler::HTTPDaemon`: jnthn++ had recently fixed the bug where Rakudo thinks that `Callable &foo` means that foo is `Callable`, rather than a `Callable` returning a `Callable` (or `Callable[Callable]` for short). So I [fixed that](http://github.com/masak/web/commit/c3b9264c9db8adf7920a6fb4c3a1846e581c27c7).
- I'm actually ready to have a small, really unimpressive live demo here. But alas, I feel I have run out of time. So the best thing I can do now is to give instructions for how to run the live demo at home. Basically, do this: `cd druid; bin/web-druid` — but for that to work, you'll need Druid, Web.pm and http-daemon, all available through proto. It's a good idea to precompile all three of them for speed reasons. When you run `bin/web-druid`, it'll tell you which URL to go to in order to play the game.
- Oh, and don't expect too much. The web app is very bare-bones right now. I'm kinda hoping people will recognize that it's a great game, and help improve the interface. Adding SVG would be kinda nice, for example.

I wish to thank The Perl Foundation for sponsoring the Web.pm effort.


