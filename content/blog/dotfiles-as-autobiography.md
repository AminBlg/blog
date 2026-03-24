+++
title = "dotfiles as autobiography"
date = 2026-03-10
description = "your config files say more about you than your resume"
[taxonomies]
tags = ["linux", "philosophy"]
+++

there is a particular kind of honesty in a dotfile that you will never find in a linkedin bio. no one curates their `.bashrc` for an audience. you write it at 2am because tab completion was driving you insane, or because you fat-fingered `rm` one too many times and decided that aliases were now a matter of survival. every line is a scar or a preference or a lesson learned the hard way. if you handed me your dotfiles i could probably tell you what language you write most, whether you are patient or impulsive, and how many times you have accidentally destroyed something important.

i have been maintaining the same neovim config for about four years now. it started as a minimal `init.vim`, became a lua rewrite, then a full `lazy.nvim` setup with more keymaps than i can consciously remember. the muscle memory knows things my brain forgot. sometimes i open a keymap file and find bindings i do not remember writing but use every single day. there is a philosophical question here -- the ship of theseus, but for configuration. if you replace every line over four years, every plugin, every colorscheme, is it still the same config? i think it is, the same way a river is still the same river. the identity is in the flow, not the water.

```lua
-- init.lua (fragment)
vim.g.mapleader = " "
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.termguicolors = true
vim.opt.scrolloff = 8
vim.opt.signcolumn = "yes"
vim.opt.clipboard = "unnamedplus"

-- the keybind i use most and remember least
vim.keymap.set("n", "<leader>ff", function()
  require("telescope.builtin").find_files({
    hidden = true,
    no_ignore = false,
  })
end, { desc = "find files" })

-- muscle memory deposited this here sometime in 2024
vim.keymap.set("v", "J", ":m '>+1<CR>gv=gv")
vim.keymap.set("v", "K", ":m '<-2<CR>gv=gv")
```

i keep my dotfiles in a bare git repo. `git log --oneline` reads like a diary. "fix tmux colors again." "remove plugin i never used." "why did i bind this." "3am." the commit messages get less coherent after midnight but more honest. there is an entire history of who i was becoming encoded in those diffs, each commit a small act of self-definition. we are, in the end, what we repeatedly configure.

your ghost lingers in the machine long after you step away from the keyboard. the configs keep running, the aliases keep resolving, the cron jobs fire on schedule. there is something almost spiritual about that -- the idea that your preferences, your muscle memory, your way of seeing the filesystem, persist as a kind of digital ghost. i do not know if that is comforting or unsettling. probably both.
