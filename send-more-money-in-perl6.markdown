---
title: Send more money (in Perl 6)
author: Carl Mäsak
created: 2015-05-25T22:59:01+02:00
---
In which I implement four different Perl 6 solutions to [MJD's SEND + MORE = MONEY challenge](http://blog.plover.com/prog/haskell/monad-search.html).

I encourage you to read that post if you haven't already, but here's a short tl;dr: Haskell's `do` notation is wonderful in that it allows the author to cleanly express a backtracking algorithm without any "noise" such as explicit backtracking information, or indentation. Monads may be weird and frightening, but proponents of other languages should take heed: `do` notation is nice.

Can we do as nicely in Perl 6?

## Version A: recursion

Here we're trying to get as close as possible to the original Haskell code without using any tricks. Basically trying to match the essence of the problem line by line. We're hampered by not having a `do` notation, of course, and no built-in backtracking in the main language. The program pretends to have no indentation, because the indentation isn't really relevant.

    my @digits = 0..9;
     
    choose @digits ∖ 0, -> $s {
    choose @digits ∖ $s, -> $e {
    choose @digits ∖ ($s, $e), -> $n {
    choose @digits ∖ ($s, $e, $n), -> $d {
    my $send = :10[$s, $e, $n, $d];
     
    choose @digits ∖ (0, $s, $e, $n, $d), -> $m {
    choose @digits ∖ ($s, $e, $n, $d, $m), -> $o {
    choose @digits ∖ ($s, $e, $n, $d, $m, $o), -> $r {
    my $more = :10[$m, $o, $r, $e];
     
    choose @digits ∖ ($s, $e, $n, $d, $m, $o, $r), -> $y {
    my $money = :10[$m, $o, $n, $e, $y];
     
    guard $send + $more == $money, {
    say "$send + $more == $money";
    }}}}}}}}};
     
    sub choose(Set $choices, &fn) {
        for @$choices -> $value {
            &fn($value);
        }
    }
     
    sub guard($condition, &fn) {
        if $condition {
            &fn();
        }
    }

This takes about 26 minutes to run on my laptop. I despaired at this &mdash; the original Haskell version finishes in less than a second &mdash; but then I wrote an equivalent Perl 5 version, and it took 8 minutes. Paradoxically, that somehow made me feel less bad about Perl 6's performance. ("Wow, we're within an order of magnitude of Perl 5!")

## Version B: iteration

Where the previous version tried to stick close to the original, this version just dumps all such concerns and tries to go fast. It does so by spewing out explicit loops, checks, and native integers.

    my int ($send, $more, $money);
     
    loop (my int $s = 0; $s <= 9; ++$s) {
        next if $s == 0;
     
        loop (my int $e = 0; $e <= 9; ++$e) {
            next if $e == $s;
     
            loop (my int $n = 0; $n <= 9; ++$n) {
                next if $n == any($s, $e);
     
                loop (my int $d = 0; $d <= 9; ++$d) {
                    next if $d == any($s, $e, $n);
     
                    $send = :10[$s, $e, $n, $d];
     
                    loop (my int $m = 0; $m <= 9; ++$m) {
                        next if $m == any(0, $s, $e, $n, $d);
                        
                        loop (my int $o = 0; $o <= 9; ++$o) {
                            next if $o == any($s, $e, $n, $d, $m);
     
                            loop (my int $r = 0; $r <= 9; ++$r) {
                                next if $r == any($s, $e, $n, $d, $m, $o);
     
                                $more = :10[$m, $o, $r, $e];
     
                                loop (my int $y = 0; $y <= 9; ++$y) {
                                    next if $y == any($s, $e, $n, $d, $m, $o, $r);
     
                                    $money = :10[$m, $o, $n, $e, $y];
                                    next unless $send + $more == $money;
     
                                    say "$send + $more == $money";
                                }
                            }
                        }
                    }
                }
            }
        }
    }

(cygz++ for pointing out that `loop (;;)` works a lot better here than `while` and manual increment; and for rewriting the code accordingly.)

Despite trying really hard, this version still takes 5 minutes. The corresponding Perl 5 code (which doesn't do natives) takes 3.3 seconds.

## Version C: regex engine

Now for a version that tries to capitalize on the regex engine having backtracking behavior. The basic idea (using `amb`) comes from [Rosetta Code](http://rosettacode.org/wiki/Amb#Perl_6). I'm a teeny bit disappointed `amb` has to resort to building regex fragments as strings, which feels inelegant.

    sub amb($var, @a) {
        "[{
            @a.map: {"||\{ $var = '$_' }"}
         }]";
    }

    sub infix:<except>(@lhs, @rhs) { (@lhs (-) @rhs).list }

    my @digits = 0..9;

    "" ~~ m/
        :my ($s, $e, $n, $d, $m, $o, $r, $y);
        :my ($send, $more, $money);

        <{ amb '$s', @digits except [0] }>
        <{ amb '$e', @digits except [$s] }>
        <{ amb '$n', @digits except [$s, $e] }>
        <{ amb '$d', @digits except [$s, $e, $n] }>
        { $send = :10[$s, $e, $n, $d] }
        <{ amb '$m', @digits except [0, $s, $e, $n, $d] }>
        <{ amb '$o', @digits except [$s, $e, $n, $d, $m] }>
        <{ amb '$r', @digits except [$s, $e, $n, $d, $m, $o] }>
        { $more = :10[$m, $o, $r, $e] }
        <{ amb '$y', @digits except [$s, $e, $n, $d, $m, $o, $r] }>
        { $money = :10[$m, $o, $n, $e, $y] }

        <?{ $send + $more == $money }>
        { say "$send + $more == $money" }
    /;

On the plus side, this algorithm nails the linear code layout and gets fairly close to being nice and clean. There's a bit of noise along the fringes, what with all the `{ }` and `<{ }>` and `<?{ }>`, but for a Perl 6 regex, this is good going.

Too bad it's so damn slow. Extrapolating from a shorter run, I estimate that the program would take around 100 minutes to finish. But it gets killed off on my system after 88 minutes because it leaks ridiculous quantities of memory (11 MB a second, or 660 MB a minute). I wonder if I could submit that as a rakudobug.

## Intermezzo

If you're new to Perl 6, you might not recognize `(-)` as set difference. I could also have used `∖` (U+2216 SET MINUS), but for once, the Texas version felt clearer.

I also like the clarity of `$send = :10[$s, $e, $n, $d]`. In the Perl 5 versions, I ended up with this helper sub that does the same.

    sub base_10 {
        my (@digits) = @_;
        my $result = 0;
        while (@digits) {
            my $digit = shift @digits;
            $result *= 10;
            $result += $digit;
        }
        return $result;
    }

Perl 6 just treats it as a variant of the base conversion syntax.

## Version D: macros/speculation

Now, obviously, the solution that isn't burdened down by properly existing yet is also the cutest one.

    use Hypothetical::Solver;

    my @digits = 0..9;

    solve {
        my $s = amb @digits (-) [0];
        my $e = amb @digits (-) [$s];
        my $n = amb @digits (-) [$s, $e];
        my $d = amb @digits (-) [$s, $e, $n];
        my $send = :10[$s, $e, $n, $d];
        my $m = amb @digits (-) [0, $s, $e, $n, $d];
        my $o = amb @digits (-) [$s, $e, $n, $d, $m];
        my $r = amb @digits (-) [$s, $e, $n, $d, $m, $o];
        my $more = :10[$m, $o, $r, $e];
        my $y = amb @digits (-) [$s, $e, $n, $d, $m, $o, $r];
        my $money = :10[$m, $o, $n, $e, $y];

        guard $send + $more == $money;
        say "$send + $more == $money";
    }

Clearly, this won't even compile, as it's missing a dependency. Let's supply it with the smallest possible dependency, just honoring signatures:

    module Hypothetical::Solver {
        sub solve(&block) is export {}
        sub amb($set) is export {}
        sub guard($condition) is export {}
    }

Which... is useless, because now we have a program which looks pretty but does nothing.

So let's fix that. Here I have *another* program which eats the first program for breakfast. More exactly, it can parse the program and emit a new one that solves the problem. Be aware that the below is a bit of a hack (I'll get back to that), but at least each individual part is nice and self-contained.

    grammar Solver::Syntax {
        token TOP { <statement>* }

        proto token statement {*}

        token statement:sym<use> {
            <sym> \s+ ([\w | '::']+) ';' \s*
        }

        token statement:sym<my> {
            <sym> \s+ \S+ \s* '=' \s* <!before 'amb'> <-[;]>+ ';' \s*
        }

        token statement:sym<solve> {
            <sym> \s+ ('{' \s*) <statement>* ('}' \s*)
        }

        token statement:sym<guard> {
            <sym> \s+ (<-[;]>+ ';' \s*)
        }

        token statement:sym<say> {
            <sym> \s+ <-[;]>+ ';' \s*
        }

        token statement:amb-my {
            'my' \s+ (\S+) \s* '=' \s* 'amb' \s+ (<-[;]>+) ';' \s*
            <statement>*
        }
    }

    class Solver::Actions {
        method TOP($/) {
            make $<statement>».ast.join;
        }

        method statement:sym<use>($/) {
            make $0 eq "Hypothetical::Solver" ?? "" !! ~$/;
        }

        method statement:sym<my>($/) {
            make ~$/;
        }

        method statement:sym<solve>($/) {
            make $0 ~ $<statement>».ast.join ~ $1;
        }

        method statement:sym<guard>($/) {
            make "next unless " ~ $0;
        }

        method statement:sym<say>($/) {
            make ~$/;
        }

        method statement:amb-my ($/) {
            make "for ($1).list -> $0 \{\n" ~ $<statement>».ast.join.indent(4) ~ "\}\n";
        }
    }

(Entire script is [here](https://gist.github.com/masak/f4cfdc9abc2a579ec531).)

The result is closest in spirit to version B above. But it doesn't try to be as optimized. As a result of this, it actually performs like version A, and finishes in 26 minutes.

Let me just conclude by making a few points.

* We're doing textual substitution, and the solution carries with it everything I detest about textual macros. (Also sometimes known as "unhygienic macros", though I maintain that if a macro is textual, it's already so bad that the notion of hygiene isn't even applicable.)
* So please consider this a throwaway prototype. Textual substitution just happens to be what we can do right now. The *real* solution would of course work on the AST level.
* The most interesting part of the transformation (in my view) is what `statement:amb-my` does. It's saying "take the remaining statements in this block and nest them inside my block". This naturally paves the way for *AST transformers*, something I haven't visualized quite so clearly until I started thinking about this problem. One might imagine a standard library of these making life easier for the macro author.
* `for` loops are slow. One would like to optimize them into `loop (;;)` (and `next`s) when possible. In this case it's possible, and a real macro transformer would have all the information available to find that it's possible. In fact, it wouldn't take *a lot* to make the above text-munging solution have enough information to do that optimization. (It'd need to store the statements in an intermediate AST form, which it could them query. [That's all.](http://strangelyconsistent.org/blog/its-just-a-tree-silly))

Lately I've been nosing around languages that compile to JavaScript. Such languages allow us to state the program in a nicer, more fit-for-the-task language than JavaScript, but still get all the advantages of being able to run things in the browser.

The intended use of macros in Perl 6 is similar to this: express the problem in a "nicer way" (variant D), then massage it down to something that you *could* have written but would rather prefer not to (variant B). The big difference between macros and slangs (IMO) is that macros allow you to parse normally and then mess with the resulting Qtree, whereas slangs allow you to replace the parser with something else entirely (and then mess with the Qtree too, if required).

The fan on my laptop is relieved that I'm done running programs for this post. 哈哈
