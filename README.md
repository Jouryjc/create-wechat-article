# create-wechat-article

Turn source material into a polished WeChat Official Account draft article.

This skill is designed for workflows like:

- turning an X thread into a long-form WeChat article
- turning a GitHub repo deep dive into a publishable公众号草稿
- turning notes, docs, or markdown into a fully rendered WeChat draft

## What It Does

The skill covers the full pipeline:

1. Normalize the source material
2. Strengthen the source material with targeted web research
3. Rewrite it as a Chinese article
4. Generate inline illustrations
5. Generate a WeChat-friendly cover image
6. Render with a local Doocs-compatible pipeline
7. Submit to the WeChat Official Account draft box

## Defaults

- Writing style: `码农小余`
- Article language: Chinese
- Theme: `grace`
- Cover aspect ratio: `2.35:1`
- Square-safe crop: required for `1:1` thumbnail compatibility
- Publishing: API-first

## Why This Skill Exists

This workflow was extracted from a real publishing pipeline and already includes fixes for common failure modes:

- weak source material that needs stronger background research
- online Doocs instability
- WeChat CSS filtering
- weak local-to-WeChat style transfer
- broken ordered/unordered list markers
- duplicate bullet markers
- cover images that look fine wide but break under square crop

## Files

- [SKILL.md](./SKILL.md): primary skill instructions
- [references/workflow.md](./references/workflow.md): end-to-end workflow summary
- [references/pitfalls.md](./references/pitfalls.md): production pitfalls and fixes
- [references/research-rules.md](./references/research-rules.md): source-enrichment rules
- [references/xiaoyu-style.md](./references/xiaoyu-style.md): writing style summary
- [references/gen-image.md](./references/gen-image.md): shared image-generation guidance
- [templates/article-frontmatter.md](./templates/article-frontmatter.md): markdown frontmatter template
- [templates/final-report.md](./templates/final-report.md): suggested completion report

## Expected Dependencies

This skill assumes these companion skills are installed in the runtime environment:

- `agent-reach`
- `baoyu-danger-x-to-markdown`
- `baoyu-url-to-markdown`
- `baoyu-article-illustrator`
- `baoyu-cover-image`
- `baoyu-post-to-wechat`

## Publishing Notes

For skillhub, this skill is intentionally written as a reusable workflow description rather than a machine-specific project script bundle.

If you want, you can further package local helper scripts alongside this directory before publishing.
