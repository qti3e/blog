+++
title = "Per repository history in zsh"
description = "Storing per repository history in your zsh shell"
date = 2023-03-22
tags = ["zsh", "linux", "shell"]
+++

A month or two ago, I was working on multiple repositories at once and switching between the repositories way too often,
and this was starting to get annoying... every time I relied on my bash history to do something specific to that repository,
my shell activity in other project was getting in the way.

To solve this problem, I decided to take a radical approach. What if I was only storing my shell history per repository,
not having access to global history, and only restricting myself to the commands I've already executed in my current repository.

<!-- more -->

At first, I thought it was a bad idea, and I was scared, but I decided to do it anyway and see how it turned out.
And I feel no regret in making this decision; now, every time I switch between projects, I can see my shell history for that project,
see the last steps I had taken, and have an idea of what I was doing.


I added the following script to my `~/.zshrc` file:

```sh

function _better_history_hook() {
  # echo "Better history at work!"

  local BETTER_ROOT=$(pwd)
  if GPATH=`git rev-parse --show-toplevel --quiet 2>/dev/null`; then
    # echo "repo: $GPATH"
    BETTER_ROOT=$GPATH
  else
    # echo "not a repo"
  fi

  local BETTER_DIR="$HOME/.history$BETTER_ROOT"
  local BETTER_FILE="$BETTER_DIR/history"

  # make sure the directory exists.
  mkdir -p $BETTER_DIR
  export HISTFILE=$BETTER_FILE

  #discard previous directory's history
  local original_histsize=$HISTSIZE
  export HISTSIZE=0
  export HISTSIZE=$original_histsize

  #read history in new file
  if [[ -e $BETTER_FILE ]]; then
    fc -R $BETTER_FILE
  fi
}
add-zsh-hook chpwd _better_history_hook
_better_history_hook

```

This code uses the zsh hook to listen for whenever the cwd changes, for example when you start the terminal
or `cd` to a directory. And then changes the history file (default to `$HOME/.zsh_history`) to a file specific
to that repository or the `cwd` if the `cwd` is not within a git repository.


To do this, we first run a git command to get the root of the current repository.
This command returns the most top-level directory within a git repository.


```sh
git rev-parse --show-toplevel --quiet 2>/dev/null
```

It then changes the history file to a file for that directory. I decided to store the history in a `$HOME/.history`
directory to do this. After using my terminal for a while, this is how my `.history` directory looks like:


```
> tree ~/.history
/Users/parsa/.history
├── Users
│   ├── history
│   └── parsa
│       ├── Library
│       │   └── Messages
│       │       └── history
│       ├── Projects
│       │   ├── Crypto-Research
│       │   │   └── history
│       │   ├── aquascope
│       │   │   └── history
│       │   ├── benchmarks
│       │   │   └── history
│       │   ├── blog
│       │   │   └── history
│       │   ├── cargo
│       │   │   └── history
│       │   ├── tokio
│       │   │   └── history
│       │   ├── ursa
│       │   │   └── history
│       │   ├── ursa-fresh-clone
│       │   │   └── history
│       │   ├── ursa-hack
│       │   │   └── history
│       │   ├── ursa-hack-make-edited
│       │   │   └── history
│       │   ├── ursa-hack-make-edited-2
│       │   │   └── history
│       │   ├── ursa-hack-no-make
│       │   │   └── history
│       │   ├── ursa-make
│       │   │   └── history
│       │   ├── ursa-nodes
│       │   │   ├── history
│       │   │   ├── node-01
│       │   │   │   └── history
│       │   │   ├── node-02
│       │   │   │   └── history
│       │   │   └── node-03
│       │   │       └── history
│       │   └── ....
│       ├── history
│       └── notes
│           ├── Work
│           │   └── history
│           └── history
├── private
│   └── tmp
│       └── cli-space-invaders-rust
│           └── history
└── tmp
    └── history

54 directories, 49 files
```

If you like content like this don't forget to follow me on [Twitter](https://twitter.com/ParsaIsBack) :)
