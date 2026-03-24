+++
title = "hello, world"
date = 2026-03-24
description = "first transmission from this layer"
[taxonomies]
tags = ["meta", "systems"]
+++

So this site exists now. After years of telling myself I'd eventually write things down somewhere that isn't a scratch buffer or a `# TODO` comment buried in a dotfiles repo, here we are. The stack is [Zola](https://www.getzola.org/) -- a single binary, zero dependencies, compiles templates faster than I can save the file. No node_modules, no build pipeline that takes longer than my actual code, no JavaScript unless I explicitly ask for it. Just markdown in, HTML out. The way the net was supposed to work.

## why static, why now

I've watched the web get buried under layers of abstraction that exist to solve problems most sites don't have. SPAs for content that never changes. Hydration for pages that don't need interactivity. Megabytes of JS to render text. Meanwhile a static site generator respects both the author and the reader -- it produces artifacts that are just *files*, served directly, cacheable forever, readable with `curl`. There's something satisfying about a deploy that's just `rsync` to a box you control. No container orchestration. No serverless invocations. Just bits on disk and a web server that's been doing this since before most frameworks were born.

## the setup

The workflow is about what you'd expect. Neovim with a terminal split, `zola serve` watching for changes, browser on the other workspace if I feel like previewing. Mostly I don't -- I trust the markdown. The whole thing lives in a git repo and deploys on push. The template is hand-written, no framework, just semantic HTML and enough CSS to not be hostile to eyeballs. Here's the build, for the curious:

```bash
# build and deploy -- the whole pipeline
zola build
rsync -avz --delete public/ deployer@edge:/srv/www/blog/
```

The gates of Steins;Gate are open, and the divergence meter reads a worldline I haven't been to before. Let's see where this one goes. Every transmission has to start somewhere -- this is `000`.
