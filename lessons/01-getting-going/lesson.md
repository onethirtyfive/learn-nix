# Getting Going

## Kicking the Tires

Fire up a **new** terminal. Let's see what the Nix installer installed.
Any modern version (>=2.18) should do for us:

```console
# nix --version
nix (Nix) 2.22.1
```

Note: omit `#` from commands you enter--it represents a prompt!

## Enabling Nix Features

Let's edit your user-level nix configuration to enable some "experimental"
features. This designation is vestigial: the Nix ecosystem depends on these
features now, and they will be defaults by the end of 2024. Let's ensure the
correct nix config directory for your user exists:

```console
# mkdir -p ~/.config/nix
```

There are notable advantages to using an in-terminal editor for editing system-
level files. GUIs can affect file permissions in weird ways. I've also had
problems with `nano` and line endings, so we'll use neovim. Take a few minutes
to learn enough to edit and exit an editor of your liking. It's worth it.

One of Nix's best tricks is the ability to make _ad hoc_ (just-in-time) shells
with only the packages you want. These packages are wrapped together with _all_
dependencies, recursively, in a bubble called "closure". Nix packages don't
dependencies with other packages--everything is baked in, for every package!
Surprisingly, this does not lead to bloat, because two packages truly depending
on the same version will point to the same place.

Run this command. You'll see Nix download neovim and its deps, then start:

```console
# nix-shell -p neovim --command "nvim ~/.config/nix/nix.conf"
```

Once in neovim, type `i` to go into insert mode. Now you can type and edit
as you'd expect. Ensure this line is present and correct (paste it!):

```ini
extra-experimental-features = nix-command flakes
```

Great, now your installation has:

- the modern version of the `nix` command
- access to Nix "flakes", which I'm excited to introduce soon

Hit the escape key to go back to normal mode, then type `:wq` to write and quit
the editor. Neovim exits and drops you back into your terminal. Neat.

## Using the Modern `nix` command

Let's use the newly available `nix` command to what we just did again
No changes made, so `:q!` will get you out of the editor:

```console
# nix run 'nixpkgs#neovim' -- nvim ~/.config/nix/nix.conf
```

You can ask `nix` command for `--help` on it or on any of its subcommands.

Nix will only ever build things once for any set of inputs. What does that mean?
In plain English, something like this:

> Please build `python311`, with no tweaks, for `aarch64-darwin` (mac, apple
> silicon), where `nixpkgs` is at git rev `dd868b7`

You can quite literally _prebuild_ that version and copy closures between nix
stores and both hosts will have that exact version of software.

Let's "garbage collect" ("gc") your system to keep things tidy:

```console
# nix store gc -v
72 store paths deleted, 411.24 MiB freed
```

<details>
   <summary><str>On legacy `nix-` commands...</str></summary>
You should know: any nix command which starts with `nix-` is from the old
world. (Examples: `nix-shell`, `nix-store`, `nix-repl`, `nix-collect-garbage`,
and more.) From time to time, they still have their uses. This happens often:
`docker-compose` used to be a standalone tool; now, it's `docker compose`,
where `compose` is a subcommand in the suite. Nix adopts this  approach for
modern commands: `nix` has a suite of tidily organized subcommands.
</details>

## More on Ad-Hoc Shells

Let's use the old world `nix-shell` command for its ability to make "pure"
temporary shells. Here, `--pure` means "only what has been specified is
available, absolutely nothing from the system":

```console
# nix-shell --pure -p nodejs python3 ruby
# node --version
[ see for yourself ]
# python --version
[ see for yourself ]
# ruby --version
[ see for yourself ]
# which ruby
[ `which` is on all unix-like systems, but not in this pure shell! ]
# exit
```

Here's the equivalent in the modern CLI. It can't make pure shells like the
old-world command--your user settings and configuration will leak in. Notice
how the command prompt doesn't change this time? That's why.

```console
# nix shell 'nixpkgs#nodejs' 'nixpkgs#python3' 'nixpkgs#ruby'
[ ...noodle around if you like... ]
# exit
```

You just installed `ruby`, `python3`, and `nodejs` into a temporary shell.
utside that shell, the packages remain installed but unusable.

## Funsies

- Look for packages on [https://search.nixos.org](https://search.nix.org)--be
  sure you're searching for _packages_ and not NixOS _options_. Make an ad hoc
  shell with something interesting. Don't forget to `exit` when you're done.
- What other commands does the modern CLI offer? You can see with a bit of
  advice above. (once you set it: `space` goes down a page and `q` exits)

## Recap

1. User-level Nix config is located at `~/.config/nix/nix.conf`. Consider editing
   configuration files with in-terminal editors like `nvim`.
1. The modern version of the CLI requires enabling, but represents the future
   of the package manager. We'll focus on this version for most things, but
   sometimes the old commands provide specific capabilities we need.
1. Packages installed into the read-only Nix store stick stay there 'til gc'ed.
1. You can make temporary (sometimes called _ad hoc_) shells with temporarily
   available packages and it won't affect your system after you exit the shell.
1. Flakes, which we'll learn about soon, are central to modern Nix. But we have
   a thing or two to learn before they'll make sense.
