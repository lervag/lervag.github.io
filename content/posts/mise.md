---
title: "The tools that I love: mise-en-place"
author: "Karl Yngve Lervåg"
date: 2025-01-22
build:
    list: never
    publishResources: false
    render: always
---

> In this series, I will briefly talk about tools that I love and that I use often.
> I will keep things brief, as there is already a lot of good content that goes into more depth.

[mise-en-place](https://mise.jdx.dev/), or `mise` for short, is a tool for setting up well-defined development environments.
To use ut, I simply specify which tools or dependencies I want in a file named `mise.toml` inside a project.
Then, when I work on that project, `mise` automatically ensures that I have the specified tools at the specified versions available.

For example, let's say I am working on a project where I want Python 3.12, Java 21, Neovim 0.5[^1] and [Hurl](https://hurl.dev).
I could define `mise.toml` at the root directory of my project with this content:

```toml
[tools]
java = '21.0'
python = '3.12'
neovim = '0.5'
hurl = 'latest'
```

`mise` will now ensure that I have these exact versions on my `PATH` whenever I work on that project.
In my experience, it is beautiful in its simplicity, and it just works™!

Here's a short list of reasons why it's on my "love" list:

* The _"it just works"_ factor is very high!
* The documentation is great!
* It is very easy to on-board friends and colleagues, as long as they use Linux or
MacOS[^2].
    I only need about 5-10 minutes to get people started.
    And the good documentation is really helpful in this regard!
* It is easy and convenient to use `mise` in pipelines in both GitLab and Github.

## Addendum

Before `mise` (and `rtx` as it was named in the early days), I used [`asdf`](https://asdf-vm.com/), "the Multiple Runtime Version Manager".
I combined `asdf` with [direnv](https://direnv.net/), and the two worked excellent together.
To my knowledge, `mise` started out as a sort of "rewrite" of `asdf`.
It is compatible with the `.tool-versions` specification files used by `asdf`.

Today, I think `mise` has grown into something much better and more robust.
It easily replaces both `asdf` and `direnv`.

I should also mention that I am aware of and quite intrigued by both [Nix](https://nix.dev/) and [Guix](https://guix.gnu.org/).
However, both of these require me to learn much more before they become useful.
I believe it would also be much harder to on-board colleagues and friends.
As such, I find `mise` really hits the sweet spot with regard to its simplicity and convenience!

[^1]: For no sane reason. Clearly, we want the [Neovim nightly release](https://github.com/neovim/neovim/releases)!
[^2]: `mise` [does have some Windows support](https://mise.jdx.dev/faq.html#windows-support), but it's not as smooth.
