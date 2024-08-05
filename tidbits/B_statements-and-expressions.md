# More Definitions

A bit on the only two kinds of code in existence.

## Just Enough: Expressions

You already know expressions! Here are some:

- literal values: `"Joshua"`, `-3`, `{ "key": 42 }`, `null`
- pure arithmetic: `2 + 4`, `9 % 2`
- any function which always returns the same value for the same inputs:
  `(a, b) => a + b`; `() => null`
- return values: `add_two_numbers(2, 4)`
- any `if` with an `else` clause. python has `a if b else c`; many languages
  have ternaries like `a ? b : c`, which are equivalent

Any combination of expressions is also an expression:

- `2 / add_two_numbers(2,4)`
- `"have it " + "your way"`
- `[3, 4]` + `[1,2].reduce((x) => x * 2)`
- `(x) => ((y) => x + y)`
- `if (x % 3 == 0) then "divisible" else "indivisible"`

If it evaluates to a stable value, it's an expression!

Easy constraints apply: to be expressions, functions must always return the
same value for the same inputs. `if` statements must have an `else`.

## Just Enough: Statements

Assignments are statements: `let a = 3;`

Statements can _involve_ expressions; above, the right-hand `3` is an
expression. But it is part of a statement, which is more important.

Loops are statements which typically contain more statements having an
effect on something repeatedly. In this case, an effect on `a` (and on the
user's screen, by printing!):

```
let file = open('numbers.txt');
let a = 3;
let items = [1,2,3];

for item in items {
  a = a + item;
  print(a)
}
```

Statements are essential in languages which execute sequences of instructions
for broad effects on volatile inputs like filesystems and networks. They often
change ("mutate") data, and try to wrangle all possible states with logic.
This mainstream mode of coding is called _imperative programming_.

## Expression Languages and Functional Progamming

There exists an entire set of languages which have no statements called
_expression languages_. These language are a subset of _functional programming_,
and the Nix Expression Language is one such expression language.

It is well suited only to defining packages in the Nix universe. But, as you'll
see, it's very good at it. The nix package manager uses these values to do its
work--the work of *realizing* (building) packages for any supported system.
