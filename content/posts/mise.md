---
title: "The tools that I love: mise-en-place"
author: Karl Yngve Lervåg
draft: true
---

> In this series, I will briefly talk about tools that I love and that I use often.
> I will keep things brief, as there is already a lot of good content that goes into more depth.

[mise-en-place](https://mise.jdx.dev/), or `mise` for short, is a tool for setting up well-defined development environments.
We specify which tools or dependencies we want in a file inside some project.
Then, when I work on that project, `mise` ensures that I have the specified tools at the specified versions available.

For example, let's say I am working on a project where I want Python 3.12 and Java 21.
I could define a file `mise.toml` at the root directory of my project with this content:

```toml
[tools]
java = '21.0'
python = '3.12'
```

`mise` will now ensure that I have these exact versions on my `PATH` whenever I work on that project.
It is really _that_ simple.

---

Around 2010 I learned about the concept of reproducible builds.
I was a PhD candidate at the time, and I was learning to write good Makefiles and shell scripts (or at least what I deemed good at the time).
With time, I noticed things like [Nix](https://nix.dev/) and [Guix](https://guix.gnu.org/) and took note.
These things really intriguied me, but I never took the time to learn how to use either of them.

Some years later, I learned about [`asdf`](https://asdf-vm.com/), "the Multiple Runtime Version Manager".
I was hooked; it was easy to learn and immediately very useful.
I also learned about [direnv](https://direnv.net/), and the two worked excellent together.

I still felt I should take the time to learn Nix or Guix, but the pull was less strong.
With `asdf` and `direnv` I had projects set up with their specific tool and environment dependencies defined in two simple files: `.tool-versions` and `.envrc`.
Even though I didn't get any colleagues to use this at the time, it was still very useful personally.
I could have different versions readily available for different projects, and setting up new systems and computers became much easier.

Getting this to work well was simple.
But not trivial; I remember having to understand the concept of shims.
A few times, things didn't work as I expected, and I found it hard to debug properly.
Still, it worked nicely and I used it with joy.



