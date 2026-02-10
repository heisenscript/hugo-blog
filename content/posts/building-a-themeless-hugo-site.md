---
title: "Building a Themeless Hugo Site"
date: 2026-02-08
description: "How to build a Hugo site from scratch with just four template files and no external theme."
tags: ["hugo", "web"]
---

You don't need a theme to build a Hugo site. Four template files are enough to get a fully functional blog.

## The four-file approach

1. **baseof.html** — The outer shell shared by every page: `<head>`, nav, footer.
2. **index.html** — The homepage, listing recent posts.
3. **single.html** — The template for an individual post.
4. **list.html** — The archive/section/taxonomy listing page.

Every other page Hugo generates — tag pages, section pages, the RSS feed — works with just these files because Hugo falls back through its [template lookup order](https://gohugo.io/templates/lookup-order/).

## The asset pipeline

Hugo can process CSS (and JS, images, etc.) through its built-in asset pipeline. Instead of linking to a static file, you use `resources.Get` to load the file from `assets/`, then apply transforms like `fingerprint`:

```go-html-template
{{ $css := resources.Get "css/style.css" | fingerprint }}
<link rel="stylesheet" href="{{ $css.RelPermalink }}" integrity="{{ $css.Data.Integrity }}">
```

This gives you cache-busting hashes and subresource integrity for free — no Webpack, no npm, no build step beyond Hugo itself.

## Why skip themes?

- Full control over every line of HTML and CSS.
- No dependency on upstream theme updates.
- Easier to understand and debug.
- The total code is small enough to read in one sitting.

For a personal blog, this is often all you need.
