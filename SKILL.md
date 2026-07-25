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
4. Run the 蒸馏小余 anti-AI-smell review and optimize the draft.
5. Add inline illustrations.
6. Generate a WeChat-friendly cover image.
7. Render to WeChat HTML with local Doocs-compatible styling.
8. Submit to the WeChat Official Account draft box.

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
- `codex-image-gen`
- `xiaoyu-wechat-article-reviewer`
- `baoyu-post-to-wechat`
- Shared image prompt fallback: [references/gen-image.md](references/gen-image.md)
- Shared image style reference: [references/xiaoyu-image-style.md](references/xiaoyu-image-style.md)
- Deep Research sketchnote image style: [references/deep-research-sketchnote-image-style.md](references/deep-research-sketchnote-image-style.md)
- Writing style guide: [references/xiaoyu-style.md](references/xiaoyu-style.md)
- Rendering and publishing notes: [references/workflow.md](references/workflow.md), [references/pitfalls.md](references/pitfalls.md)
- Research quality rules: [references/research-rules.md](references/research-rules.md)

## Default Conventions

Unless the user asks otherwise:

- Writing style: `蒸馏小余 2.0`
- Article language: Chinese
- Theme: `grace` with classic blue primary `#0F4C81`
- Illustration style: 蒸馏小余知识卡 / Deep Research Sketchnote technical explainer, matching `https://mp.weixin.qq.com/s/GaEdNZRgPV4ofNXvJsJQjQ`
- Default image style family: [references/xiaoyu-image-style.md](references/xiaoyu-image-style.md)
- Detailed reusable image design spec: [references/deep-research-sketchnote-image-style.md](references/deep-research-sketchnote-image-style.md)
- Cover image ratio: `2.35:1`
- Cover composition: important title and core subject must survive a centered `1:1` crop
- Default cover style: match the article's inline “码农小余知识图解” visual system when inline illustrations use that style
- Cover style reference: default to the 蒸馏小余奶油纸底知识卡 system; use `<workspace>/raw/640.jpeg` only when the user explicitly asks for that reference
- Final HTML path: `doocs-wechat-rendered.html`
- Publishing method: WeChat API first
- Theme spec: [references/theme-doocs-grace-classic-blue.md](references/theme-doocs-grace-classic-blue.md)
- Image generation backend: `codex-image-gen` only. Do not invoke the old `baoyu-image-gen` provider script for 蒸馏小余 article illustrations or covers.
- Image system prompt priority:
  1. Use `./gen-image.md` in the current workspace if it exists
  2. Otherwise use [references/gen-image.md](references/gen-image.md)
- Anti-AI-smell review: mandatory for 蒸馏小余 / 码农小余 WeChat drafts before rendering or publishing
- Publish source: use `article-anti-ai.md` when the reviewer creates it; keep the original draft for traceability

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

Start with the title, not the body:

- First choose the dominant title mechanism from the article itself: pain diagnosis, method replacement, benchmark result, workflow shortcut, boundary warning, reusable checklist, or capability shift.
- Generate 5 title candidates before drafting: recommended, safe, broad-audience, expert, and contrast. These candidates must use at least 4 different sentence patterns.
- Prefer titles that start from reader pain, observable behavior, a concrete outcome, or a strong engineering judgment, then introduce technical terms as the explanation path.
- Do not default to `为什么...？...` or `X 为什么 Y？从 A 到 B...`. Question titles are allowed only when the article genuinely answers a reader's live question, and at most one of the 5 candidates should be a `为什么` title.
- Good non-question structures include:
  - Action warning + method: `RAG 别再硬塞 chunk：SAG 用「事项+实体」接证据链`
  - Misconception correction: `RAG 不缺更多 chunk，缺一条证据链`
  - Method replacement: `把 chunk 变成证据链：SAG 的轻量多跳做法`
  - Result first: `Recall@2 提升 11 个点，SAG 靠的不是更大向量模型`
  - Object + outcome: `一份 MCP 配置，把项目文档变成 Agent 工具箱`
- Avoid concept-only titles such as `X 深度解读`, `一文读懂 X`, or `X 详解` unless they include a specific pain, benefit, conflict, or practitioner scenario.
- The first 200-300 Chinese characters must fulfill the title promise directly.

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
written_style: "蒸馏小余 2.0"
created_at: "YYYY-MM-DD"
coverImage: "imgs/article-cover.png"
summary: "..."
---
```

### Step 2.5: Run anti-AI-smell review

For 蒸馏小余 / 码农小余 WeChat drafts, this is a default pre-publish gate, not an optional polish pass.

Use `xiaoyu-wechat-article-reviewer` on the draft before generating the final render or sending anything to WeChat. The review must check:

- title promise and first-screen delivery
- AI-smell phrases and generic transitions
- paragraph rhythm, heading density, and WeChat scanability
- whether the article contains a reusable object such as a checklist, command set, comparison table, or decision rule
- whether there is explicit author judgment before the final third
- whether the ending has a real collect / forward / follow reason instead of a hollow CTA

Run the metrics script when available:

```bash
python3 <xiaoyu-wechat-article-reviewer-dir>/scripts/article_metrics.py \
  <workspace>/article-name.md
```

Outputs:

- Keep the original draft unchanged or only lightly corrected for traceability.
- Write the diagnosis to `<workspace>/article-review.md`.
- Write the optimized publication draft to `<workspace>/article-anti-ai.md`.
- All later image insertion, render, dry-run, and publish steps should use `article-anti-ai.md` when it exists.

Gate:

- Continue only when `ai_smell_hits` is empty, or remaining hits are quoted source text / intentional terms documented in the review.
- Resolve CTA warnings before publishing unless the user explicitly asks for a quiet ending.
- If the reviewer changes headings or structure, update image prompts and cover wording from the optimized draft, not the earlier draft.

### Step 3: Generate inline illustrations

Use `baoyu-article-illustrator` only to plan the illustration set and write reusable prompt files. Generate the actual image files with `codex-image-gen`.

Before writing prompts:

  1. Check whether `<workspace>/gen-image.md` exists.
  2. If it exists, use it as the shared image system prompt.
  3. Otherwise use [references/gen-image.md](references/gen-image.md).
  4. Also read [references/xiaoyu-image-style.md](references/xiaoyu-image-style.md) and [references/deep-research-sketchnote-image-style.md](references/deep-research-sketchnote-image-style.md) as the default visual target.
  5. If the source article contains strong hand-drawn technical diagrams, summarize that visual language into `<workspace>/gen-image.md` before generating new images.
  6. If the user provides a reference image in chat, that image's visual language overrides looser defaults.
  7. Keep prompts and outline files in the article workspace.

Default recommendation:

- Type: `infographic`, `framework`, `flowchart`, or `mixed`
- Density: `balanced`
- Style: 蒸馏小余知识卡 / Deep Research Sketchnote, editorial hand-drawn technical explainer
- Preferred look: warm cream paper background, dark navy hand-drawn outlines, pastel sticky-note cards, centered title, compact knowledge-card density, clear arrows, cute technical flowchart, Chinese labels, bottom takeaway
- Image text language: Chinese

Generate each inline illustration with:

```bash
node <codex-image-gen-skill-dir>/scripts/generate-image-with-codex.mjs \
  --prompt-file <workspace>/illustrations/<topic-slug>/prompts/<image-prompt>.md \
  --out-dir <workspace>/illustrations/<topic-slug> \
  --prefix <image-slug> \
  --timeout-ms 900000
```

For batch illustration work, run the command once per prompt file. Do not fall back to `baoyu-image-gen` if Codex image generation is rate-limited or unavailable; report the blocker and the exact prompt file that failed.

After generation:

- Insert the images back into the markdown article
- Keep `outline.md` and prompt files for later regeneration

### Step 4: Generate the cover image

Use `codex-image-gen`.

Requirements:

- Reuse the same shared image system prompt used for inline illustrations
- Reuse the same shared image style target used for inline illustrations
- Default to the 蒸馏小余知识卡 / Deep Research Sketchnote family unless the user or source material clearly requires another style
- If inline illustrations use “码农小余知识图解” style, make the cover use the same warm cream, dark-outline, rounded-card information-graphic system instead of a separate poster style
- Generate through a child Codex run, not provider-specific OpenAI / Google / Gemini image scripts
- If a local style reference is explicitly provided, inspect it first and translate the style cues into the cover prompt text before calling `codex-image-gen`
- If the user provides a style reference image in chat, that style takes priority over the generic default and should be translated explicitly into the prompt
- If the user explicitly mentions `@640.jpeg`, that reference overrides any looser style direction
- If `<workspace>/raw/640.jpeg` merely exists but the user did not ask for it, do not let it override the default 蒸馏小余奶油纸底知识卡 style
- Generate a `2.35:1` cover by default
- Keep title, focal object, and key symbols inside a centered `1:1` safe crop area
- Save to `imgs/article-cover.png` or a similarly explicit path
- Update article frontmatter `coverImage`

Default 蒸馏小余 knowledge-card visual family:

- match the visual language of `https://mp.weixin.qq.com/s/GaEdNZRgPV4ofNXvJsJQjQ`
- warm cream or light beige paper background, not a bright poster background
- dark navy hand-drawn outlines with rounded cards and containers
- low-saturation pastel blocks: pale blue, mint green, soft yellow, soft pink, light peach
- centered title, compact knowledge-card density, sparse corner doodles, clear arrows
- cute editorial flowchart composition, not poster composition
- short Chinese labels, strong readability on mobile
- friendly robots, engineer doodles, folders, webpages, docs, databases, locks, arrows, magnifiers, summary bars
- clear process or architecture explanation in one glance
- cover-specific composition: large centered title panel + one small workflow / layered structure + 2 to 4 short labels showing the article's core method
- keep all title text, subtitle text, and the central workflow inside the centered `1:1` safe crop

Legacy `raw/640.jpeg` blue cover style is opt-in. When the user explicitly asks to use it, match its visual language:

- bright sky-blue base with warm orange accents
- hand-drawn editorial tech illustration, not photorealistic
- large rounded headline area with strong central hierarchy
- light translucent content panel or card feeling
- playful gears, clouds, circuit lines, icons, and workflow symbols
- high contrast, clean Chinese title readability
- do not copy brand text or exact layout; only borrow the visual family

Preferred command pattern:

```bash
node <codex-image-gen-skill-dir>/scripts/generate-image-with-codex.mjs \
  --prompt-file <workspace>/cover-image/<topic-slug>/cover-prompt.md \
  --out-dir <workspace>/imgs \
  --prefix article-cover \
  --timeout-ms 900000
```

After the command succeeds, move or copy the selected returned image to `<workspace>/imgs/article-cover.png` if the generated filename differs, then update article frontmatter `coverImage`.

### Step 5: Render to WeChat HTML

Prefer local rendering. Read [references/workflow.md](references/workflow.md), [references/pitfalls.md](references/pitfalls.md), and [references/theme-doocs-grace-classic-blue.md](references/theme-doocs-grace-classic-blue.md) first.

Preferred command:

```bash
node <x-to-wechat-article-skill-dir>/scripts/render-local-wechat-html.mjs \
  --input <workspace>/article-anti-ai.md \
  --output <workspace>/doocs-wechat-rendered.html \
  --theme grace \
  --title "<article-title>" \
  --description "<wechat-summary>"
```

Theme requirements for the rendered HTML:

- Keep the visual structure of Doocs `grace`
- Lock the primary accent to classic blue `#0F4C81`
- Do not leave mauve / purple accent colors such as `#92617E`
- Ensure headings, strong emphasis, left borders, and other theme accents use the same classic blue
- If the renderer cannot set the primary color directly, post-process the rendered HTML before publishing

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

- Publish the optimized source: if `article-anti-ai.md` exists, the rendered HTML or submitted markdown must come from that file
- Always pass `--cover`
- Keep summary under 120 Chinese characters when possible
- Treat whitelist / `invalid ip` as environment issues

### Step 7: Final report

At the end, report:

- Source file or source URL
- Final markdown path, especially whether `article-anti-ai.md` was used
- Review report path
- Illustration directory
- Cover image path
- Final rendered HTML path
- WeChat draft `media_id` if publishing succeeded

## Output Quality Bar

The article is not done until all of these hold:

- The title points to a specific reader pain, benefit, conflict, or practitioner scenario, not just a technical concept
- Five title candidates were considered before choosing the final title, using at least 4 different sentence patterns
- `为什么` question titles were not used as the default template
- The first screen directly fulfills the title promise
- The source material has been strengthened with high-quality external research when useful
- The anti-AI-smell reviewer ran for 蒸馏小余 / 码农小余 drafts
- `article_metrics.py` results were captured when the script is available
- `ai_smell_hits` and CTA warnings are resolved or explicitly justified in `article-review.md`
- The optimized sibling draft `article-anti-ai.md` is used for render / dry-run / publish when present
- The markdown article reads naturally in Chinese
- The cover follows `2.35:1` + centered `1:1` safe crop
- Image prompts follow the 蒸馏小余知识卡 / Deep Research Sketchnote spec unless intentionally overridden
- Each generated technical image explains one concrete flow, contrast, architecture, or decision, and includes a short takeaway
- The rendered HTML is local-Doocs based, not clipboard-dependent by default
- Ordered lists still show numbers in WeChat
- Unordered lists show a single bullet, not zero, not double
- Code blocks remain visually distinct after WeChat filtering

## Notes

- This skill is optimized for publishing, not just summarization
- Prefer fixing the pipeline instead of manually patching one article
- If the user only wants local files and not posting, stop after Step 5
- If the user already provides a rendered WeChat HTML file, skip directly to Step 6
