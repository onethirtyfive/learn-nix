# Learn Nix

It took seven years of casual interest before I got to a point where I could
use Nix professionally. Not because it _had_ to, and not because I'm a bad dev.
What I needed, and hope to provide here, is value propositions and explanations
suitable for busy normie devs like myself, to help close that motivation gap.
Learning should be fun, with quick skill confirmation. Elitism is for haters.

**Every document in this repo--present one included--is limited to 100 lines of
80-char-width, non-code text out of respect for your time. I hope this brevity
is a gift to you, and hope it helps you learn Nix quickly.**

After reading this doc, start with the lessons; then, use the the katas to make
your new skills muscle memory.

## About Nix

What is one thing and three things at the same time? The Holy Trinity? Yes!
Also, Nix, which is a comparably mystical triad of three technologies:

- the biggest, freshest [package universe](https://github.com/NixOS/nixpkgs/),
  with definitions for 100k+ packages
- a programmable [expression
  language](https://en.wikipedia.org/wiki/Expression_language) well-suited for
  software packaging
- a system which builds packages reproducibly from source or (even
  better) substitutes a verified, signed binary from a trusted remote, saving
  you large amounts of time

Here are some material benefits--real value, as in time/money--of using Nix:

- customizable software distributions for all mainstream languages; faster dev
  cycles and spinup of new products
- reproducible, heuristically layered OCI/Docker images with a function call,
  for drop-in integration into existing non-Nix build systems
- reproducible devenvs for less "worked on my machine"; containers optional
- profound savings not paying engineers to _wait_: Nix rebuilds only modified
  packages and their referers; no more "rebuilding the world every time" in CI
- no reliance on upstreams definitions after first build; offline rebuilds free
- native builds out of box for all common systems; cross-compliation available
- free partial immunity to supply chain attacks; total immunity with low effort
- proprietary software support with a bit of configuration

That's a lot of profit for a bit of learning.

## Who Loves Nix?

Nix is an inkblot test. Dr. Scrappydev will diagnose you if you talk about
purity; Dr. Haskell will diagnose you unless you see lambda calculus. In the
real world, early Nix adopters have been seasoned devs who prefer:

- [_simpler_ work products over _easier_ work
  products](./tidbits/A_simplicity-vs-difficulty.md)
- adoption of designs forcing reckoning about failures _early_; restated, a
  preference for leveraging rather than avoiding computers' eternal pickiness
- building small irrefutable solutions, composing them into bigger irrefutable
  solutions, and being provably *done* when you've made the last needed one
- the pragmatism of _no surprises_

Often, though not necessarily, these folks favor functional programming styles,
which fit these priorities like a glove. But mainstream developers often feel
thrown into the deep end of a pool of oxygenated breathing fluid, and we need
explanations for _why_ we should learn entire concept families before we make a
"Hello, world." The constraints are all newcomers notice, but they're actually
very helpful when things click. Let me bridge that gap for you.

If you're ready to adopt a handful of new motivations and design patterns,
that's the work required for this skill. Go on: dip a toe in the pool, I hear
the breathing's good today.

## Software Requirements

This project assumes you have [installed](https://nixos.org/download/) Nix. Nix
plays well with other package managers*, and can run on most operating systems.
This project assumes comfort with a mainstream, dynamic lang like py/rb/js.

\*Nix may well clash with with assumptions of your shell environment, like
hooks and automatic behaviors sourced when the shell loads. _What did we learn?_ 💅

### Nix vs NixOS

[NixOS](https://nixos.org) is a Linux variety (a "distribution") which uses Nix
to manage system and user profiles. I love NixOS and use it daily, but you can
learn how to use Nix in the comfort of your own operating system.

## Resources

- [Nix Pills](https://nixos.org/guides/nix-pills/) - Bite-sized guides, or
  "pills"
- [@ChrisMcDonough on YouTube](https://www.youtube.com/@ChrisMcDonough/videos)
  \- Often hilarious, always informative and accessible Nix noodlings
- [nix.dev](https://nix.dev/) - References, tutorials, and more
- [Nix is a better Docker image builder than Docker's image
  builder](https://xeiaso.net/talks/2024/nix-docker-build/)
- [devenv](https://devenv.sh/) - Simple, declarative development environments
  with Nix

## Disclaimer

I have no credentials or bona fides outside of my own experience, and some
information here will inevitably be wrong. I welcome correction. Be kind.
