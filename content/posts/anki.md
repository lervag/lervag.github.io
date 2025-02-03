---
title: 'The tools that I love: Anki'
author: Karl Yngve Lervåg
date: 2025-02-03
---
> In this series, I will briefly talk about tools that I love and that I use often.
> I will keep things brief, as there is already a lot of good content that goes into more depth.

[Anki](https://apps.ankiweb.net/) is a flash card tool to help with remembering things.
In their own words:

> Anki is a program which makes remembering things easy.
> Because it's a lot more efficient than traditional study methods, you can either greatly decrease your time spent studying, or greatly increase the amount you learn.
>
> Anyone who needs to remember things in their daily life can benefit from Anki.
> Since it is content-agnostic and supports images, audio, videos and scientific markup (via LaTeX), the possibilities are endless.
> For example:
>
> - Learning a language
> - Studying for medical and law exams
> - Memorizing people's names and faces
> - Brushing up on geography
> - Mastering long poems
> - Even practicing guitar chords!

![Anki](https://ankiweb.net/logo.png#center)

In my experience, Anki works wonders!
I've used it to both learn and memorize geography, math, physics, programming languages, frivolous details about people, quotes, various laws of life, and a lot more.
As a small example: Before I used Anki, I could not name or list many of the countries in the world.
Today, I know a few facts for more or less every country in the world.
This includes silly things like the dialling codes or a rough estimate of populations.

It is a little bit hard to explain how much I like Anki.
Before I started using it, I didn't really have a good method for memorizing things.
I was already good at remembering things that I could understand, but other things usually slipped my mind fast.
I now have about 11850 cards in my Anki deck.
My retention is about 90 %, so I would likely remember the answer to around 10665 of my cards.
This means I am now able to remember things more or less _at will_.

The main value for me came when I started to use it for things more relevant to my work and hobbies.
Today, if I work on something new, I always make sure to write a few Anki cards for the main concepts.
This can be everything from new programming languages, new tools, or the domain that I work with.
Doing this, I am able to keep a much more livid memory of the topics even years after I worked on them.
And this makes it much easier to make new connections between different topics as well.

Anki really feels like one of the _real_ life hacks in this world.
I can strongly recommend it!

## apy

Anki comes with a very nice graphical user interface on the desktop.
It also has very good applications for your phone.
However, as a long-term fan of the command-line interface, I've ended up writing my own little tool for interacting with my Anki cards:
[`apy`](https://github.com/lervag/apy).

With `apy`, I can easily add new cards with `apy add` or `apy add-from-file`.
And when I do my daily reviews, I will mark cards that I find need adjustments.
Then I do `apy review [query]`, which allows me to quickly edit the marked cards in a simple, but efficient terminal user interface.

## Continue reading

There are a lot of very good blog posts and essays on Anki and on spaced repetition learning.
Here are a few of the ones that had an impact on me:

* [20 rules of knowledge formulation](https://supermemo.guru/wiki/20_rules_of_knowledge_formulation) - this is a wiki article written by Piotr Wozniak.
  Wozniak is the author of the first computer algorithm for spaced repetition.
  I've found that his list of rules is very on point, although I've not incorporated the _incremental reading_ part.
* [Augmenting Long-term Memory](http://augmentingcognition.com/ltm.html) (2018) - this is a long, but _very_ good essay by Michael Nielsen.
  I can heartily recommended reading it if you are already starting to get invested in Anki.
* [Spaced Repetition for Efficient Learning](https://gwern.net/spaced-repetition) - another relatively lengthy post about spaced repetition.
  Although Gwern uses another tool, his writing is still relevant to Anki users.
  Gwen also refers to a lot of interesting research literature on the topic.

