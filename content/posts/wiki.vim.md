---
title: 'The tools that I love: My personal wiki'
author: Karl Yngve Lervåg
date: 2025-04-06T00:00:00.000Z
---
> In this series, I will briefly talk about tools that I love and that I use often.
> I will keep things brief, as there is already a lot of good content that goes into more depth.

People who know me know that I am very fond of using text both as a help for thinking clearly and for communication.
I take a lot of notes.
Most of these are in my own personal wiki.
In this blog post, I'll explain how I write and maintain my personal wiki.
I'm not going in depth, instead I'll highlight some of the essential parts of my workflow.
I might write some in-depth posts about certain parts at a later time.

For those in a rush, here's a few key takeaways:

- Speed is key - make sure you have a _fast_ workflow:
  - Use keyboard shortcuts to open whichever tool you use for taking notes. Your tool should open fast!
  - Ensure you have a way to very quickly search your notes.
- Write short pages and use links between related pages.
- Don't obsess about the wiki structure.

In my experience, the last point is really quite important!
I like how [@mgoral phrased it](https://github.com/lervag/wiki.vim/issues/101#issuecomment-718284921) (some people may enjoy reading the entire thread from which this quote is captured):

> I think that the most important thing is to not overthink things and just keep writing stuff down.

## Context and tooling

I first started taking digital notes in 2007, which is about 18 years ago now.
Today, my personal wiki consists of a large amount of simple text files.
As of 2025-04-05 I have about 15 megabytes worth of text in 3365 files.
The files span a total of 112 033 lines of text.

I use [Syncthing](https://syncthing.net/) to keep my files synchronized across my computers, phones, and tablets.
I use [Neovim](https://neovim.io/) with [wiki.vim](https://github.com/lervag/wiki.vim) to view, search, navigate and write my notes.
`wiki.vim` has most of the main features I need, especially concerning links.
Neovim combined with a few other plugins fill in the rest.

Note, though, that I strongly believe that one may adopt a similar workflow with a lot of different tools.
Some alternatives are listed [on Wikipedia](https://en.wikipedia.org/wiki/List_of_wiki_software#Personal_wiki_software).
I think this blog post may be interesting even if you use different tools.

## Why write a personal wiki?

From my point of view, writing notes is a good way to extend my memory.
And writing the notes is itself a useful process for several reasons:

- It helps me remember.
- It helps me to _think_ about the thing I'm writing about and thus to learn.
  If I can't find a good way to put something, it is usually because of a lack of understanding.
  This is related to the [Feynman technique](https://en.wikipedia.org/wiki/Learning_by_teaching#Plastic_platypus_learning), so I'm obviously doing things that greater minds than mine found useful. That's cool!

The idea of linking the notes and creating _a personal wiki_ has strongly increased the power of my notes.
Without links, the notes were already useful.
I could search them and I benefited from writing them.
But having the links allow me to _connect related concepts_ in a very flexible manner.
It makes it easier to keep my notes short and to the point, with links to related or more in-depth notes.
Adding the connections also seems to help me _realize_ and _recognize_ these connections.
The personal wiki essentially becomes a tool for [personal knowledge management](https://en.wikipedia.org/wiki/Personal_knowledge_management).

## Key aspects of my workflow

In the remaining part I'll briefly outline what I think are the key aspects of my workflow:

- Speed
- The wiki index
- How I write (a few remarks on writing and structure)

These are things that I find import for successfully maintaining my personal wiki.
I believe some of this should be relevant for most people.

### Speed

In my opinion, speed is very important.
That is, the tools should start fast, navigation should be fast, searching should be fast.
It should be fast, because I don't want to break my current flow of thought when I lookup a note or if I want to write something quickly.
If I know I've written about something, I should be able to find it in a matter of seconds.

I don't think it matters much _how_ one achieves speed.
For me, the following things work very well:

- I have a system shortcut `<alt-n>` that opens my personal wiki in Neovim [^1].
  This mapping ensures that it takes less than a second from the moment I have a thought until I can start to add it to my wiki for later processing.
  The allows me to write down a thought and then continue with the current work without being disrupted.

- I have a global mapping in Neovim (`<leader>ow`) that starts a search for a file in my wiki.
  It usually takes no more than a couple of seconds from the moment I think of something until I have opened the relevant note.

- I use [ctrlsf.vim](https://github.com/dyng/ctrlsf.vim) to search for content within my wiki pages.
  This plugin uses the very fast [ripgrep](https://github.com/BurntSushi/ripgrep) as a search backend.
  This makes it very easy and fast to search the contents of the wiki.
  If my search has too many hits, I can either narrow it down by changing the search pattern, or I can search within the result buffer of `ctrlsf.vim`.
  This has generally not failed me in finding information I'm looking for.

### The wiki index

Sometimes I want to write down stuff that I don't know where to put.
This includes a lot of different things:

- Links to things I want to read at some time.
- Ideas or thoughts I want to pursue at a later time.
- Link to pages in my wiki that need attention.
- Things that I want to memorize [with Anki](/posts/anki/).

All of the above are things that I put in my wiki index.
This is the page I reach with my `<alt-n>` shortcut mentioned above.
Essentially, anything I want to write down that does not have a dedicated page goes here.
And with time, often slowly, I work through the index and process the content.
This may result in new pages to hold new content, or that I just remove things that are no longer relevant.

### How I write

I make sure to write down what I feel like at the moment.
I clean it up and rinse/repeat as something is developing.
I add links to relevant external and internal pages/resources.
I often add examples, for instance tools I discover or libraries I learn about.

With time, I've started to adopt an unstructured approach where I avoid complicated structure and subdirectories.
I try to use the proper names of things as the name of a page.
I don't worry about using spaces in my wiki file names.
This simple structure makes it easy to convert text to the correct links and it makes it easy to remember the relevant page names.

I don't worry about having many pages.
I don't worry about almost empty pages.
I don't worry about keeping my pages in a hierarchy.
I've found it can be useful to split information into separate pages, as it makes it easier to link things.

For some topics, I may write a wiki page about stuff where I have links to data files in which I can append new data and to scripts I can run to visualize the data.
Thus, I don't keep everything in the wiki, instead, I may write the "metadata" and general overview in the wiki, and then keep the real data and relevant tools as a "third party".

## Closing remarks

The idea with this post was to give an overview.
I will be glad to go more in depth on parts of this stuff.
Feel free to reach out if there is something in particular that you think I should share more details on.

[^1]: Specifically, I map `<alt-n>` to the shell command `wezterm-gui start -- nvim +WikiIndex`

