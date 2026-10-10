# khawzk.github.io

Personal site for Mark Khaw (Khaw Zhe Kang), GenAI and cloud solution architect, served by GitHub Pages at https://khawzk.github.io/.

- `index.html` is the homepage: one page with inline CSS and JS. Its only Liquid is the "Notes" section near the end, which lists the three newest posts.
- `cv.pdf` is the resume linked from "Download CV". Replace the file to update it.
- The carousel's skill-radar slide reads `/SGJobRadar/data/latest.json` (published by the [SGJobRadar](https://github.com/khawzk/SGJobRadar) Pages site) and falls back to a snapshot built into `index.html`.
- The notes (blog) live at `/blog/` and are built by GitHub Pages' Jekyll.

## Writing a note

Add a Markdown file to `_posts/` named `YYYY-MM-DD-short-slug.md`. You can do this from the GitHub web editor; the site rebuilds on commit.

```markdown
---
title: Pricing video generation by usable output
lang: en            # en or zh
ref: video-yield    # optional: give the English and 中文 versions the same ref to link them
description: One sentence shown in the list and in link previews.
tags: [genai, pricing]
---

Write the note here in Markdown.
```

- A Chinese note uses `lang: zh`. Use an English slug with a `-zh` suffix for the file name, e.g. `2026-10-12-video-yield-zh.md`.
- Two notes with the same `ref` get a "Read in English / 阅读中文版" link to each other.
- Images go in `assets/` and are referenced as `/assets/name.png`.
- The feed is at `/blog/feed.xml`.

## Previewing locally

```sh
gem install jekyll -v 3.10.0 && gem install jekyll-feed kramdown-parser-gfm webrick
jekyll serve
```
