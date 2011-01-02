---
title: November 1 2008 &#8212; the .end is here
author: Carl Mäsak
created: 2008-11-01T19:42:00+01:00
---
56 years ago today, the 10-megaton hydrogen bomb [Mike](http://en.wikipedia.org/wiki/Ivy_Mike) detonated in the Pacific Ocean in the US-conducted [Operation Ivy](http://en.wikipedia.org/wiki/Operation_Ivy). The Wikipedia article about Mike says:

<blockquote><div><p>Edward Teller, who was perhaps the most ardent supporter of the development of the hydrogen bomb, was in Berkeley, California, at the time of the shot. He was able to receive first notice that the test was successful by observing a seismometer, which picked up the shock wave travelling through the earth all the way from the South Pacific test site. In his memoirs, Teller wrote that he immediately sent an unclassified telegram to his colleagues at Los Alamos which contained only the words&#8212;for security purposes&#8212;"It's a boy."</p></div></blockquote>

Today's humble contribution to the furthering of all things November was to implement the `.end` method on lists. The `@array.end` syntax is supposed to be one of the replacements for the familiar Perl 5 `$array[-1]` syntax (the other being `@array[*-1]`, which does not yet work in Rakudo).

I haven't gone through the p6w source code to check if it could be used anywhere, but this is one of those small methods that will be taken for granted once Rakudo Perl 6 is in wider use.


