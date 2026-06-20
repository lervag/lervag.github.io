---
title: 'VimTeX: a history and some reflections'
author: Karl Yngve Lervåg
date: 2026-06-20
---

I want to give a brief history of how VimTeX came to be, partly based on [an interview I did back in 2016](http://howivim.com/2016/karl-yngve-lervag/).
I'll also share some thoughts and reflections on VimTeX, what it has meant to me, and on the future.

[VimTeX](https://github.com/lervag/vimtex) is a plugin for Vim and Neovim for writing documents with [LaTeX](https://www.latex-project.org/).
And for those who might not know, LaTeX is a system for typesetting beautiful documents[^1].
It is very much favored by mathematicians and physicists, in part because it makes equations easy to write.
I've personally used LaTeX quite a bit, for writing papers, reports, and many other things.

{{< figure
  src="/static/vimtex-screenshot.png"
  caption="Editing a LaTeX document with VimTeX: source on the left, the typeset PDF on the right."
>}}

## How it all started

Well before VimTeX ever existed, I was a happy Vim user.
My first experience with Vim was in 2003 when I started University.
And I was also introduced to LaTeX around that same time.
I was writing LaTeX documents with Vim and was mostly running LaTeX commands (e.g. `pdflatex` and `bibtex`) in my terminal.

At some point, I installed and configured [LaTeX-suite](https://vim-latex.sourceforge.net/).
Although it sort of worked fine, I was never quite happy with it.
It felt like it tried to do too much, and the configuration was not very intuitive to me.
It also lacked some core Vim concepts like text objects and a few motions.

Later on, I believe around 2008, I discovered a plugin called [LaTeX-Box](https://github.com/LaTeX-Box-Team/LaTeX-Box), which aimed to be simpler and more lightweight.
It worked quite well and had some of the features I was missing from LaTeX-suite.
After some time, I started to contribute to it.
However, I remember I found it was difficult to solve issues, and my feeling was that the code structure was not very well thought out.
Being quite naïve, I thought to myself that it shouldn't be too hard to rewrite everything and create a new plugin that was _actually_ lightweight and simple.
So, on a plane trip to Iceland in September 2013, I started writing VimTeX.

The first couple of years, VimTeX was called vim-latex.
However, that name was already taken: LaTeX-suite was also known as vim-latex, which is evident e.g. from the project URL.
When I was made aware of this, I ended up changing the name to VimTeX after a short discussion[^2].
And so since around March 2015, the plugin has been known as VimTeX.

## Reflections

The core idea behind VimTeX was to create a simple, lightweight plugin with a well thought-out code structure.
I remember spending quite a lot of time thinking about how to structure things.
I wanted a modular structure where I avoided using any state unless necessary.
But I did need state to track things like which file is the main file in a big LaTeX project.
So for stateful things, I relied on modern Vimscript features to get a quasi object-oriented project state.
Combined, these things enabled me to quite efficiently add new features requested by users.
The structure helped avoid high-impact bugs.
And a good test framework made the bugs that did appear quick to fix.

Even though there are things I see today that I should have done differently, I think the plugin and my work on it has been a success.
It more or less "just works" on most systems, and it has support for most or all features you would expect for a filetype plugin in Vim.
I have to say that, even though I have mainly developed VimTeX all by myself, it would NOT have been possible without the help from users who open issues and help solve them.
I feel lucky in that there are many users who take their time to write good and thoughtful bug reports and feature requests.

I have to admit that it is probably not fair to call VimTeX lightweight any longer:
the core VimTeX code spans about 200 files of Vimscript code with about 23 000 lines of code.
And there's almost 7500 lines of documentation.
I would still claim that the code is well structured and relatively untangled, and it is not too hard to maintain.
Here are some numbers fresh from today:

```
> tokei --sort lines --exclude test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Language              Files        Lines         Code     Comments       Blanks
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Vim Script              194        26212        22927         1235         2050
Plain Text                1         7453            0         5978         1475
JSON                      2         1462         1462            0            0
Lua                       7         1309          839          280          190
Python                    4          141          107            3           31
Dockerfile                1           36           17           13            6
TOML                      1           33           32            0            1
TeX                       1           26           22            1            3
─────────────────────────────────────────────────────────────────────────────────
Markdown                  6         1267            0          987          280
|- Lua                    1            9            7            2            0
|- TeX                    1            3            3            0            0
(Total)                             1279           10          989          280
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total                   217        37951        25416         8499         4036
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

VimTeX has been a very useful project for me personally.
It has allowed me to learn about all sorts of things!
For instance, I've learned a lot about programming and software development, how to write good tests, CI pipelines, documentation, and more.
I've learned about how to manage a project and how to communicate with users, how to be patient and how to follow-up in complicated issue threads where things can become unclear and sometimes frustrating.

I think it is fair to say that my work with VimTeX has been one of the main reasons I could successfully switch my career from researcher in applied physics to software developer.
And since I'm very happy with my career change, it also makes me very grateful for how something that has been a fun hobby to me has provided real value as well.

Regarding the future of VimTeX, I don't quite know what it brings.
I believe the plugin today serves most users well.
And based on the star history on GitHub, it seems there are still new people using 

{{< figure
  src="https://api.star-history.com/svg?repos=lervag/vimtex&type=Date"
  link="https://star-history.com/#lervag/vimtex&Date"
  class="img-center"
>}}

However, since I left academia in 2022, I don't really use VimTeX much myself.
This makes it less motivating for me to work on it, unfortunately.
I still try to address new issues, but I find that I am more reluctant to spend time on new features and general improvements.
And to be honest, I would probably not mind if anyone wanted to adopt the project[^3].

[^1]: I'm aware that a lot of people are now looking towards [Typst](https://typst.app/) as a modern alternative to LaTeX.
It seems to be going in the right direction, but it will likely take a long time to fully replace LaTeX.

[^2]: https://github.com/lervag/vimtex/issues/129

[^3]: Don't worry, I won't stop maintaining it unless I should have a serious incident or unless someone else shows up and adopts it.
