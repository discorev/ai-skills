---
name: html-outcome
description: report, give me a report, write-up, summary of work, outcome, findings, show me the plan, analysis, comparison, prototype, mockup, explainer, walkthrough, tutorial, guide, teach me, educational page, html page, html report
---

# When to use

When the user asks for a substantial, standalone piece they will read later (a report, analysis, prototype, mockup or explainer), the deliverable is an HTML file, even if they don't say "HTML". "Once done, give me a report" means build the file. If the user asks for a different format, use that.

Quick answers, alignment questions and back-and-forth discussion stay in chat. A prototype built inside the real app is just code, not an outcome.

Plans are aligned in chat. Only build a plan outcome when the user asks to see the plan written up; plans written to organise your own work stay as plain working files and are never uploaded.

# How to write the outcome

A single self-contained HTML file: inline CSS and JS, with images embedded as data URIs. Use the frontend-design skill, and the dataviz skill for charts when it is available.

Choose a look that suits the subject and the audience instead of a house style. When revising an existing outcome, keep its styling. Never copy the styling from unrelated artefacts.

The user is short on time. Diagrams, tables and visual components beat prose, and a wall of text will not get read. Put the most important information first, along with anything the user might want to change or disagree with.

## By type

- **Report on finished work.** Open with what changed and whether it works, and link the evidence (commits, PR, screenshots, test output). Then cover:
  - Consequential decisions made on the user's behalf. For each, give the pick, a real alternative, and how hard it would be to reverse now.
  - What isn't done, wasn't verified, or failed. This section is required and goes near the top; if nothing is outstanding, say so in one line. Never oversell.
  - Anything that needs the user's action or decision.
- **Plan.** Sort sections by how likely the user is to change them, not by execution order. Judgement calls come first, each shown as the pick vs a real alternative. Sequencing comes next. Mechanical work goes last, collapsed. If the plan has a weakest part, make it a decision with an alternative instead of a footnote.
- **Analysis or comparison.** Lead with the conclusion and the recommendation, then the evidence. Make clear what is measured and what is inferred.
- **Prototype or mockup.** The file is the product, so make it interactive where that helps. Add a short note on what is real, what is faked, and the open design questions.
- **Explainer or educational page.** Build from intuition to detail, with diagrams and worked examples. Interactive elements are welcome where they make a concept clearer.

# Where to save

Pick the location by type, relative to the repo root (or the working directory when there is no repo):

- Plans go in `.plans/<short-slug>.html`.
- Reports, analyses, prototypes, mockups and one-off explainers go in `.outcomes/<short-slug>.html`. Keep `.outcomes/` out of git by adding it to the local exclude file (`git rev-parse --git-path info/exclude`), not `.gitignore`. Only commit an outcome when the user asks.
- Pages meant as repo documentation go wherever the repo keeps that material, such as `docs/`.

Before creating a file, check `.plans/` and `.outcomes/` for one on the same subject and revise it instead. Reuse the same path for revisions.

# Check

Open the page at desktop and narrow widths and try any interactive parts. Fix anything broken before uploading.

# Upload

After creating or updating the file, invoke the `planreview-upload` skill with its path. Skip this only when the user asks for a local-only, draft or no-upload result.

In chat, give the link, the local path and a one or two sentence headline. Don't paste the report into chat.
