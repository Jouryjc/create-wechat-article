---
name: create-wechat-article
description: Turn source material into a polished WeChat Official Account draft article with Chinese rewriting, article illustrations, a 2.35:1 cover image that remains readable after 1:1 crop, local Doocs-style rendering, and API-first draft submission. Use when the user asks to 写公众号文章, 生成公众号草稿, 公众号排版, 配图, 封面图, Doocs 美化, or wants to turn X links, GitHub repos, webpages, notes, or plain text into a WeChat draft.
---

# Create WeChat Article

## Overview

Use this skill for the full pipeline:

1. Read and normalize source material.
2. Strengthen the source material with targeted web research.
3. Rewrite it as a Chinese long-form article.
4. Add inline illustrations.
5. Generate a WeChat-friendly cover image.
6. Render to WeChat HTML with local Doocs-compatible styling.
7. Submit to the WeChat Official Account draft box.

This skill is API-first for WeChat draft submission. Do not switch to browser posting unless the user explicitly asks for it.

## Best Fit

Use this skill when the input is any of the following:

- X / Twitter links
- GitHub repositories
- Web pages or docs
- Existing markdown drafts
- Interview notes / transcripts
- Plain text outlines that should become a WeChat article

## Skill Dependencies

Read only the pieces you need for the current source type. Resolve these by installed skill name in the current environment:

- `agent-reach`
- `baoyu-danger-x-to-markdown`
- `baoyu-url-to-markdown`
- `baoyu-article-illustrator`
- `baoyu-cover-image`
- `baoyu-post-to-wechat`
- Shared image prompt fallback: [references/gen-image.md](references/gen-image.md)
- Writing style guide: [references/xiaoyu-style.md](references/xiaoyu-style.md)
- Rendering and publishing notes: [references/workflow.md](references/workflow.md), [references/pitfalls.md](references/pitfalls.md)
- Research quality rules: [references/research-rules.md](references/research-rules.md)

## Default Conventions

Unless the user asks otherwise:

- Writing style: `码农小余`
- Article language: Chinese
- Theme: `grace`
- Illustration style: technical explainer, easy to scan, Chinese labels
- Cover image ratio: `2.35:1`
- Cover composition: important title and core subject must survive a centered `1:1` crop
- Final HTML path: `doocs-wechat-rendered.html`
- Publishing method: WeChat API first
- Image system prompt priority:
  1. Use `./gen-image.md` in the current workspace if it exists
  2. Otherwise use [references/gen-image.md](references/gen-image.md)

Recommended output layout:

```text
<workspace>/
├── article-name.md
├── doocs-wechat-rendered.html
├── imgs/
│   └── article-cover.png
├── illustrations/
│   └── <topic-slug>/
└── cover-image/
    └── <topic-slug>/
```

## Workflow

### Step 1: Normalize the source material

Choose the narrowest source-ingestion path that fits:

- X link: use `baoyu-danger-x-to-markdown`
- Web page: use `baoyu-url-to-markdown`
- GitHub repo: read the repo directly, then write your own research notes
- Existing markdown / notes: use them directly

Then run a second pass of targeted network research with `agent-reach` or another available search tool.

This second pass should be used whenever the article would benefit from:

- background context
- official documentation
- real-world examples
- ecosystem comparisons
- stronger factual grounding
- up-to-date information that may have changed recently

Requirements:

- Preserve source URL if one exists
- Preserve source author / repo / site metadata when available
- Keep the source capture or notes locally for traceability
- Treat the original source as the spine, not the whole article
- Prefer high-quality sources: official docs, repo README, maintainer posts, primary materials, strong technical writeups
- Avoid low-signal aggregation content unless it adds unique evidence
- Keep a compact source list so the final article can cite or summarize where the extra context came from

### Step 2: Write the article

Before drafting, read [references/xiaoyu-style.md](references/xiaoyu-style.md).

The article should:

- Open with the conclusion
- Explain complex ideas in plain language
- Keep technical points accurate
- Use section headings that are easy to scan in WeChat
- Add short code comments when code snippets are included
- Fold the extra research back into the main narrative instead of appending a disconnected source dump

Recommended frontmatter:

```yaml
---
title: "..."
source: "https://..."
source_author: "..."
written_style: "码农小余"
created_at: "YYYY-MM-DD"
coverImage: "imgs/article-cover.png"
summary: "..."
---
```

### Step 3: Generate inline illustrations

Use `baoyu-article-illustrator`.

Before generating prompts:

1. Check whether `<workspace>/gen-image.md` exists.
2. If it exists, use it as the shared image system prompt.
3. Otherwise use [references/gen-image.md](references/gen-image.md).
4. Keep prompts and outline files in the article workspace.

Default recommendation:

- Type: `infographic`, `framework`, `flowchart`, or `mixed`
- Density: `balanced`
- Style: editorial or friendly technical explainer
- Image text language: Chinese

After generation:

- Insert the images back into the markdown article
- Keep `outline.md` and prompt files for later regeneration

### Step 4: Generate the cover image

Use `baoyu-cover-image`.

Requirements:

- Reuse the same shared image system prompt used for inline illustrations
- Generate a `2.35:1` cover by default
- Keep title, focal object, and key symbols inside a centered `1:1` safe crop area
- Save to `imgs/article-cover.png` or a similarly explicit path
- Update article frontmatter `coverImage`

### Step 5: Render to WeChat HTML

Prefer local rendering. Read [references/workflow.md](references/workflow.md) and [references/pitfalls.md](references/pitfalls.md) first.

Preferred command:

```bash
node <x-to-wechat-article-skill-dir>/scripts/render-local-wechat-html.mjs \
  --input <workspace>/article-name.md \
  --output <workspace>/doocs-wechat-rendered.html \
  --theme grace \
  --title "<article-title>" \
  --description "<wechat-summary>"
```

Why local rendering is the default:

- More stable than `md.doocs.org`
- Avoids clipboard failures
- Avoids page DOM changes
- Still reuses the vendored Doocs-compatible renderer

### Step 6: Publish to WeChat drafts

Use the WeChat API script first:

```bash
npx -y bun <baoyu-post-to-wechat-skill-dir>/scripts/wechat-api.ts \
  <workspace>/doocs-wechat-rendered.html \
  --title "<article-title>" \
  --summary "<wechat-summary>" \
  --cover <workspace>/imgs/article-cover.png
```

Important:

- Always pass `--cover`
- Keep summary under 120 Chinese characters when possible
- Treat whitelist / `invalid ip` as environment issues

### Step 7: Final report

At the end, report:

- Source file or source URL
- Final markdown path
- Illustration directory
- Cover image path
- Final rendered HTML path
- WeChat draft `media_id` if publishing succeeded

## Output Quality Bar

The article is not done until all of these hold:

- The source material has been strengthened with high-quality external research when useful
- The markdown article reads naturally in Chinese
- The cover follows `2.35:1` + centered `1:1` safe crop
- The rendered HTML is local-Doocs based, not clipboard-dependent by default
- Ordered lists still show numbers in WeChat
- Unordered lists show a single bullet, not zero, not double
- Code blocks remain visually distinct after WeChat filtering

## Notes

- This skill is optimized for publishing, not just summarization
- Prefer fixing the pipeline instead of manually patching one article
- If the user only wants local files and not posting, stop after Step 5
- If the user already provides a rendered WeChat HTML file, skip directly to Step 6
