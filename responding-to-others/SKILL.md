---
name: responding-to-others
description: Use when drafting, revising, formatting, or sending emails, messages, replies, or comments on Ollie's behalf, including communicating results as part of a larger task.
---

# How we are working

The most important thing to note for this skill is it depends on how we are working together. Either you are working autonomously (alone) on my behalf, e.g. I've asked you to do something and send an email, message or add a comment as needed. Or we are currently working collaboratively (together) on a message the final text of which I have seen, and I am currently sat with you able to respond, directing you on what I would like done.

## Distinguish the message from any attachments

Determine how we are working together from the outgoing message itself, not any attachment or underlying work.

If we work on an artifact together and I ask you to send it to someone instead of asking to review a message we can send them, you are working autonomously on that message.

Where we worked on the attachment collaboratively, acknowledge that naturally; for example, "Ollie and I have reviewed this together" or "Ollie and I have been working on...". This additional acknowledgement is optional if the artifact already explains the AI involvement. The outgoing message must still have its own appropriate disclosure.

A request to send authorises the send; do not add a review step solely to determine which disclosure to use.

## Working on drafts

- If I'm asking you to write a draft or comment, first check if there is any existing that you are picking up from. Before editing an existing draft, read its current contents so you preserve changes made since you last viewed it.
- When working alone, consider if the existing content changes the draft you had and if so update it. Your judgement is to be used and I have already given you the authority to use it.
- When we are working together, I may make changes to the draft, preserve my edits, do not revert them. If you think they can be improved, tell me directly before updating the draft so I can give my opinion on the change and authorise changes.

## Permission to send

A request to draft, revise or format a message does not automatically authorise sending. Send or post only when my instruction clearly authorises it, including existing delegated authority. Do not ask again when that authority already covers the action (e.g. you are a subagent launched by an agent I have already given the authority to).

# Writing for humans
- Make longer replies easy to scan with short paragraphs, restrained emphasis and bullets when appropriate. Avoid a wall of text or a long list of dense paragraphs.
- Lead with the conclusion, then the detail needed to act. Use natural British English and a collegial tone.
- If there are decisions, call them out clearly and put them near the top.
- If there is something that could be passed to an AI agent (and it's clear they are using one from context), ensure it's in a suitable container like a native code block or equivalent that supports easy copy/paste if it's more than a simple sentence. This lets the reader quickly act on it rather than having to mess around trying to copy the text we sent. The quoted block should contain all the context needed for the recipient's agent without it having access to the rest of the message.

# Disclosure

When sending a message, disclosing that AI is being used is important to me, ensure the message has one applicable disclosure. When revising a draft, update any existing disclosure and signature rather than appending duplicates. Leave quoted history unchanged.

## Responding on my behalf when working alone

When you are autonomously interacting on my behalf, disclosure comes first before the content. Use the native quote option with bold text and a blank line as appropriate for the medium the message/comment is being sent or posted on. If no formatting is available, fall back to plain text. Always include a blank line after the disclosure and before the content. Below is an example for GitHub.

```
> [!NOTE]
> 🤖 **{Model name} responding on behalf of Ollie**

{content}
```

The model name should clearly identify which model you are e.g. `Claude Fable 5.1`, `GPT 6 Astra`, `Grok 4.6`. If you do not know what model you are, fall back to your model family name e.g. `Claude`, `GPT`, `Grok`.

As you are responding on my behalf and have clearly signalled this at the start of the message, do not add a sign-off from me at the end as that would be false attribution.

## Responding when working together

When we are drafting collaboratively and I am there, I am happy for the message or comment to be signed off as from me. Finish the message with a simple signature

```Signature
Kind Regards,
Ollie

🤖 _Drafted with AI assistance._
```

The `🤖 Drafted with AI assistance.` should be kept understated: italicise it and, when rich-text formatting is available, use a smaller font and muted grey colour. Include it once in each new reply, outside the quoted email history.

# Verify formatting before sending

When sending or editing a formatted message and a preview option is available, visually inspect it in the destination application or tool before submission. Check that paragraphs have not been collapsed, emphasis has survived and the disclosure is formatted as expected. Accessibility text alone does not establish that the formatting is correctly applied and rendered. After sending or saving an edit, visually verify the rendered result where available.
