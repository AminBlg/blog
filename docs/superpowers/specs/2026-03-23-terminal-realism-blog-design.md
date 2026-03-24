# Terminal Realism Blog — Design Spec

## Context

Building a personal blog from scratch. The blog should feel like it belongs to a poweruser — monospace, dark, CRT aesthetic with Catppuccin colors. No flashy frameworks, no vibe-coded AI slop. Hidden cultural references reward the cultured. Simple authoring: write markdown, git push, done.

## Framework: Zola

Single Rust binary. No Node.js, no npm. Built-in Sass, search index, RSS, taxonomies, syntax highlighting. Deployed to GitHub Pages via GitHub Actions CI.

## Design Language: CRT Terminal + Catppuccin

### Visual Identity
- **Catppuccin Mocha** (dark, default) / **Catppuccin Latte** (light)
- Monospace everything, system font stack (zero web font downloads)
- CRT scanlines overlay + vignette edge darkening in dark mode
- Light mode: clean Latte colors, no CRT effects
- Follows `prefers-color-scheme` system preference by default
- `<meta name="color-scheme" content="dark light">` for extension detection

### Font Stack
```css
font-family: 'SFMono-Regular', 'Cascadia Code', 'Fira Code', 'JetBrains Mono', Consolas, 'Liberation Mono', monospace;
```

### Color Palette — Catppuccin

**Dark mode — Mocha:**
| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| Background | Base | `#1e1e2e` | Page bg |
| Surface | Mantle | `#181825` | Code blocks, inset areas |
| Border | Surface 0 | `#313244` | Separators, borders |
| Text body | Subtext 1 | `#bac2de` | Paragraph text |
| Text primary | Text | `#cdd6f4` | Titles, headings |
| Text muted | Overlay 0 | `#6c7086` | Dates, metadata, directory info |
| Text dim | Surface 2 | `#585b70` | Nav brackets, secondary UI |
| Link | Blue | `#89b4fa` | Nav links, prev/next |
| Post link | Yellow | `#f9e2af` | Post titles in listings |
| Tag | Green | `#a6e3a1` | Tags, username in prompt |
| Accent | Peach | `#fab387` | RSS, special links |
| Prompt host | Blue | `#89b4fa` | Hostname in prompt |
| Prompt user | Green | `#a6e3a1` | Username in prompt |

**Light mode — Latte:**
| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| Background | Base | `#eff1f5` | Page bg |
| Surface | Mantle | `#e6e9ef` | Code blocks, inset areas |
| Border | Surface 0 | `#ccd0da` | Separators, borders |
| Text body | Subtext 1 | `#5c5f77` | Paragraph text |
| Text primary | Text | `#4c4f69` | Titles, headings |
| Text muted | Overlay 0 | `#9ca0b0` | Dates, metadata |
| Text dim | Surface 2 | `#acb0be` | Nav brackets, secondary UI |
| Link | Blue | `#1e66f5` | Nav links |
| Post link | Yellow | `#df8e1d` | Post titles |
| Tag | Green | `#40a02b` | Tags, username |
| Accent | Peach | `#fe640b` | RSS, special links |

### Anti-Vibe-Coding Rules
- No gradients of any kind
- No hero sections or generic taglines
- No rounded corners, glassmorphism, or backdrop blur
- No emoji in chrome/navigation
- No animated entrance effects
- No Tailwind default palette
- No generic polished uniformity

## Project Structure

```
blog/
├── config.toml
├── content/
│   ├── _index.md                  # Homepage section
│   ├── blog/
│   │   ├── _index.md              # Blog section (sort_by = "date")
│   │   ├── on-ephemeral-systems.md
│   │   └── ...
│   └── about/
│       └── _index.md              # About page
├── sass/
│   └── style.scss                 # Single file: colors, layout, CRT effects. Easy to hand-edit.
├── static/
│   └── favicon.ico
├── templates/
│   ├── base.html                  # HTML shell, head, color-scheme meta
│   ├── index.html                 # Homepage: ls -la listing
│   ├── section.html               # Blog section listing
│   ├── page.html                  # Blog post: readable terminal
│   ├── taxonomy_list.html         # All tags
│   ├── taxonomy_single.html       # Posts for a single tag
│   ├── 404.html                   # Easter egg page
│   └── search.html                # Client-side search
└── .gitignore
```

## Pages

### Homepage (`/`)
- Styled as `sotch@blog:~$ ls -la posts/`
- Prompt uses Catppuccin colors: green username, blue hostname
- Each post rendered as a directory entry: `drwxr-xr-x  sotch  YYYY-MM-DD  post-slug`
- Post slugs are clickable links in Yellow
- Footer nav: `[about] [tags] [search] [rss]` in Blue, rss in Peach
- Post count + tag count below listing

### Blog Post (`/blog/slug/`)
- Top nav: `[index] [about] [tags] [rss]`
- Title in Text color (primary)
- Meta line: date in Overlay 0, tags in Green, reading time
- Rendered markdown with proper heading hierarchy (not raw `##`)
- Monospace body text in Subtext 1, comfortable line-height (1.8)
- Syntax-highlighted code blocks on Mantle background with Surface 0 border
- Prev/next navigation at bottom in Blue

### About (`/about/`)
- Simple markdown page
- Same template structure as blog post

### Tags (`/tags/`)
- List all tags
- Per-tag page lists posts in same `ls -la` format

### Search (`/search/`)
- Uses Zola's built-in `build_search_index = true` (elasticlunr.js)
- Simple input field styled as terminal prompt
- Results listed as terminal output
- No external services

### 404 Page
- Primary easter egg location
- "You don't seem to exist in this layer of the Wired."
- Styled as a terminal error output

### RSS (`/rss.xml`)
- Zola built-in, configured in `config.toml`
- Tags also get individual feeds

## CRT Effects (CSS + minimal SVG, dark mode only)

Dark mode (Mocha) only:
1. **Scanlines**: `repeating-linear-gradient` overlay, ~12% opacity
2. **Vignette**: `box-shadow: inset 0 0 80px rgba(0,0,0,0.5)` darkening at edges
3. **Animated scan line**: Thin bright band moving top-to-bottom via `@keyframes`, slow (~8s cycle)
4. **Phosphor flicker**: Very subtle opacity oscillation on scanline overlay
5. **Gaussian noise**: SVG `feTurbulence` filter as low-opacity background layer — analog static texture. No JS needed, pure SVG filter.
6. All CRT overlays applied via pseudo-elements / positioned layers with `pointer-events: none`
7. `prefers-reduced-motion: reduce` disables all animations
8. Light mode (Latte): clean, no CRT effects whatsoever

## Easter Eggs

Hidden references — NOT obvious. Only recognizable to the cultured:
- **404 page**: Lain quotes about the Wired
- **Footer quotes**: Fixed pool, randomly selected at build time via Tera's `now()` seed — no client JS needed
  - Sources: Cowboy Bebop, Ghost in the Shell, Texhnolyze, Ergo Proxy, Serial Experiments Lain, etc.
- **Last post in a tag listing**: "See you space cowboy..."
- **HTML source comments**: References in `<!-- -->` tags
- **Meta tags**: Subtle nods in `<meta name="keywords">`

## Content Schema (Frontmatter)

```toml
+++
title = "On the nature of ephemeral systems"
date = 2026-03-15
draft = false
description = "Thoughts on infrastructure impermanence"
[taxonomies]
tags = ["systems", "philosophy"]
+++
```

Note: `draft` is a Zola built-in field at the top level — drafts are automatically excluded from builds.

## config.toml Key Settings

```toml
base_url = "https://sotch.dev"  # adjust as needed
title = "sotch"
description = "terminal output"
compile_sass = true
minify_html = true
build_search_index = true
generate_feeds = true
feed_filenames = ["rss.xml"]

taxonomies = [
    {name = "tags", feed = true},
]

[markdown]
highlight_code = true
highlight_theme = "catppuccin-mocha"  # acceptable trade-off: code blocks use Mocha colors in both modes
external_links_target_blank = true
```

## Authoring Workflow

1. Create `content/blog/my-post.md` with frontmatter
2. Write markdown
3. `zola serve` for local preview (hot-reload)
4. `git add && git commit && git push`
5. CI builds and deploys

## Deployment

GitHub Actions workflow:
1. Checkout repo
2. Install Zola (single binary, fast)
3. `zola build`
4. Deploy `public/` to GitHub Pages

## Verification

1. `zola build` completes without errors
2. `zola serve` — homepage shows ls listing, posts render correctly
3. Light/dark mode toggles with system preference
4. RSS feed validates
5. Search works client-side
6. 404 page shows easter egg
7. All pages are monospace, Catppuccin themed
8. CRT scanlines + vignette visible in dark mode only
9. No CRT effects in light mode
10. Deploys successfully to GitHub Pages
