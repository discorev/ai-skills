---
name: build-html-plan
description: This skill should be used when the user asks to "make a plan", "plan for this", "build a plan", "update the plan", "upload the plan", or wants a substantial implementation approach documented for review.
argument-hint: [plan request or .plans/path.html]
---

# How to write a plan

Plan files should be a single-file HTML with inline CSS, not markdown. Prefer rich diagrams and visual components over verbose explanation. Use the frontend-design skill; artistic liberty is welcome within the mood below. If the repo already has `.plans/*.html` in this style, reuse its CSS variables and components rather than rebuilding from scratch. While we're still aligning on a plan, keep the discussion conversational.

## Mood

warm editorial/print, like a well-typeset magazine — ivory paper background, white cards with soft warm-grey borders, one warm accent colour plus a muted green for positive marks; serif headings, sans body, mono for labels/code and uppercase "eyebrow" section labels. Light-mode only. (Baseline palette if unsure: #FAF9F5 ivory, #D97757 clay accent, #788C5D olive.)

## Structure
- Break content into components, not prose: term/definition card grids instead of bare tables, pipeline step strips, checklists with ✓/✗ marks, accent-bordered callout boxes, code blocks with numbered margin notes.
- Structure = "tweakable plan": sort sections by likelihood-of-tweaking, NOT execution order. Section A = judgment calls I'm likely to change (data model, new interfaces, anything user/author-facing), each shown as a choice card with the plan's pick vs a toggleable real alternative. Section B = sequencing. Section C = mechanical work, collapsed in a `<details>` ("trust me"). Header carries effort/files/risk chips; end with a "tweak these three things" card of copyable one-line replies.
- If the plan can name its own "weakest part", promote that concern to a Section-A decision with a real alternative — never leave it as a callout while the mechanism causing it goes unquestioned.

# Upload

After creating or explicitly updating the plan, invoke the `planreview-upload` skill with the HTML file path. Skip this only when the user requests a local-only, draft, or no-upload result.
