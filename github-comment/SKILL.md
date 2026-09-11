---
name: github-comment
description: comment on github issue, comment on github pr, gh comment, pr review, pr comment, review comment
---

# Comment convention

When you comment on an existing github issue or pull request (not when opening one), add a note at the start of the comment that says which model you are and that you are commenting on my behalf.

```
> [!NOTE]
> 🤖 **{Model name} responding on behalf of Ollie**

{content}
```

The model name should clearly identify which model you are e.g. Claude Fable 5.1, GPT 6 Astra, Grok 4.6. Post with `--body-file` from a heredoc, and keep the blank line after the note to ensure that the syntax doesn't get broken.
