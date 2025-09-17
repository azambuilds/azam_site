## Azam Blog — Authoring Guide (Zola + Duckquill)

This repo is a Zola site using the Duckquill theme.

### Run locally
- Install Zola, then:
```bash
zola serve
```
- Open http://127.0.0.1:1111

### Where posts live
- Blog posts live under `content/blog/`.
- Each post is a folder with an `index.md` so you can co-locate images:
```
content/
  blog/
    _index.md            # section config (already set)
    hello-world/
      index.md           # the article
      banner.jpg         # optional banner (recommended 1920x960, 2:1)
```

### Create a new post
1) Create a new folder under `content/blog/your-slug/`.
2) Add `index.md` with TOML front matter, then content. Use the template below.
3) Optional: put a `banner.jpg` next to `index.md` and set `banner = "banner.jpg"`.
4) Use `<!-- more -->` to define the summary cut-off.

### Ready-to-copy post template
```md
+++
# Required
title = "Your post title"
# YYYY-MM-DD or RFC3339 (no quotes)
date = 2025-01-01
# Set to true while writing
draft = true
# Use Duckquill article template
template = "article.html"

# Optional
description = "One-liner used in cards and lists"

[taxonomies]
# Enable tags in config to use these (see below)
tags = ["notes", "zola"]

[extra]
# Duckquill extras
banner = "banner.jpg"         # 1920x960 (2:1). Put the file next to index.md
banner_pixels = false          # true keeps pixel-art sharp when scaled
# card = "card.png"           # OPTIONAL: custom social/share card filename colocated with the post
katex = false                  # enable KaTeX for this page
toc = false                    # per-post ToC
+++

Intro paragraph that will appear in lists.

<!-- more -->

## Section
Your content here. Use Markdown as usual. Shortcodes also work, e.g.:

{{ youtube(id="dQw4w9WgXcQ") }}

{% alert(kind="info") %}
Heads-up: this is an informational callout.
{% end %}
```

### Homepage (intro + latest posts)
- Edit `content/_index.md` to change the homepage intro (supports Markdown and Duckquill shortcodes).
- The page uses `templates/index.html` to render an intro and a “Latest posts” list.
- To change how many posts show on the home, edit this line in `templates/index.html`:
```tera
{% for page in blog.pages | slice(end=5) %}  {# change 5 to your desired count #}
```

### Header navigation (navbar)
Navbar items are configured in `config.toml` under `[extra.nav]`.
```toml
# config.toml

[extra]
default_theme = "system"   # shows theme switcher

[extra.nav]
auto_hide = false
show_theme_switcher = true
show_feed = true            # shows feed icon (requires generate_feeds = true)
show_repo = false
links = [
  { name = "Links", menu = [
      { name = "Blog", url = "blog/_index.md" },  # internal: use content path or section
      { name = "Demo", url = "/demo/" },          # internal: absolute path
      { name = "Mods", url = "/mods/" }
  ]},
  { name = "Coffee", url = "https://example.com/coffee" }  # external link
]
```
- Internal URLs:
  - Use a content path like `"blog/_index.md"` to let Zola localize correctly.
  - Or use an absolute path like `"/about/"`.
- External URLs must be full `https://…`.
- Site title shown in the navbar comes from top-level `title` in `config.toml`.

### Drafts and publishing
- Keep `draft = true` while writing. Drafts are hidden by default.
- Preview drafts locally with:
```bash
zola serve --drafts
```
- Publish by setting `draft = false`, then commit/push.

### Tags (optional)
Enable tags in `config.toml` to display/use them:
```toml
# config.toml

taxonomies = [
  { name = "tags", feed = true },
]
```
Then use `tags = ["..."]` in your post front matter (already shown in the template).

### Images
- Prefer co-locating images next to `index.md` and link them relatively: `![alt](my-image.png)`.
- Banner: set `banner = "banner.jpg"` in `[extra]`. Recommended size 1920x960 (2:1). Ensure the file exists.
- Social card: optional `extra.card = "card.png"` to override the generated card.

### Search
- `build_search_index = true` is enabled in `config.toml`.
- Duckquill provides a built-in search UI; no extra setup needed.

### Theming quick knobs
- Global settings live in `config.toml` under `[extra]`. Common ones:
```toml
[extra]
# Accent color (light/dark)
accent_color = "#3584e4"
accent_color_dark = "#ff7800"
# Default theme: "light" | "dark" | "system"
default_theme = "system"
# Per-site extras
emoji_favicon = true
```
- Per-post overrides go under `[extra]` in the page front matter (see template).

### Build for deployment
```bash
zola build
```
Outputs to `public/` (configurable via `output_dir`). Deploy that folder to your static host.

### Deployment
Install firebase-cli, e.g. via brew
first do firebase logout in case u aren't on the right user
then firebase login
then firebase deploy --only hosting

### Troubleshooting quick notes
- Front matter must use TOML delimeters `+++ … +++`.
- `extra.card` must be a filename (string), not a boolean.
- If you set `extra.banner`, the file must exist next to `index.md`.
- If the homepage “Latest posts” fails, ensure `content/blog/_index.md` exists and `get_section(path="blog/_index.md")` is used in `templates/index.html`.

### Shortcodes you can use (Duckquill)
- YouTube: `{{ youtube(id="...") }}`
- Vimeo: `{{ vimeo(id="...") }}`
- Video (HTML5): `{{ video(src="movie.mp4") }}`
- Image helper: `{{ image(src="pic.jpg", alt="...") }}`
- Alert blocks:
```md
{% alert(kind="warning") %}Careful!{% end %}
```
More options and features are documented in the Duckquill site.

### References
- Duckquill documentation: [duckquill.daudix.one](https://duckquill.daudix.one/)
