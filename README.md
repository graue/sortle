Sortle
======

[Sortle](http://esolangs.org/wiki/Sortle) is an
[esoteric programming language](http://esolangs.org/wiki/Esoteric_programming_language)
that I ([Scott Feeney](https://github.com/graue))
designed back in 2005 based on insertion sort, regex matching
and side-effect-free expressions that rename themselves.

This is just a geeky exercise for fun.
The language is not designed to be useful,
but rather as an intellectual challenge to try to reason in
and write programs for.

When I first invented the language, I couldn't figure out
how to do much with it. Years later, I took another look
and managed to show that Sortle is, in fact,
[Turing-complete](https://en.wikipedia.org/wiki/Turing_completeness),
and I wrote a quine, a program that prints its own source code.
Along the way, I had to write a new interpreter in Perl
because the old one written in C was buggy and fragile.

This repo contains that interpreter and a couple of examples.

See [the Esolang wiki's article on Sortle](http://esolangs.org/wiki/Sortle)
for a language overview.
