---
title: 'How I approach learning new concepts'
author: Karl Yngve Lervåg
date: 2026-01-17
---

My friend Jakob wrote a very interesting and insightful post on competence and learning ([Whether you think you can, or think you can't](https://jakob.fun/blog/whether-you-think-you-can-or-think-you-cant)).
In his post, he requested that I share some thoughts.
I won't assume everyone reads his post, even though I recommend it, so here's a brief summary of his main points:

- Software engineering is a field where constant learning is more or less a requirement throughout one's career.
- As a consequence, software engineers should come to terms with "feeling stupid". There will always be new challenges with unknowns and situations where you won't immediately understand what you are doing.
- He writes about how building competence and skill requires that you embrace the uncomfortable situations where you are in the dark place. Where you don't know what you are doing.
- Next, he observes that it may be very useful to be equipped with what he calls _intellectual confidence_. That is, a confidence that one will be able to understand things even though one is currently in the dark.
- I believe he correctly points out that this intellectual confidence is a feedback loop; a chicken and egg-problem.
- He also mentions that there's another ingredient for building competence and to enhance the before-mentioned feedback loop: discipline. And then he goes on to implore me to write about discipline and on my approach to learning[^1].

There's a lot of interesting stuff here!
But in this post I want to focus on two things: intellectual confidence, and of course, the requested thoughts on discipline and my approach to learning.

## Intellectual confidence

I think the term _intellectual confidence_ is well put.
I've always found myself quite confident in my ability to learn and understand something new.
That is to say, I don't remember having to build this confidence.
It might be genetic, or something I got during my upbringing from my parents, environments, or simply through sheer luck?
But I can easily say that this confidence has been very useful!
Being confident makes it easy for me to enter into the unknown and dark places where I don't fully understand or know something.

It is clear that this is a type of confidence that is not universal.
So it raises the question of how to build this type of confidence.
Here's one approach based on Jakob's post combined with my personal reflections:

1. Take a step into the unknown: You have to step out of your comfort zone.
2. It might help to consider the confidence as an attitude that you can equip. At first it may feel wrong, but I think there may be value in the adage "fake it until you make it".
3. Since there's a feedback loop, you should attempt to make it a positive feedback loop. You might step out of your comfort zone and fail, which may hurt. But then you should remember that _that's ok_! And try again! Utilize the wins to boost your confidence. There will be wins.
4. When you step into the dark, you may need some discipline to stay the course or some routine/technique to navigate the waters.

## Discipline and learning

Jakob writes the following:

> While it’s good to understand the importance of being competent, I believe a second ingredient is needed to get the most out of the positive feedback loop I’ve described: Discipline. Specifically, the discipline to actually apply this philosophy by taking the time to learn new concepts you come across. In the short term, this will slow you down, and it will require you to be comfortable with facing your lack of knowledge all of the time, rather than just some of the time.

> This is something Karl Yngve, the previously mentioned scientist-turned-programmer, is excellent at. It really seems like it’s second nature to him, and I formally implore him to write a post on his own blog explaining how that came to be.

What I take from this is that Jakob thinks I am good at taking the time to learn new concepts as I come across them.
And I think he is probably right.
And I think discipline is definitely part of this.
Discipline is ["the self-control that is gained by requiring that rules or orders be obeyed, and the ability to keep working at something that is difficult"](https://en.wikipedia.org/wiki/Discipline).
I do have some amount of self-control and discipline, especially when it comes to topics and areas that are interesting to me.

But when it comes to how I approach learning new concepts, I think discipline is only a part of the equation.
Before I unpack that, I think it might be useful to first describe my approach[^2]:

1. I'll try to write out clearly what my current goal is. Perhaps I'll write it in a shared issue description at work, or in my journal, or if it's hobby related I'll write it in a page in my personal wiki.
2. I research the topic via colleagues, AI chats, web searches and references/documentation.
3. Then I document my findings in my personal wiki or in shared docs.
4. While writing things down, I often realize that some parts are useful to remember in the future, and I'll make new Anki notes to memorize it.

As an example, when I needed to do an upgrade of a PostgreSQL server for the first time in my career, I wrote down what I was doing along the way.
In this case, I had a very skilled colleague who helped me out, thus researching was mostly sparring with him.
The work ended up being a blog post:
[The first time I upgraded a PostgreSQL database](/posts/db-first-upgrade).
In this case, the main motivation for writing the blog post was to improve my own learning.

Now, let's unpack what I mean about discipline only being part of the technique.
What I've described above is my approach to learning.
It is a technique; and I think having good technique can be extremely useful!
I want to go into that in some more depth.
In particular, I want to focus on memory and writing, and on structure and process.
But I also want to address another missing ingredient: ambition, or an inner drive or motivation.

### Technique: Memory and writing

My memory is at most average.
That's fine, and until around grad school it wasn't really a problem.
But when the topics I worked with became more complicated, I realized that I needed to "extend" my memory somehow.
That is, to properly understand advanced topics, I need a way to handle large contexts, and I can't only rely on my memory for that.
My solution has been to build and maintain a personal wiki or knowledge base where I collect all sorts of information; see [The tools that I love: My personal wiki](/posts/wiki.vim).

My solution might not be what's best for you.
That's OK.
But I strongly advice on finding some way to take structured notes.
Maintaining and using a personal wiki has three very important benefits for me:

1. Writing about things I'm working on or thinking about is a very good way to understand things better in itself.
2. Writing down things with links between topics and pages helps me build a personal model of how different concepts relate.
3. Writing down things in a personal wiki makes it easy to find relevant information when I need it in the future.

I've also found it very useful to strengthen my memory on certain topics.
For this I use Anki, which is a program that makes remembering things easy.
With Anki, I've memorized a lot of things that I find useful at work, including tips and tricks about Python, SQL, Scala, Linux and unix tools, algorithms, shortcuts for a lot of different tools, and so on.
Every once in a while, remembering things because of Anki really ends up being useful!
I won't go into more details, here, but for those interested:
[The tools that I love: Anki](/posts/anki).

### Technique: Structure and process

I previously outlined my approach to learning, and I've mentioned how I maintain notes.
The next part of my technique is the structure and process for when I apply my approach.

I maintain a journal for work that I use to keep track of my most important tasks.
This is very useful for me to prioritize which task to work on and to write down related and necessary context (e.g. links to GitHub issues).
I also maintain a "hobby index", that is, a dedicated page in my personal wiki where I write down things that I want to explore on my own time when I get the time.

When I work on something, I regularly notice things where there's a gap in my knowledge.
Whenever I do that, and if I think this is an interesting or important gap, I write it down.
If it is clearly work related, _and if it is important for work_, I write it in my work journal and I'll dive deeper into it at work.

If it is not properly work related, it goes into my hobby index.
For instance, I might write down the link to a blog post or article I want to read or that someone shared with me.
I pick up this page every once in a while to work through it and ensure it doesn't grow too large.
But since this is on my spare time, I do it because I want to; and if there are things left from some earlier times that I'm no longer interested in, I can safely remove it.

Finally, and also quite important:
This technique has developed over time.
And I think it is useful to sometimes reconsider it and look for ways to improve it.
And I use my technique to do that as well!
For instance, I might write in my hobby index if I notice something I should do differently.

### Ambition and the desire to learn

I promised to unpack how I believe discipline is only a part of the equation.
In the above, I've outlined a second part: how I approach learning through techniques and a structured process.
Discipline is necessary, and as I said, it feels easy for me to apply discipline when I'm learning things that are interesting to me.
But I believe there is a third part: an internal desire or drive to understand, learn and to become good at what I do.

I don't know where this ambition or inner desire to learn comes from.
I think I've always liked to learn so there's always been some of this ambition inside of me.
But I think it is crucial, because it is the fuel that drives me to pursuit personal growth.

That is, ambition helps motivate me and fuels my drive to learn and grow as an engineer.
At some point in my career, I realized that when I combined my ambition with some discipline and a good technique, there was little I could not accomplish.
And interestingly, this feed into the positive feedback loop.
When I feel like I'm reaching my ambitions, it is a very good feeling!

## Final remarks

I want to look back once more on what Jakob wrote:

> I believe a second ingredient is needed to get the most out of the positive feedback loop I’ve described: Discipline. Specifically, the discipline to actually apply this philosophy by taking the time to learn new concepts you come across.

The first ingredient Jakob is pointing to is to understand the importance of being competent.
I didn't discuss that here, and I won't, but is is an interesting claim that I agree with.
He raises discipline as a second ingredient.
And I want to raise two more ingredients:

- Technique: use writing and tools to aid your learning. And strive to improve both your tools and technique over time.
- Ambition, or inner drive: figure out what your internal drive is? Try to use this to fuel your path to becoming more competent.

To end my post, I want to repeat some words from Jakob that I find I am in total agreement with:

> It’s okay to be clueless, to not know, and to not understand. Every single person who is good at anything, including all of the people you look up to, started out that way. Not understanding the things you work with is uncomfortable, and it’s supposed to be. The only sustainable way to mend that feeling of discomfort is to accept the fact that you don’t understand, and decide to do something about it. Don’t resign yourself to a life of not getting it. You should do better, and you can do better.

[^1]: I had to look up the word _implore_: "beg someone earnestly or desperately to do something".
[^2]: I use more than a single approach. For instance, I'm also a fan of the [Feynman technique](https://en.wikipedia.org/wiki/Learning_by_teaching#Plastic_platypus_learning). And I might use a combination of approaches. But I believe what I'm writing here is what I most often tend to do.

