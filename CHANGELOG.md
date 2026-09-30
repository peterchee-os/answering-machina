# Changelog

All notable changes to Answering Machina are listed here.

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
