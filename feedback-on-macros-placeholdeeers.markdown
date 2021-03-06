---
title: Feedback on "Macros: Placeholdeeers!"
author: Carl Mäsak
created: 2014-12-13T12:06:31+01:00
---
These comments pertain to [this pooost](http://strangelyconsistent.org/blog/macros-placeholdeeers).

* [lue](http://irclog.perlgeek.de/perl6/2014-12-04#i_9760374): '"Does this whole thing remind you of beta reduction in lambda calculus?" um, sure :) .'
* [TimToady](http://irclog.perlgeek.de/perl6/2014-12-05#i_9765361): "I think the CoffeeScript approach only works well for pieces of grammar that can have a specific piece of code stand in for a generic node, which is easier for nouns, but harder for more abstract grammatical constructs, like a new trait syntax, or a new parameter syntax. So I'd rather see something tied a little closer to grammatical categories rather than just ad hoc chunks of target text that the substituter might have to guess on which thing, or level of thing, is actually being referred to."
* [geekosaur](http://irclog.perlgeek.de/perl6/2014-12-05#i_9765647): "seems to me we need (a) some way to introspect the names mentioned in an AST (b) these names should default to be new names not related to any existing ones (i.e. it is an independent scope) unless <a> is used to associate with names presumably via CALLER:: or etc."
* [TimToady](http://irclog.perlgeek.de/perl6/2014-12-05#i_9765675): "`quasi { ¤A ¤B ¤C }`  # is A a listop? is it a declarator? is B an infix? is C an initializer? the parser must know in advance in order to parse correctly"
* [TimToady](http://irclog.perlgeek.de/perl6/2014-12-05#i_9765767): "the most generic macro in P6 is going to look a lot like a parsing rule with an AST maker for its action closure, which may or may not build the returned AST out of quasis, that may or may not refer to the bits parsed by the parse rule, presumably by the same name they were parsed under"
* [TimToady](http://irclog.perlgeek.de/perl6/2014-12-05#i_9765785): "sometimes I think even `%*LANG` is the wrong way to handle braids"
* [japhb](http://irclog.perlgeek.de/perl6/2014-12-06#i_9766743): [wip macro idea](https://gist.github.com/japhb/92442a22962c5156e102)
* [masak](http://irclog.perlgeek.de/perl6/2014-12-07#i_9770110): "japhb: using parameter typing to decide the grammatical category of incoming ASTs seems, hm, both right and wrong somehow."
* [masak](http://irclog.perlgeek.de/perl6/2014-12-07#i_9771308): 'learning from tinkering around with Qtrees: there are things out there that are neither terms, nor expressions, but kind of head off in another direction. yet they are very "structural" and expression-like. they sort of occur in "restricted" parts of the language. that's also currently where unquotes have trouble going.'
* moritz (chiming in via email): "I feel you overlooked a kinda important point in your examples and analysis: In Perl 6, all variables must be declared. So [the example with the substitution](https://gist.github.com/masak/fbc45fdb1fa7861f8fd4) goes BOOM on the first '$s'. Two of your three examples miss the variable declarations, which usually isn't your style. Something else might be going on."
* masak (replying): "It seems to me now that the simplicity of `.subst` is dependent on implicit variable declarations in a way I hadn't appreciated when writing the post."
