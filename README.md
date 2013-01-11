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

The repo contains an interpreter written in Perl and a few interesting programs.

## Example

```
# hello, world program

# This will delete itself when run
world := ""

# This will run first and rename itself to "hello, world"
hello := "hello, " ".!" "" ? ~          # grabs "world" using a regex
```

First, the expressions (`hello` and `world`) are sorted by name. `hello` comes first, so its right-hand side is evaluated. Separating it out to make it more understandable:

* `"hello, "` pushes the string "hello, " onto the stack.
* `".!" ""` pushes these two strings onto the stack.
* `?` pops the previous two strings and does regex matching. In this case `".!"` is the regex, matching any character repeated one or more times, and `""` means to search the names of all expressions other than the one currently being evaluated. In this case the regex matches `"world"`, the name of the other expression. `"world"` is therefore pushed onto the stack.
* `~` pops the two strings on the stack, `"hello, "` and `"world"`, concatenates them, and pushes `"hello, world"` back.

Now the expression evaluation is done. `hello` evaluated to the string `hello, world`, so the expression is renamed that. The program now consists of two expressions, `hello, world` and `world`. The sort order hasn't changed. Execution goes to the next expression, `world`.

All `world` does is push the empty string, `""`, to the stack. It therefore evaluates to the empty string. But as a special case, an expression that evaluates to the empty string isn't renamed, it's deleted. So `world` is deleted.

A Sortle program terminates when only one expression remains, printing that expression's name as output. Since `world` deleted itself, we now have only one expression left. Its name is `hello, world`, so that is printed and the program ends.

### A slightly different example

```
hello := "hello, world"
world := "hello, world"
```

This one illustrates **clobbering**. First `hello` renames itself `hello, world`, but then `world` *also* renames itself `hello, world`, clobbering the existing expression with that name. Now there is only one `hello, world`, its name is printed, and the program ends.

## Sortle is Turing-complete!

When I first invented the language, I couldn't figure out how to do much with it. I didn't know what [computational class](http://esolangs.org/wiki/Computational_class) it was in, but assumed it was probably a [finite-state automaton](http://esolangs.org/wiki/Finite-state_automaton), where the amount of data you could access was limited by program size (like [SMETANA](http://esolangs.org/wiki/SMETANA)).

I was wrong! In 2012, I took another look and managed to show that Sortle is, in fact, [Turing-complete](https://en.wikipedia.org/wiki/Turing_completeness). This stems from the fact that, with the “one or more” modifier `!`, you can use regexes to capture an arbitrary amount of data stored in another expression's name. My somewhat less-than-formal proof consists of implementing the Turing-complete language [Bitwise Cyclic Tag](http://esolangs.org/wiki/Bitwise_Cyclic_Tag) in Sortle. You can find that implementation in bct.sort.

Afterwards, feeling saucy, I even wrote a quine, a program that prints its own source code. That's quine.sort. It took a bit of hacking to get it to work.

## Reimplementing Sortle

Before I could get these results, I had to write a new interpreter in Perl
because my old one written in C was buggy and fragile. Its regexes didn't do what they were supposed to. The language implemented by my old C interpreter was not Turing-complete at all.

Note that because I chose to create a very limited type of “regex” for Sortle, with nonstandard modifiers, I didn't get to pass Sortle's regexes directly into Perl. The matching routines are custom.

## More info

For a fuller exposé, see [the Esolang wiki's article on Sortle](http://esolangs.org/wiki/Sortle) or my old and slightly embarrassing (but complete) [language spec](http://esoteric.voxelperfect.net/files/sortle/doc/sortle.pdf). Happy hacking!