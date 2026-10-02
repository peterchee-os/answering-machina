# Changelog

All notable changes to Answering Machina are listed here.

## 2026-10-01: tested ChatGPT browser setup and controlled email delivery

- Added a Chrome-based Retell/Zapier walkthrough with exact URL and Authorization header fields, credential handoff, restricted email action settings and staged verification.
- Added consistent no-delivery and one-email test modes. Removed the instruction to preserve email promises in no-alert Urgent/Guardrails/Wrap-up sections.
- First tests now use an unpublished draft, Manual Chat and web voice without buying a number. Telephone acceptance remains required before live routing.
- Corrected F1–F4 setup: a permanently broken token cannot prove transient recovery or recipient isolation.
- Added a sanitized report distinguishing observed text/web/email passes from untested production, failure and permission-scope behavior. No deployment credentials or account identifiers included.

## 2026-09-30: day one at both locations (docs only)

Follow-up wording fixes the same night: the Retell route is described as documented rather than working, the recap email is labeled as an experiment outside the kit, and the console-gap lesson no longer calls email the record of truth.

README and changelog updates after the first full day live at both locations. No template or playbook changes.

- Where it stands: the AI receptionist is live on both main lines, answering after hours and taking daytime calls the desk doesn't pick up within about 20 seconds. The daytime path took its first real calls on Sep 30.
- Day-one results (Sep 30, real outside calls on the two main lines, excluding door buzzers, the owner's test calls and an automated listing-check bot): Seattle 8 calls from 5 callers, 6 answered by staff, 2 by the AI; Redmond 6 calls from 5 callers, 5 answered by staff, 1 by the AI; none went to voicemail. The AI's 3 real calls were a vendor confirming an appointment (message emailed to the team), the same vendor calling again, and a first-time caller who hung up during the greeting. No urgent or after-hours calls.
- Cost: 12 AI calls including tests, estimated from durations at $0.09 a minute at about $1.04 for the day and about $0.10 for the real calls.
- Email: on our own line we're trying a short recap email for answer-only calls, so every real call produces exactly one email to the team. This is an experiment outside the kit; the templates' email policy is unchanged (answer-only calls get no email).
- New known limits and lessons: forwarded calls show the business's own number as caller ID; the console missed some conversations for roughly six hours, so the AI's team email served as a fallback record (email can fail too); bots call too (no recap sent, correctly); join the phone system's call log with the AI's call list to get the real picture.
- New "Next: door call boxes" section: Jan 1 – Sep 30, 2026, the two door call boxes rang 3,962 times, staff answered about 3,500 (about 14.6 hours of talk time), 457 went unanswered and 563 came after hours. Door calls need their own greeting; AI door unlocking by keypad tone is untested and marked as next.

## Unreleased: multi-platform layout

Adds a second platform and more operators. Nothing existing moved, so the Grok Bot template, the Add button and existing links still work. The new material is written from public docs and isn't tested on a real line yet; unconfirmed steps are marked "TODO: verify".

- `core/`: a map of the platform-neutral core (it stays in `templates/`, `docs/`, `skills/` and `examples/`) and a table of what changes per platform.
- `platforms/`: a comparison page; `platforms/xai/` indexes the original xAI guide; `platforms/retell/` is a new Retell AI build guide (agent, prompt changes, welcome message, knowledge base, email alert options, post-call review, retention, test number, routing, transfers, costs), with sources.
- `operators/`: Grok Bot (unchanged), ChatGPT and dots (`operators/chatgpt-dot/INSTRUCTIONS.md`: a first-run owner guide plus paste-able operator instructions for any assistant), and Claude (custom connector or Claude Code with Retell's MCP server and a restricted key the owner enters).
- Email alerts on Retell: no project-built code or endpoint. The guide points to existing third-party services attached as an MCP server: Zapier MCP (Gmail Send Email, connection token in a header) or n8n (MCP Server Trigger, plus a community Gmail MCP template), with the recipient fixed in that service.
- Positioning: Grok Bot with xAI is stated as the simpler, recommended path (built-in Gmail connector, template does the setup); the Retell routes need more manual setup and a Zapier or n8n account for in-call alerts.
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
