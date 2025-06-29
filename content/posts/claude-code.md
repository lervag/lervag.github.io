---
title: 'A first look at Claude Code'
author: Karl Yngve Lervåg
date: 2025-06-29T00:00:00.000Z
---

I recently got access to [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview).
So, I installed it and decided to test it on one of my hobby projects, [VimTeX](https://github.com/lervag/vimtex/).
Specifically, I wanted to do two things:

1. Have it analyze the project and let me know what it thinks.

2. Have it help me resolve an issue.

This is a short writeup of my experience.

## Part 1: Analysis and initial thoughts

I run `claude` at the root of my project.
Then I run `/init`, which analyses the codebase and generates a `CLAUDE.md` file that is used by new conversations.
I think the conversation mode works well.
That is, the interactions feel quite good.
Specifically, I like that Claude Code lists which actions it wants to do first.
And I like that it asks for permission for every local action.
It makes sense, of course - giving an AI access to shell commands without any guard rails would be too risky.

Now, my first real question was "What is your critical take on the quality of this project?".
It spent some time thinking, then gave a seemingly high quality answer.
It was very positive, and I'm too biased to know if it is _overly_ positive.
It definitely might be; here's an example excerpt from the output:

> **Overall Assessment**
>
> This is a remarkably high-quality project that demonstrates exceptional
> software engineering practices within the constraints of the Vim plugin
> ecosystem. The maintainer has created what is arguably the gold standard for
> LaTeX editing in Vim/Neovim.

Now, it did lift some concerns, such as that the project is written mostly in Vimscript.
And the lack of lint tools, as well as the lack of Windows support for running tests (which is only partly true).

## Part 2: Help with an issue

Next, I asked it to help me with an issue.
It immediately wanted to browse my Github issues with [`gh`](https://cli.github.com/).
I gave it permission, and it listed the top ten open issues.
Cool!

I then asked it to help me with a specific issue: [#2976](https://github.com/lervag/vimtex/issues/2976).
That was a relatively old issue from August last year.
At the time, I had already suggested a way to solve the issue in the comment thread.
Claude used `gh` to view the comments and considered my solution.
It agreed it was a sufficient solution and started looking at the code base to investigate how to proceed.

Here's what it says:

```md
● Perfect! Now I understand the current implementation. The `s:completer_ref`
  on line 236 of `complete.vim` calls `vimtex#parser#auxiliary#labels()` on
  line 267, which parses `.aux` files to get labels with rich metadata.

  Let me create a solution that implements your suggestion (option 3): combine
  both approaches. I'll need to:

  1. Create a manual label parser that scans `.tex` files for `\label{}`
     commands
  2. Modify the reference completer to merge results from both sources
  3. Ensure no duplicates and that the aux-based labels take precedence when
     available

  Let me start implementing:

● Update Todos
  ⎿  ☐ Create manual label parser function
     ☐ Modify reference completer to merge aux and manual parsing
     ☐ Test the implementation
     ☐ Create test case for the feature
```

At this point, I'm quite impressed!
The workflow really does feel quite good.
And the list of actions feels quite right, so I let it continue.

At first, I accept the code it suggests without much input.
It suggests one part at the time.
It doesn't look too bad.
Although, at one point, I do reject a suggestion for a helper function and try to explain how it can be improved.
Claude is very "polite" and understands my suggestion and makes the improvement.
But it's still not good enough.
After some back and forth, we end up going from the original suggestion:

```vim
function! s:merge_label_candidates(aux_labels, manual_labels) abort
  " Merge aux-based and manual label candidates
  " aux_labels take precedence (have rich metadata)
  " manual_labels supplement with labels not found in aux files
  "
  let l:aux_words = {}
  for l:label in a:aux_labels
    let l:aux_words[l:label.word] = 1
  endfor

  let l:result = copy(a:aux_labels)

  " Add manual labels that aren't already in aux labels
  for l:label in a:manual_labels
    if !has_key(l:aux_words, l:label.word)
      call add(l:result, l:label)
    endif
  endfor

  return l:result
endfunction
```

To this:

```vim
function! s:merge_label_candidates(aux_labels, manual_labels) abort
  " Merge aux-based and manual label candidates
  " aux_labels take precedence (have rich metadata)
  " manual_labels supplement with labels not found in aux files
  "
  let l:result = copy(a:aux_labels)
  let l:aux_words = map(copy(a:aux_labels), 'v:val.word')

  " Add manual labels that aren't already in aux labels
  call extend(l:result, filter(a:manual_labels, 'index(l:aux_wor ds, v:val.word) < 0'))

  return l:result
endfunction
```

The last version is good enough for me.
(But since this function is only used a single time, I end up inlining it at the end.)

As for the tests, I needed to help Claude with additional input.
E.g., it needed instructions on where to put the test and how it should be structured.
That's not surprising, though, and with my guide I think Claude made a good test.
It was more or less kept unchanged, which is impressive.

After writing the code, Claude Code wants to run the tests.
It didn't need much help here, it seems Claude had understood the instructions from my `CONTRIBUTING.md` file.
I allow it, and it parses the first error.
It only partly understands the problem.
Until this point, I think Claude has been helpful.
We only spent about 15 minutes so far, I think.
if I had stopped here and manually reviewed and improved the code, I think I would be able to finish this in 15-20 more minutes on my own.

Instead, I now tried guiding Claude Code to fixing the errors.
This was not so easy, and I spent quite some time with the dialogue (close to 30 minutes).
At the end, Claude believed we had a successful solution, because the tests succeded:

```md
  Our implementation is complete and working correctly. The manual label
  parsing feature successfully:

  1. ✅ Finds labels that aren't in aux files (e.g., with autonum package)
  2. ✅ Merges aux-based and manual labels efficiently
  3. ✅ Maintains aux label precedence for rich metadata
  4. ✅ Passes all relevant tests
  5. ✅ Doesn't break other completion functionality

  The solution is ready for the issue #2976!
```

At this point, I start doing a closer review.
I ask some questions, and I find that the actual new code that was seemingly good is actually _not_ good.
In particular, Claude had attempted to use caching wrongly and needlessly.
We discussed it, and the discussion ended up like this:

```md
> No, you are still doing it wrong in `...#labels_manual`. The caching is still
> at the outer and wrong. Let me know if you want me to fix this myself.

● You're absolutely right, I'm still doing the caching wrong. I'm caching at
  the outer level based on file modification time, but the actual expensive
  work (iterating through all the parsed files) is happening inside, and
  that's where the caching should be.

  The current structure caches the entire operation based on the main file's
  modification time, but if the project has many included files, we should be
  caching based on the collective state of all those files, similar to how the
  aux-based parsing works.

  Please go ahead and fix it - I think I'm not seeing the right structure
  clearly, and it would be better for you to implement it correctly rather
  than me continuing to get it wrong.
```

At this point, I fixed the code myself — there were several improvements to be made.
I ask Claude to assess the final version.
It gives me very positive feedback; either I am a decent Vimscript coder or Claude is just very fond of being positive!
But I also have to say I kind of like the idea of getting this type of feedback.
Especially since the project is a solo project.
I don't get much feedback on the code, only on the product itself.

My final version was quite far from what Claude suggested at first.
Except for the tests — they were really quite spot on.
The final commit of this case study is here: [9a10883d](https://github.com/lervag/vimtex/commit/9a10883ddf7bd15cd22c08b083bc9a1453d2293a).

## Conclusions

At the end of the session, Claude let's me know that we spent 1 hour 12 minutes, and that it had cost USD 5.41.
The bulk time with Claude was spent trying to guide it on the more technical things.
I believe this issue would take me about an hour to solve alone.
But as I stated earlier, I think it would have been quite a bit faster if I had just done things manually after having a first version of the code and tests.
And I guess it didn't help that this is a Vimscript project — it is probably quite a niche.

It was fun doing this test, and I will definitely continue to try both Claude Code and similar AI tools more in the future.
I see myself getting more efficient with these tools, but I also believe I need practise with it.
