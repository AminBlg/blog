+++
title = "tmux changed my brain"
date = 2026-03-22
description = "on terminal multiplexers and spatial memory"
[taxonomies]
tags = ["linux", "tools"]
+++

before tmux i used to open terminals like someone who had never heard of organization. twelve gnome-terminal windows scattered across two monitors, each one a mystery. which one had the server running? where was the ssh session? the answer was always "click through all of them until you find it" and the cognitive overhead was real. tmux did not just organize my terminals. it reorganized how i think about work itself.

the mental model is spatial and it maps perfectly to how my brain already wanted to work. sessions are contexts -- one for each project, each concern, each thread of thought. windows are rooms inside those contexts. panes are surfaces within those rooms. i have a `dev` session where window 0 is always neovim, window 1 is always the dev server, window 2 is a scratch shell. i do not have to think about this. my fingers know that `prefix-2` means "go check the logs." the spatial consistency means my brain can offload the "where is this" question entirely and focus on the "what am i doing" question instead.

```bash
# ~/.tmux.conf (the important parts)
set -g default-terminal "tmux-256color"
set -ag terminal-overrides ",xterm-256color:RGB"
set -g base-index 1
set -g pane-base-index 1
set -g renumber-windows on
set -g mouse on
set -g escape-time 0
set -g history-limit 50000

# prefix
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# splits that make spatial sense
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"

# vim-style pane navigation
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# session management
bind S command-prompt -p "new session:" "new-session -s '%%'"
bind K confirm-before -p "kill session? (y/n)" kill-session
```

people keep talking about spatial computing like it is the next frontier. apple sells you a headset so you can arrange windows in three-dimensional space. but i have been doing spatial computing since i opened my first tmux session. the terminal already solved this. it just did it with text instead of pixels, with keyboard shortcuts instead of hand gestures, and it runs on a machine from 2015 without needing to render a single polygon.

the real shift, though, is not about tmux specifically. it is about what happens when your tools become transparent -- when you stop thinking about the tool and start thinking through it. i do not "use" tmux the way i "use" a web app. tmux is just how i see my work now. the sessions are the shape of my attention. the panes are the boundaries of my focus. when i detach from a session and reattach later, the context comes flooding back instantly because the spatial layout is a mnemonic. this is what good tools do. they do not demand your attention. they become the medium through which you attend to everything else.
