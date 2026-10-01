# Changelog

All notable changes to Answering Machina are listed here.

## Unreleased: multi-platform layout

Adds a second platform and more operators. Nothing existing moved, so the Grok Bot template, the Add button and existing links still work. The new material is written from public docs and isn't tested on a real line yet; unconfirmed steps are marked "TODO: verify".

- `core/`: a map of the platform-neutral core (it stays in `templates/`, `docs/`, `skills/` and `examples/`) and a table of what changes per platform.
- `platforms/`: a comparison page; `platforms/xai/` indexes the original xAI guide; `platforms/retell/` is a new Retell AI build guide (agent, prompt changes, welcome message, knowledge base, email alert options, post-call review, retention, test number, routing, transfers, costs), with sources.
- `operators/`: Grok Bot (unchanged), ChatGPT and dots (`operators/chatgpt-dot/INSTRUCTIONS.md`: a first-run owner guide plus paste-able operator instructions for any assistant), and Claude (custom connector or Claude Code with Retell's MCP server and a restricted key the owner enters).
- README: a "Choose your setup" table, updated Works with, Known limits, Quick start and What's in the box, a Related projects line, and a broader not-affiliated note. The story, stats and pricing comparison are unchanged.

## v0.1.1 (unreleased)

Fixes from a second outside review. Not tagged or released yet.

- The Grok Bot template is published: the README now has an "Add Answering Machina to Grok Bot" button (https://x.ai/bot/FUSB3whX23EEO5aiyTk0P), and Quick start and Setup point to it first.
- Honest failure wording: after two failed sends, the agent says "I wasn't able to send your message to the team just now, so I can't confirm they've received it. I'm sorry about that." It no longer says the message is saved or that the team will see it. Then 911 for any danger, then `<approved alternative, if any>` (replaces `<fallback phone number>`), otherwise a request to call back during front desk hours or the next business day. The same applies to Message emails: "the team will follow up" or "passed along" only after a successful send.
- No-deals wording is now "I'm not able to arrange that, but I can take a message for the team," so it doesn't promise a hand-off before the email has sent.
- Security wording: removed the claim that the worst case is an unwanted email to your own team inbox. The one-recipient rule is prompt-level only unless an independent allowlist is configured and verified; send-only access limits actions, not recipients. Added a future hardening note (prefer an alert tool with a fixed destination) and a Known limits entry. The agent now refuses requests to email any other address.
- Call review: expect one successful notification per qualifying call; a second attempt is fine only after a failed first one. Flags missing notifications, duplicate successful sends, sends to any address other than the team inbox, and spoken success claims not backed by a successful tool result.
- Test script: failure-path calls F1 to F4 for a separate test agent (first send fails then succeeds, both urgent sends fail, a Message send fails, a caller asks for an outside address), with the expected wording, tool attempts and what the reviewer can recover; time-boundary calls T1 to T4 for the one-agent approach (around closing and opening, and a listed holiday).
- README: the observed $0.37 for our first five real phone test calls is now part of the story, with a new "Test cheaply before you trust it" guideline. The cost section still says to budget from the published rate. It also notes that xAI's smallest credit top-up is $5, the real minimum to start.
- README: "What we'll measure" (outcomes, not just "the AI answered", with a planned anonymized results report), an accuracy pass on Where it stands, costs and the review routine, and clearer wording on post-call emails and message emails.

## v0.1.0 (2026-09-29)

First public early release.

- Seven Grok Bot playbooks in `skills/`: getting-started, receptionist-design, voice-agent-setup, phone-forwarding, business-hours-mode, call-review and knowledge-refresh.
- Templates for the intake, Instructions (with a Key facts block), guardrails, intents, alerts, welcome lines, test script and a sample knowledge base, plus a complete fictional example (`examples/sunny-desk-coworking/`).
- One **Email policy** in the Instructions: one team inbox, at most one email per call, exactly one `URGENT` email for urgent calls or one `Message` email for non-urgent messages, and no email for spam, wrong numbers or answer-only calls.
- Urgent alerts distinguish success from attempt: "the team has been alerted" only after the send succeeds, one retry, then a plain failure message with 911 guidance for danger and an optional fallback phone number.
- Time-aware wording: "as soon as someone is free" during open hours, "the next business day" after hours, and never "closed" during open hours.
- Business-hours mode supports two patterns: one time-aware agent covering after hours and daytime no-answer, or a separate daytime agent (recommended when you want daytime transfers).
- Docs for phone systems, recording consent, the go-live checklist (now required, with a test on the free xAI number first and a daily health check), call review (including urgent calls with no matching URGENT email) and troubleshooting.
- README sections: Works with, Known limits, and a v0.1 early-release note.
- GitHub issue templates: a bug report form, with blank issues allowed.
