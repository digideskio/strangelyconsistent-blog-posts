---
title: Trinity
author: Carl Mäsak
created: 2016-04-28T17:07:14+02:00
---
Lately, driven by a real itch I wanted to scratch, I finally wrote my first Perl 6 web app since the November wiki engine. (That was back in 2008. Very early days. I distinctly recall Rakudo didn't have a does-file-exist feature yet.)

Because I'm me, naturally the web app is a game. A board game. The details don't matter for this article &mdash; if you're curious, go check out [the README](https://github.com/masak/nex/blob/master/README.md). If you're not, I forgive you. It's just a game with a board and stones of various colors.

Here's the order in which I wrote the web app. The first thing I made work was a *model*, literally [the game itself](https://github.com/masak/nex/blob/fc6941f0ddc11a593d9ec8803a6690b2950cda28/lib/Games/Nex.pm#L71-L254).

<center><img src="http://strangelyconsistent.org/blog/images/three-tier-01.png" alt="image of the model in the middle"></center>

This is the core, the heart of the application. A `Games::Nex` instance models the progress of a game, and the rules of Nex are encoded in this type's "algebra". Naturally, this was developed [test-first](http://strangelyconsistent.org/blog/why-tests-will-change-the-way-you-code), because it would be reckless not to. This is the essence of [masakism](http://strangelyconsistent.org/blog/after-the-masakism-workshop): "testing gets you far" and "keep it small and simple". Eliminate all other factors, and the one which remains must be the model at the core.

The core domain is nice, but it doesn't have any face. By design, it's just mind stuff. I wanted it to be a web app, so the next reasonable step was to add a Bailador app that could show the board and allow moves to be tried on it.

<center><img src="http://strangelyconsistent.org/blog/images/three-tier-02.png" alt="image of the app+model in the middle"></center>

Of course, when you run it &mdash; it's a *web* app &mdash; the result turns up in your browser, as you're expect.

<center><img src="http://strangelyconsistent.org/blog/images/three-tier-03.png" alt="image of browser &lt;--- app+model"></center>

And when you make a move by interacting with the web page, the browser goes "Hey, app! Yo! Do some stuff!"

<center><img src="http://strangelyconsistent.org/blog/images/three-tier-04.png" alt="image of browser ---&gt; app+model"></center>

None of this is news to you, I'm sure. My point so far though is that we're now up to *two* environments. Two separate runloops. It has to be that way, because &mdash; in every single case except the crazy one where *I am you* &mdash; the server and the client will be two different computers. The GET and POST requests and their responses are simply client and server shouting to each other across the network.

Now my app had a face but no long-term memory. Every move was against an empty game board; you made it, and all traces of it disappeared into the great data limbo.

Time to add (you guessed it) a database backend:

<center><img src="http://strangelyconsistent.org/blog/images/three-tier-05.png" alt="image of browser &lt;---&gt; app+model &lt;---&gt; db"></center>

(Postgres is great, by the way. Highly recommended. Makes me feel happy just thinking about it. Mmm, Postgres.)

The database also sits in its own, separate runloop. Maybe &mdash; probably &mdash; on its own machine, too. So we have three environments.

It was at this point I felt that something was... well not *wrong* exactly, but *odd*...

    <masak> I was struck by not just how wide apart the database world is from
            the server backend world, but also how wide apart the [browser]
            world is from the server backend world
    <masak> you're writing three separate things, and making them interoperate
    <masak> I almost feel like writing a blog post about that

Coke insightfully quipped:

    <[Coke]> this reads like you discovered a 3-tier web app.

And yes, that is the name for it. Freakin' three-tier. Love those bloody tiers. Like a hamburger, but instead of meat, you got a *tier*, and not in bread as usual, but between two *more* tiers!

I have a subtle point I want to make with this post, and we're now getting to it. I'm well aware of how good it is to separate these three tiers. I teach a software architecture course several times yearly, and we've made a point to be clear about that: the model in the middle should be separated, nay, *insulated*, from both UI and DB. Why? Because UI layers come and go. First it's a desktop client, some years later it's a responsive Web 2.0 snazzle-pot of janky modernity, and surely in a few more decades we'll all be on VR and telepathy. Throughout all this, the model can stay the same, unfazed. Ditto the database layer; you go from RDBMS to NoSQL back to PffyeahSQL to an in-memory database to post-it-notes on the side of your screen. [The model abides](https://www.youtube.com/watch?v=sYsw0KVRjCM).

And yet... I want to make sure I'm building a single monolithic system, not three systems which just happen to talk to each other. *That's* my thesis today: even in a properly factored three-tier system, it's *one* system. If we don't reflect that in the architecture, we have problems.

<center><img src="http://strangelyconsistent.org/blog/images/jay-z-message-passing.jpg" alt="If you have three tiers I feel bad for you son / I got 99 problems but message passing ain't one"></center>

So, just to be abundantly clear: the three tiers are *good* &mdash; besides being a near-necessity. The web is practically dripping with laud and praise of three-tier:

> By segregating an application into tiers, developers acquire the option of modifying or adding a specific layer, instead of reworking the entire application. &mdash; [Wikipedia](https://en.wikipedia.org/wiki/Multitier_architecture)

> During an application's life cycle, the three-tier approach provides benefits such as reusability, flexibility, manageability, maintainability, and scalability. &mdash; [Windows Dev Center](https://msdn.microsoft.com/en-us/library/windows/desktop/ms685068(v=vs.85\).aspx)

> Three-tier architecture allows any one of the three tiers to be upgraded or replaced independently. &mdash; [Technopedia](https://www.techopedia.com/definition/24649/three-tier-architecture)

It's against this unisonal chorus of acclaim that I'm feeling that some kind of balancing force is missing and never talked about. Why am I writing three things again? I'm down in the database, I'm writing code in my model and my app, and I'm away writing front-end JavaScript, and *they are basically unrelated*. Or rather, they are only related because I tend to remember that they should be.

I think it's especially funny that I didn't really *choose* the three-tier model along the way. It was forced upon me: the browser *is* a separate thing from the server, and the database engine *is* a separate thing from the app. I didn't make it so! Because I didn't make that design decision, it's also not something to be proud of afterwards. "Ah, look how nice I made it." When everyone keeps repeating how good it is with a software architecture that's not a choice but a fact of the territory, it sounds a bit like this:

> Potholes in the road allow us to slow down and drive more carefully, making our roads safer. &mdash; no-one, ever

Maybe we can close our eyes and imagine a parallel universe where some enlightened despot has given us the ultimate language Trinity, which encompasses all three tiers, and unites all concerns into one single conceptual runloop, one single syntax, and one single monolithic application. Sure, we still do DB and frontend stuff, but it's all *the same code base*, and incidentally it's so devoid of incidental complexity, that pausing to think about it will invariably make us slightly misty-eyed with gratitude.

Would that work? I honestly don't know. It might. We don't live in that universe, but I sure wouldn't mind visiting, just to see what it's like.

One very good argument that I will make at this point, before someone else makes it for me, is that the three tiers actually handle three very different *concerns*. Basically:

* **UI tier**: displaying state to and putting together requests from the user.
* **App/model tier**: processing application requests while upholding model invariants.
* **DB tier**: persistent storage, retrieval.

These differences go *deep*, to the point where each tier has a quite specialized/different view of the world. To wit:

* The model cares about a certain ongoing game being in **a certain well-defined state**. It doesn't care about what happened five moves ago.
* The database, on the other hand, stores *moves*, not *game states*. Why? Because the game state can be recreated from the moves, but not the other way around. **Individual moves are more denormalized** if you will, and allow us to do so much more with a game later. (Thanks for this insight, Event Sourcing!) But this means that what's in the model doesn't look structurally like what's persisted.
* Meanwhile, the frontend contains a full replica of the game state from the server &mdash; how wasteful &mdash; *but* it also has **a lot of "transient" state** reflecting (for example) the move that the player is *about* to make. (This is especially apparent in Nex, since making a move is a multi-step process.) The frontend also cares a lot about positions, colors, the keyboard, the mouse, and how these all relate to each other.

But it doesn't end there. The three tiers also have very typical, well-defined relations to each other.

* **The frontend has no way to talk to the database.** This kind of falls out of the expectations of the three-tier thing. In the strongly-typed Trinity (over in the parallel universe), having the frontend talk to the database is about as meaningful as trying to divide by a filehandle.
* **The app trusts the frontend about as far as it can throw it.** [All input is evil, mmkay?](http://www.codemag.com/article/0705061) Usually, the app checks that the input is *sane* before the model even gets to see it. (Yeah, yeah, [I'll get to it](https://github.com/masak/nex/blob/95a55367960f93e2a8b28a01d9b9cdf505bf5904/app.pl#L95). Heh.) Note that the frontend logic can be bug-free, meticulous, and basically the most honest person you've ever met. It doesn't matter. In Trinity, the static type system would make sure that values could never just pass from client to server.
* On the other hand, **there's no reason to distrust the database**. Let's assume for simplicity that it's *our* database. (Pro tip: consider not sharing your database with other apps, even when that's an option. Cordially, [microservices](https://twitter.com/mathiasverraes/status/711168935798902785).) So the only one who ever put data in there is the app. And the app only put data &mdash; moves in our case &mdash; after running it through the model. So... *either* all the data we've persisted in the database is kosher, *or* our model has approved corrupt data. In which latter case we have bigger fish to fry.
* The above reasoning presupposes that the model never changes. Consider this: if a game was played according to faulty rules which were later corrected (or a faulty implementation of the rules which was later fixed), that game still *happened*, and so the move events in the database are in some sense **a correct reflection of the past**. Therefore, somewhat crazily, the model can't/shouldn't apply any kind of validation on the database. Total trust. A method `from-moves` in `Game::Nex` does just this: when the app asks what game state some moves from the database correspond to, the method [builds that game state](https://github.com/masak/nex/blob/95a55367960f93e2a8b28a01d9b9cdf505bf5904/lib/Games/Nex.pm#L224-L246), bypassing all kinds of model assertions. (Again, this is Event Sourcing 101: event application doesn't do validation.)

And I haven't even started talking about synchronous/asynchronous calls, timeouts, retries, optimistic UIs, user experience, contention, conflicts, events pushed from the server, message duplication, and error messages.

I guess my point is that however Trinity looks, it has a lot on its plate. I mean, it's wonderful that a Trinity system is *one* single application, and things that ought to be close to each other in code can be... but there's still a lot of concerns. Sometimes [the abstraction leaks](http://www.joelonsoftware.com/articles/LeakyAbstractions.html). You can practically see that server-code block glare in paranoid suspicion at that client-code block. And, while Trinity of course has something *much* more unified than SQL syntax, sometimes you'd feel a little bump in the floor when transitioning from model code to DB code.

But I could imagine liking Trinity a lot. Instead of writing one page Perl 6 and two pages JavaScript, I'd just implement the code once in Trinity. Instead of describing the model once for the server and once for the database, I'd just describe it once. Parallel universe, color me jealous.

But back to our world with three inevitable tiers. I've been thinking about what would make me feel better about the whole ui-app-db impedance mismatch. And, I haven't tried it out yet, but I've decided I want to borrow a leaf from 007 development. In 007, basically when we've found things that might go wrong during development, we've turned those things into tests. These are not testing the *behavior* of the system, but the *source code* itself. Call them consistency tests. We really like them; they make us feel ridiculous for making silly consistency mistakes, but also grateful that the test suite caught them before we pushed. (And if we accidentally push, then TravisCI notifies us that the branch is failing.)

What needs testing? As so often, the interfaces. In this case, the surface areas between frontend/app, and between app/database.

<center><img src="http://strangelyconsistent.org/blog/images/three-tier-06.png" alt="image of browser &lt;-|-&gt; app+model &lt;-|-&gt; db"></center>

Here's what I'm imagining. The frontend and app are only allowed to talk to each other in certain pre-approved ways. The tests make sure that each source file doesn't color outside of the line. (Need to make that code analysis robust enough.) Ditto with the app and database.

I figure I could use something pre-existing like [Swagger](http://swagger.io/) for the frontend/app interaction. For the app/db interaction, actually using the database schema itself as the canonical source of truth seems like a decent idea.

Maybe it makes sense to have both static tests (which check the source code), and runtime tests (which mock the appropriate components and check that the results match in practice). Yes, that sounds about robust enough. (I'm a little unsure how to do this with the client code. Will I need to run it headless, using [PhantomJS](http://phantomjs.org/)? Will that be enough? I might need to refactor just to expose the right kind of information for the tests.)

*That* would give me confidence that I'm not breaking anything when I'm refactoring my game. And it might make me feel a little bit better about the Trinity language being in a parallel universe and not in this one.

I might write a follow-up post after I've tried this out.
