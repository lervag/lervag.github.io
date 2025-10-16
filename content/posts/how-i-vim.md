---
title: 'The tools that I love: Vim'
author: Karl Yngve Lervåg
date: 2025-10-12
---
> In this series, I will briefly talk about tools that I love and that I use often.
> I will keep things brief, as there is already a lot of good content that goes into more depth.

There is probably no single tool that I love more than Vim.
It is strange to think about how much it has ended up meaning for me.
Vim has been a part of my entire career both as a student, researcher, and a developer.
Needless to say, it is hard to sum up my thoughts and feelings about Vim in a short blog post.
So I'll allow it to be slightly longer than the average "The tools that I love"-posts.

I'll first write a little bit about what Vim is.
Then I'll try to explain why I love it.
Finally, I want to highlight some of the _phases_ I've been through as a Vim user.

## What is Vim and why do I love it

[Vim](https://www.vim.org/) is a text editor, that is, a program we use to write and edit text.
This is essentially to most of what I do on a computer, be it programming, development, and writing in general.
Vim was originally developed in 1988 by [Bram Moolenaar](https://en.wikipedia.org/wiki/Bram_Moolenaar), and he kept on improving and maintaining it until he unfortunately passed away in 2023.
There is now a community effort for maintaining Vim, although the current main maintainer is [Christian Brabandt](https://github.com/chrisbra).
I should also mention that in 2014, [Neovim](https://neovim.io/) was created as a fork of Vim.
It has a slightly different vision and goal, but is similar in spirit.
In the context of this post, Vim and Neovim can be considered to be the same thing.

Before I continue, I want to highlight a related post from 9 years ago (2016):
[I did an interview on How I Vim](http://howivim.com/2016/karl-yngve-lervag/)[^1].
A lot of what I said in that interview is now outdated, for instance:

- I'm no longer a research scientist.
- I use [Neovim](https://neovim.io/) (since around 2018, I think).
- I've rewritten [my config](https://github.com/lervag/dotnvim/) more or less from scratch at least once since then.
- I now use [wezterm](https://wezfurlong.org/wezterm) as my terminal emulator.

Here's a list of things that I _**love**_ about Vim:

- **Speed** — Vim allows me to edit text and navigate _very quickly_.
  I can write a dedicated blog post on this topic alone, and a lot of people have.
  I think the famous StackOverflow post [Your problem with Vim is that you don't grok vi](http://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118) (from 2009 by Jim Dennis) does a good job at explaining how Vim can make a user fast.
  And I love the speed, because being able to edit and navigate close to the speed of my thought helps me avoid loosing my train of thought.
- Vim is not an IDE and it does not hide complexities.
  For instance, I can't just open a project, build it and run the code with a clean Vim configuration.
  Modern IDEs often give you this.
  But I actually _like_ how Vim forces me to learn and understand how things work.
- I can customize Vim to make it work as I want it to.
  I think the term ["personalized development environment" (PDE)](https://www.youtube.com/watch?v=QMVIJhC9Veg) is very fitting!
  And for some strange reason, I _love_ to customize and optimize my development environment, both vim and related tools.
  It's hard to explan it, but it's fun and it has become a hobby.
- Writing Vim plugins was probably my first programming "achievement".
  That is, the first program I ever wrote where I found joy in _creating_ something was probably a Vim plugin.
  I can remember the feeling of finishing something that worked and that had a real impact on my day-to-day development environment.
  And I love how Vim was able to give that to me!

## My Vim journey as a series of phases

I started using Vim in 2003 as an undergraduate.
It was hard at first, but I liked the challenge and I liked how learning it made me faster at editing and working with text and code.
I quickly fell in love, and I've just kept using it since.
This means I've used Vim for about 22 years now.
When I look back, I can reflect on having been through a series of phases[^2]:

- **The Vim zealot** — I was a Vim zealot in the first few years.
  I would explain to anyone who would listen[^3] why it is better than everything else.
  I would talk warmly of it and try to persuade others to use it.
  I tried to guide and advice students and interns I mentored into using Vim.
  But to be honest, during the first years I mostly learned the basics.

- **The elitist** — After some years, I stopped actively trying to teach "the gospel".
  I would still gladly help and assist anyone who was curious (I still do!).
  To help them, I wrote a simple starter configuration that I shared with anyone who was interested[^4].
  I might characterize this as a phase of _elitism_.
  E.g., "I don't need an IDE - I have a terminal and Vim. They are far superior to any IDE!"
  Still, if I am to be kinder to myself, this was also a phase of mastery.
  I learned to write Vimscript at a high level, and it was during this phase I started to write and maintain my own plugins such as [VimTeX](https://github.com/lervag/vimtex/).
  However, I was not very open minded and was more arrogant than I should have been.

- **The curious** — I'm glad to say that this has changed.
  With age and experience I've become more open minded, I've even been curious about other tools.
  I tried to use Emacs for a few months, mostly because of [org-mode](https://orgmode.org/).
  However, I realized I was too invested in [my own tooling for note-talking](/posts/wiki.vim) and kept to Vim[^5].
  Still, I accepted that there are different strenghts and weaknesses in the large list of alternative editors and IDEs.
  I did still claim that Vim really _is_ an excellent editor, though—and I still do!

- **The pragmatic** — Today I consider myself a _pragmatic_ Vim user.
  I prefer Vim whenever it is possible and if it does not make me less efficient.
  This does hold true in most cases, but I stretch for other tools once in a while.
  For instance, I would prefer Intellij if I work with Kotlin, because the open source tooling is not good enough there.
  I also would not mind to use another editor or IDE in a pair programming session if that would be easier for the other person(s).

My Vim journey, and the related Linux and shell journey, have been very useful for my education.
Learning to use Vim, learning to _configure_ Vim, and learning to develop programs with Vim and a terminal, has taught me a lot of the skills I depend on today:

- I can effortlessly navigate code and text at very high speeds.
  I believe this is super useful to maintain focus on my tasks across code bases.
- I can adjust my configuration at my own will and at relative ease and make Vim behave differently for particular tasks if and when it suits me.
- Vim has traditionally been very reliant on regular expressions, which is often very useful to know.
- Learning to configure and write plugins has introduced a ton of problems for myself.
  But as a consequence, I've had to _fix_ my problems time and time again.
  This has made me become quite fearless of a lot of types of challenges.
- I've learned a lot about how Linux and unix works at a relatively low level.
  This is clearly tangential, but I do think it has been a consequence of the Vim journey as I've ended up always having a terminal by my side.

## Final remarks

If you read this far, then thanks for staying with me.
For my final remarks, I want to highlight some of the older writings that are still interesting today:

- [Seven habits of effective text editing](http://moolenaar.net/habits.html), Bram Moolenaar (RIP), 2000 - he clearly created a great editor.
  He also shared very interesting thoughts on learning the tools of the trade.
  This essay is interesting even if you don't use Vim.
  Remember to sharpen the saw!
- [Vim Creep](https://rudism.com/vim-creep/), Rudis Muiznieks, 2011 - A nice inspiring story about Vim.
- [Vim Kōans](https://sanctum.geek.nz/arabesque/vim-koans/) - I'm not sure if they actually bring wisdom, but I do find them somewhat amusing.

[^1]: How I Vim is/was a [series of interview blog posts](http://howivim.com/about/) by Kevin Litchfield about people who use Vim. I've read several of the posts and I think they are very interesting.
[^2]: I realize that the phases might also align with phases of maturity. From being young, immature, and brave, then growing into experience, wisdom, and skill.
[^3]: Probably people who wouldn't listen as well. Sorry!
[^4]: There was a repo once, but it has been long since deleted and forgotten.
[^5]: Somewhere around 2017 I converted to Neovim, but for the sake of this post I consider that to be Vim.

