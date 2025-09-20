You are generating a single Markdown file for a Zola site using the Duckquill theme. Produce only the file contents, with no commentary or code fences.

Goal
- Create `index.md` for a new blog post under `content/blog/YYYYMMDD-<slug>/index.md`.
- The theme is Duckquill; respect its expectations and avoid options that break rendering.

Folder naming convention
- Name the post directory as `YYYYMMDD-<slug>` using the `date` in front matter (e.g., `2025-09-17` → `20250917`).
- Only the folder name gets the date prefix. Do not include the date in the title.
- Do not set `slug` in front matter; the folder name handles ordering/sorting.

Output format (this is mandatory)
1) TOML front matter delimited by +++ at top and bottom. (MAKE SURE TO HAVE 3 PLUS SIGNS, NO MORE NO LESS, to start and end the front matter)
2) A short intro paragraph.
3) A summary cutoff marker: `<!-- more -->` on its own line.
4) The rest of the article content (headings, sections, etc.).

Front matter requirements (Duckquill/Zola)
- Required:
  - `title` = "<Post title>"
  - `date` = YYYY-MM-DD (or RFC3339 without quotes)
  - `draft` = false (set true only if explicitly asked)
  - `template` = "article.html"
- Recommended:
  - `description` = "One-line summary for cards/lists"
- Optional (use only if explicitly provided in the instruction/context; otherwise omit):
  - `[taxonomies]` with `tags = ["tag1", "tag2"]`
  - `[extra]`:
    - `banner = "banner-filename.jpg"` ONLY if the file is colocated with `index.md`. If not provided, do not set.
    - `katex = true` ONLY if the post needs LaTeX.
    - `toc = false` (default). Set true only if asked.
- Never set the following unless explicitly asked:
  - `slug`, `path`, `aliases`, `card` (card must be a filename string if used; do not use booleans), or any keys not listed above.

Writing rules
- Do not include a top-level `# Title` heading in content; the theme prints the title.
- Start with a concise intro paragraph, then `<!-- more -->`, then the rest.
- Use meaningful section headings starting at `##`.
- Prefer clear, concrete explanations. Include examples, code blocks, and links when helpful.
- If you include code, add language fences (```rust, ```ts, ```bash, etc.).
- Internal links: use absolute site paths like `/blog/` or `/about/` when linking to known sections.
- External links: use full `https://…` URLs.
- Images: only reference files colocated with the post. If no image provided, do not set `extra.banner` and do not embed images.
- Math: if LaTeX is used, set `[extra] katex = true` and use inline $…$ or block $$…$$.

Allowed Duckquill shortcodes (use sparingly, only if relevant)
- Alerts:
  {% alert(kind="info") %}Note text{% end %}
- YouTube:
  {{ youtube(id="dQw4w9WgXcQ") }}
- Image helper:
  {{ image(url="my-pic.jpg", alt="Alt text") }}

Shortcode tips / gotchas
- Shortcodes do NOT evaluate Tera expressions inside arguments. Pass literal values only (e.g., `url="my-pic.jpg"`), not `page.colocated_path ~ "…"`.
- Duckquill’s image shortcode expects `url` (and optional `url_min`), not `src`.

Layout tips (images and floats)
- Using `{{ image(url="…", start=true) }}` or `end=true` floats the image and text may wrap.
- If you want the next section/list to start below the image (no wrap), include a page-specific style and clear the float:
  - In front matter of the page:
    - `[extra]`
    - `styles = ["home.css"]`
  - In `static/home.css`:
    - `#main-content img.start { max-width: min(320px, 40vw); height: auto; margin: 0 1rem 0.5rem 0; }`
    - `#main-content h3, #main-content ul, #main-content ol { clear: both; }`
- If you don’t want any wrapping, omit the float flag entirely (no `start=true` / `end=true`) so the image becomes a block above the text.

Style & tone
- Target a general software audience: practical, friendly, and technically accurate.
- Favor active voice, short sentences, and concrete steps.
- Include a brief conclusion with next steps or references.

Checklist before output
- Front matter starts with +++ and ends with +++.
- Contains title, date, draft, template; optional description.
- Does NOT set banner/card unless a filename is given; no booleans for `card`.
- Summary cutoff `<!-- more -->` exists after the intro paragraph.
- No H1 `#` in body; start headings at `##`.
- Only produce the file content (no surrounding explanations or fences).

Skeleton (example structure; replace with real content)
+++
title = "<Your Title>"
date = 2025-01-01
draft = false
template = "article.html"
description = "<One-line summary>"

# Optional blocks; include only if explicitly provided
#[taxonomies]
#tags = ["tag1", "tag2"]

#[extra]
#banner = "banner.jpg"   # only if the file exists next to index.md
#katex = true             # only if math is used
#toc = false
+++

Intro paragraph that summarizes the post and its value proposition.

<!-- more -->

## Background / Context
Explain the problem, prior art, or motivation.

## Main Section
Concrete explanation, steps, examples, and code where helpful.

```bash
# example terminal commands
```

## Considerations / Alternatives
Trade-offs, caveats, and when to pick a different approach.

## Conclusion
Key takeaways and suggested next steps.
