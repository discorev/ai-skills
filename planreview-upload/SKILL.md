---
name: planreview-upload
description: This skill should be used when the user asks to "upload the plan", "publish the plan", "upload to planreview", "share the HTML plan", or wants an existing HTML plan uploaded for review.
argument-hint: <path-to-plan.html>
---

# PlanReview Upload

Upload by default after creating or explicitly updating/re-presenting the plan. Skip upload only when the user asks for a local-only, draft, or no-upload result.

Run the upload directly in JSON mode:

```bash
planreview upload "$PLAN_PATH" --json
```

Do not run `planreview help` or `planreview upload --help`; the required interface is documented here.

The command accepts exactly one HTML path. Preserve its default canonical-path mapping:

- Do not pass `--new` during normal creation or revision. Reusing the same canonical path updates the mapped plan.
- Do not pass `--plan` unless the user explicitly supplies a plan ID or requests targeting a different existing plan.
- Do not pass `--static` unless the user explicitly wants scripts disabled. The CLI detects executable inline content automatically.
- Keep the default `--fonts auto` behavior unless another font mode is explicitly requested by me.

On success, stdout is one JSON object with this shape:

```json
{
  "planId": "plan-id",
  "version": 1,
  "execution": "static",
  "latest": "https://planreview.dev/...",
  "permalink": "https://planreview.dev/...",
  "identitySource": "new"
}
```

Parse stdout as JSON. Return:

1. the clickable `latest` URL prominently as the main review link
2. the version
3. any issues detected during upload that may prevent the plan behaving as expected.
4. the local HTML path

If upload fails, report the actual error faithfully and keep the local plan path available. Suggest `planreview login` only when the error indicates authentication is required. Do not claim that a link exists when publishing did not succeed.
If auto classifier blocks the upload, ask the user to confirm they are happy with the upload - do this as the only question in that turn, save any detail on areas they may wish to review until after they have responded.
