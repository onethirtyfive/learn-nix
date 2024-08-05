# Everything is an Expression

## Two Types of Code

In the vast world of programming, there are two mutually exclusive types of
code: _expressions_ and _statements_. Expressions are combinations of literal
values, operators, function calls which, together, result in a value.
Statements are anything else.

💫 There's a [tidbit](../../tidbits/B_statements-and-expressions.md) with
details. You should probably read it before continuing.

## OOPS! All expressions!

Imagine a language with only expressions.

It has literal values like strings, numbers, some basic keywords like
`if`/`else`, collections like arrays and dictionaries. Such a language would
provide syntax for writing and combining expressions, and an interpreter
for evaluating them.

Add a _further_ crazypants constraint: values, once set, cannot change. This
makes sense: if functions are expressions and must always _return_ the same
value, and functions are "just" values, then they can't change! The same
applies to literal non-function values, naturally.

These constraints seem impractical until something clicks, but for that to
happen, I first need to explain _lazy evaluation_. That's next lesson.

## Expressions for Fun and Profit

Let's cut to the chase and evaluate some expressions. Along the way, we'll
learn the Nix expression language (which I will call `Nix`, for short--see,
it's a mystical trinity!)

Fire up a terminal, let's evaluate some Nix expressions:

```console
# nix eval --expr '"Hello"'
"Hello"
```

You did it, you had Nix evaluate an expression. That's all you'll *ever* do
with this language! Ask for something, and if it's evaluable, get a value.
The difference will be the sophistication and organization of expressions.

Fun. Let's do more:

```console
# nix eval --expr '2 + 3'
5

# nix eval --expr '{ a = 1; b = "Hello"; }'
{ a = 1; b = "Hello"; }

# nix eval --expr '{ a = 1; b = "Hello"; }.b'
"Hello"

# nix eval --expr '[ 1 2 3 ]'
[ 1 2 3 ]

# nix eval --expr 'null'
null

# nix eval --expr 'if 3 > 2 then "duh" else "multiverse is real"'
"duh"

# nix eval --expr '[ (a: b: a + b) (a: b: a * b) ]'
[ «lambda @ «string»:1:4» «lambda @ «string»:1:18» ]
```

The most alien thing about above is the attrset (aka hash/dict/object) syntax,
but it's fine! Just remember to put semicolons after each key-value pair.
The second most alien thing: array elements are separated only by whitespace!

## Gird Yourself: Here Come the Functions

Functions are a deeper dive. Nix has a *singularly* terse syntax for functions:

```console
# nix eval --expr 'a: b: a + b'
«lambda @ «string»:1:1»
```

Let's break down the syntax: we defined a function which has two parameters,
`a` and `b`, which returns their sum. Once you get used to it, other languages
seem downright _chatty_. This terse syntax is--I promise--quite nice!

But the evalution result, what the hell? Well: it's a lambda sourced from row
1, col 1 of the string you evaluated. Recall: functions are values, so you can
just _have_ one. That's the expression, all done! (narrator: "_they weren't_.")

Let's _call_ the function:

```console
# nix eval --expr '(a: b: a + b) 2 3'
5
```

What?! Well, it turns out calling functions is similarly terse. If you refer to
a function by name (or definition), you call it by providing arguments
separated by spaces. _Terse_. We defined the function, put this definition in
parens to signal "this is a whole thing", and called it with two arguments.

## Partial Application

I'll leave you with a cliffhanger. What is going on here?

```console
# nix eval --expr '(a: b: a + b) 2'
«lambda @ «string»:1:5»
```

## Funsies

- Eager beavers: what other operators hath Nix? What does `//` do? `++`?
  Can you evaluate an expression using them using `nix eval`?
- How can we print the result in JSON? (hint: use `--help`)

## Recap

1. The Nix Expression Language has some severe constraints, by design.
   Values are immutable (can't change) and _everything_ is an expression.
   These end up enabling some amazing functionality, so keep going.
1. Nix attrsets are the language's dict/hash/object. Key value pairs are
   always separated by a semicolon. Array items are separated by whitespace.
1. Expressions are _terse_, which is hard at first but _amazing_ later.
1. Something interesting happens when you call a function with fewer arguments
   than it wants...

