---
title: 'Some thoughts on recent neovim changes'
author: Karl Yngve Lervåg
date: 2026-08-22
---

I've been a Vim and Neovim user for a long time now.
From the very beginning, around 2003-2004, I learned about "sharpening the saw" (see [Part 3 from Bram's "Seven habits of effective text editing"](http://moolenaar.net/habits.html)).
And I still find it both interesting and fun to continue sharpening the saw.
This is evident from my [dotnvim](https://github.com/lervag/dotnvim), which has seen more than 20 the last couple of months.

One of the things I really like about Neovim is that it embraces new ideas and improvements to the editor.
When I stay on Neovim nightly and follow the work and progress of the development, I get to test these new features as they arrive.
In the early stages, they are experimental and tend to change somewhat until they land in a versioned release.

For instance, during the last few months, Neovim has adopted and implemented the `:restart` Ex command [0].
Before this, restarting meant actually quitting the editor and restarting it from the terminal.
I do restart Neovim frequently e.g. when I am configuring the editor itself, because it is sometimes the best or only way to reload configuration.

In the first iteration of `:restart`, it didn't properly save and restore the session automatically.
It also didn't have a default mapping.
So I created a mapping for it in early April:

```lua
vim.keymap.set("n", "<leader>rr", function()
  local session = vim.fn.stdpath "state" .. "/restart_session.vim"
  vim.cmd("mksession! " .. vim.fn.fnameescape(session))
  vim.cmd("restart source " .. vim.fn.fnameescape(session))
end)
```

In a later update to Neovim, `:restart` started automatically saving and restoring the session, so I could simplify:

```lua
vim.keymap.set("n", "<leader>rr", "<cmd>restart<cr>")
```

Then Neovim got a default map: `ZR`.
I realized this was better than my own `<leader>rr`, so I removed it in July.
Now I use `ZZ` to write and quit the current buffer, `ZQ` to immediately exit Neovim, and `ZR` to restart it.
Neat!

And this is only _one_ of many improvements made to Neovim since v0.12 was released in March.
All the main changes are documented, see [news](https://neovim.io/doc/user/news/#news).
In addition to the `:restart` command, I've also embraced the new package manager [vim.pack](https://neovim.io/doc/user/pack/#_plugin-manager) (early April as well).

These days I'm very much looking forward to testing the new feature where Lua functions/closures can be assigned to "func" and "expr" options [1].
This means I can rewrite expressions like

```lua
vim.wo.foldexpr = "v:lua.vim.treesitter.foldexpr()"

-- to
vim.wo.foldexpr = vim.treesitter.foldexpr

-- or
vim.wo.foldexpr = function() return vim.v.lnum == 1 and '>1' or '1' end
```

I do maintain a few custom `foldexpr` functions and similar, so this will allow me to improve and clean up my config.
Cool stuff!

[0]: https://neovim.io/doc/user/gui/#%3Arestart
[1]: https://neovim.io/doc/user/options/#expr-option-function
