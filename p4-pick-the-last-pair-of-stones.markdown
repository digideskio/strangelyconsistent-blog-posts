---
title: p4: Pick the last pair of stones
author: Carl Mäsak
created: 2011-02-15T00:14:00+01:00
---
*With the Yapsi release over with, and the two [preparatory](http://strangelyconsistent.org/blog/the-thing-with-nim) [posts](http://strangelyconsistent.org/blog/that-is-so-octal) done, let's get back to summarizing the results of the [Perl 6 coding context](http://strangelyconsistent.org/blog/masaks-perl-6-coding-contest). Here we go.*

    <colomon> If only someone hadn't drained the tuits of most of us with some sort
    of contest.... ;)

In the *Star Trek* universe, there's a Starfleet training exercise called [Kobayashi Maru](http://en.wikipedia.org/wiki/Kobayashi_Maru), a simulation specially designed so that the cadet will fail. Quoting Wikipedia:

<div class='quote'>There is no way to win the resulting battle, especially since the computer is allowed to "cheat" to guarantee defeat; the simulation ends with the understanding that the cadet's ship has been lost with all hands. The objective of the test is not for the cadet to outfight the opponent but rather to test the cadet's reaction to a no-win situation.</div>

In planning the contest, I tried to choose some of the tasks this way: a seemingly simple problem with unexpected depth that would in essence "defeat" the contestants, so that I could study the contestants' reactions to a no-win situation. Only with p4 did this really work. *No-one* among the four finalists solved this one completely, and their attempts are varied and show different amounts of struggle against failure.

...except for one contestant, arnsholt++, who apparently solved the whole thing. His solution deserves an honorary mention because it actually uses the theory of impartial games to find the right moves. Unfortunately, arnsholt blew the deadline with one day, and only sent in four solutions. But apart from that, he's the James T. Kirk of this story. 哈哈 His code shows me that solving the problem definitely was within the bounds of the possible, and that one didn't have to write big amounts of code to solve it.

Here are [the solutions sent in](http://strangelyconsistent.org/p6cc2010/) (on time) and my reviews of them.

For the sake of people who didn't participate, here's the problem description for p4:

    == Pick the last pair of stones in a game of picking pairs of stones
    
    The game starts out with a ring of $N stones. Either the computer or the human
    player starts. Each move consists of picking up a pair of adjacent stones and
    removing them. Stones on either side of a removed pair do not become adjacent.
    The player unable to make a move loses; equivalently, the last player to make a
    legal move wins.
    
    With the stones numbered 0..$N-1, the pair must either be of the form ($k,$k+1)
    or ($N-1,0). (That is, the last stone is considered to be adjacent to the first
    stone.)
    
    For simplicity (rather than because it's an exemplar of brilliant user
    interface design), the first two lines of input are assumed to contain the
    number ($N) of stones and the player ('human' or 'computer') to make the first
    move, respectively.
    
    Each time the computer moves, the output is "computer takes " and then two
    comma-separated numbers of either of the allowed forms above.
    
    Each time the human moves, a line is output saying "human to move: " and then
    a list of the stones still remaining in the ring. The program then awaits
    input in the form of two comma-separated numbers.
    
    It's not possible for the computer to win all the time. As a trivial example,
    in a two-stone game with the human player starting, the game is over before
    the computer has the time to make a move. Barring hopeless cases such as
    this, your goal is to make a computer player that plays to win.
    
    There is some tactics to the game. For example, after the first move in a
    6-stone game, no matter which pair was removed there is now a chain of four
    stones left in the ring. Taking the middle two wins the game; taking any of
    the other two pairs gives the opponent the winning move.

There were a number of things that I *didn't* say, but which we now know, especially after the two [preparatory](http://strangelyconsistent.org/blog/the-thing-with-nim) [posts](http://strangelyconsistent.org/blog/that-is-so-octal):

* It is possible to play this game perfectly.
* Under perfect play, the game is already a foregone conclusion before the first move.
* Playing perfectly is a simple matter of putting the game in a losing position each time, which is the same as making all the components (nim-)sum to zero.
* The (nim-)value of each component can be easily calculated:

    $ perl6 -e 'my @g = 0, 0; print "0.0"; loop { my %set;
                ++%set{@g[$_] +^ @g[* - $_ - 2]} for 0 .. @g/2 - 1;
                my $mex = first { !%set.exists($_) }, 0..*;
                print $mex; push @g, $mex }'
    0.0112031103322405223301130211045274
      0112031103322445523301130211045374
      8112031103322445593301130211045374
      8112031103322445593301130211045374
      8112031103322445593301130^C

In a very real sense, this repeating sequence captures the strategy of the whole p4 game. First off, it tells us which starting positions

    The game starts out with a ring of $N stones.

will be a win for the first player, and which ones will be a win for the second player. The thinking is this: the first move in the ring always removes two adjacent stones, and which two we remove doesn't matter because of the symmetry of the ring. We are left with a row/heap of `$N - 2` stones. A nonzero value in the sequence above for this heapsize will tell us that the game will be a win for the *second* player (because it is now the second player's turn).

In other words, the starting positions with 2, 3, 7, 11, 17, 23... stones are wins for the first player, and the rest are wins for the second player. The sequence 2, 3, 7, 11, 17, 23... corresponds to the positions of the 0s in the sequence above, except we've added 2 to each position.

So if we were to, say, [pit all the five non-trivial solutions against each other]() in all possible combinations and with a number of different values of `$N`, we could pick up the places where the algorithms *deviate* from perfect play.

And they all deviate from perfect play: moritz's first and second algorithm, fox's agorithm, colomon's first algorithm...

Only colomon's second algorithm makes it through unscathed. Determined to find a hole in it, I'm bashed on it with various automated tools until it cracked, which it finally did at `$N` = 56. I'm... reluctantly impressed.

I find it hard to summarize the contributions to this task. I think the closest I get right now is this: the two players who emerge on top are moritz and colomon. moritz settles on elegance/maintainability, while colomon drills into the problem with unmatched vigor, and comes out solving the problem the furthest. Both of these approaches score points in my book (albeit in different columns, and possibly different amounts, too).

So, I'm more undecided than I thought I would be. I guess we'll just have to wait and see how the contestants do on the last task, and *then* weigh it all together.
