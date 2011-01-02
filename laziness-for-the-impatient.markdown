---
title: Laziness for the impatient
author: Carl Mäsak
created: 2009-10-11T17:35:00+02:00
---
In the future — the not too distant future, if I read the [ROADMAP](http://github.com/rakudo/rakudo/raw/master/docs/ROADMAP) correctly — Rakudo will be able to handle potentially infinite streams of values.

    my @a = gather {
        my $count = 5;
        take $count++ while True;
    };
    
     .say for @a[0..4]; # 5\n6\n7\n8\n9


Actually, not only the `gather` construct does this, but lists themselves:

    my @a = 5 ... *;
     .say for @a[0..4]; # 5\n6\n7\n8\n9


Neither of these two work yet. The former hangs, and the latter [just earned me a rakudobug](http://rt.perl.org/rt3/Ticket/Display.html?id=69728). (*Update 2009-10-13:* A rakudobug which, 46 hours later, moritz++ [fixes](http://github.com/rakudo/rakudo/commit/3d1afedfd18a85d570c96937d9abe7f0d6128c49). Complaining is extra rewarding when the reaction is swift.)

Anyway, awaiting [all that lazy goodness](http://en.wikipedia.org/wiki/Lazy_evaluation), we can always fall back on the "old-fashioned" way of lazily generating a stream, namely a closure. The following code *does* work in Rakudo:

    class LazyIterator {
        has $!it;
    
        method get() {
            $!it();
        }
    }
    
    class Counter is LazyIterator {
        method new(Int $start) {
            my $count = $start;
            self.bless(*, :it({ $count++ }));
        }
    }
    
    my $c = Counter.new(5);
    say $c.get for 0..4; # 5\n6\n7\n8\n9


In our daily fight against [scattered and tangled](http://en.wikipedia.org/wiki/Aspect-oriented_programming#Motivation_and_basic_concepts) code, closures are a fine weapon to have in one's arsenal. The fact that they are capable of giving us laziness in Rakudo today, *before it is actually implemented*, is a nice example of that.


