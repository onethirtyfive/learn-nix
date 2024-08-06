# Getting Going

## Kicking the Tires

Fire up a **new** terminal. Let's see what version of Nix was installed.
Any modern version (>=2.18) should do for us:

```console
# nix --version
nix (Nix) 2.22.1
```

Note: omit `#` from commands you enter--it represents a prompt!

## Enabling Nix Features

Let's edit your user-level nix--as opposed to system-wide--config to enable
some "experimental" Nix features. This label is a holdover: the Nix ecosystem
depends on these features now, and they will be defaults by the end of 2024.
Let's ensure the correct nix config directory for your user exists:

```console
# mkdir -p ~/.config/nix
```

There are notable advantages to using an in-terminal editor for editing config
files. GUIs can affect file settings in weird ways. I've also had problems with
`nano` and line endings, so I'll guide you through using neovim. Fear not: for
the most part, you can do these lessons in whatever editor you want.

💫 Take 5 minutes to learn just enough to _edit_ and _exit_ in vim.

One of Nix's best tricks is the ability to make on-demand shells with any
software you want, in one command. These packages come with their entire
dependency tree, all the way down to system libraries, which is called a
"closure". Everything Nix builds goes in the Nix store, at `/nix/store`.

Let's make a shell which has neovim. As a shortcut, I'll have it start the
editor for us immediately. Nix downloads just enough, and starts the editor:

```console
# nix-shell -p neovim --command "nvim ~/.config/nix/nix.conf"
```

Once in, type `i` to go into "insert" mode. Now you can type and edit as you'd
expect. Paste or type in this line, checking for correctness:

```ini
extra-experimental-features = nix-command flakes
```

Congratulations, fellow nerd. You just enabled:

- the modern version of the `nix` command, for your user
- access to Nix "flakes", which I'm excited to get you excited about soon

`escape` returns you to "normal" mode. Type `:wq` to write and quit. Neovim
exits and you're back in your own shell. Neat.

<details>
  <summary><str>For all the <em>fun</em> software...</str></summary>
  <p></pNix>Nix disallows proprietary ("unfree") software by default, so if you want
  access to software like Discord or Google Chrome, do this once:</p>
  <pre># mkdir -p ~/.config/nixpkgs
# cat > ~/.config/nixpkgs/config.nix<< EOF
{
  allowUnfree = true;
}
EOF</pre>
</details>

## The Modern `nix` Command

Let's repeat what we just did with the modern `nix` command, now available:

```console
# nix run 'nixpkgs#neovim' -- nvim ~/.config/nix/nix.conf
```

You should see that line you pasted last time. Exit with `:q!`.

💫 You can ask `nix` command for `--help` on it or on any of its subcommands.

<details>
<summary><str>Legacy Nix commands...</str></summary>
<p>Any command which starts with <code>nix-</code> is a legacy Nix command.
Examples include <code>nix-shell</code>, <code>nix-store</code>, <code>nix-repl</code>,
<code>nix-collect-garbage</code>, and more. From time to time, they still have
their uses. Nix devs made the common decision to move distinct programs into
one top-level command with subcommands, like how <code>docker-compose</code> became
<code>docker compose</code>.</p>
</details>

## One Shell To Bind Them All...

How about a bit more with on-demand shells? Let's use the old world `nix-shell`
command for its ability to make "pure" shells. Here, "pure" means absolutely
nothing "leaks in" from your system. _Nothing_ unspecified will be available.

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

Here's the equivalent in the modern CLI. It can't quite make pure shells like
the old-world command--your user settings and configuration will leak in.
Notice how the appearance of your prompt doesn't change this time? That's why.

```console
# nix shell 'nixpkgs#nodejs' 'nixpkgs#python3' 'nixpkgs#ruby'
[ ...noodle around if you like... ]
# exit
```

You just installed `ruby`, `python3`, and `nodejs` into a temporary shell.
Outside that shell, the packages remain installed but out of view.
Let's "garbage collect" ("gc") your system to keep things tidy:

```console
# nix store gc -v
[ ... it shows what it's deleting because of the `-v` ... ]
72 store paths deleted, 411.24 MiB freed
```

## On Uniqueness

Nix will only ever build anything once for any set of inputs. What does that
mean? In plain English, something like this:

> Please build `python311`, with no tweaks, for `aarch64-darwin` (macOS, apple
> silicon), where `nixpkgs` is at git rev `dd868b7`

If any of those parameters changes, Nix rebuilds only the items in the
closure--the tree of exact dependencies needed--which need rebuilding.
Installing software is as simple as copying a closure. _Neat_.

## Funsies

- Look for packages on [search.nixos.org](https://search.nix.org). Be sure
  you're searching for _packages_, not NixOS options or public flakes. Make an
  on-deand shell with something interesting. Be sure to `exit` when done.
- What other commands does the modern CLI offer? Never forget, there's always
  `--help`! (when viewing help, `space` goes down a page and `q` exits)

## Recap

1. User-level Nix config is located at `~/.config/nix/nix.conf`. Consider editing
   configuration files with in-terminal editors like `nvim`.
1. Packages installed into the read-only Nix store stick around until gc'ed.
1. You can make on-demand shells with any packages you want and it won't affect
   the rest of your system system.
