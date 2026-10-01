# The platform-neutral core

Most of Answering Machina doesn't depend on which voice AI answers the phone. The intake, the Instructions, the email rules, the knowledge base, phone forwarding, call review and the test script work the same way on any platform. This page lists that core, and what you swap per platform.

**Why the files didn't move:** the core still lives in `templates/`, `docs/`, `examples/` and `skills/`, at the same paths as v0.1. The published Grok Bot template and existing links point at those paths, so moving them would break both. This folder is the map; the per-platform guides in [`platforms/`](../platforms/README.md) say what changes.

## What's in the core

| Piece | Where it lives | Notes |
|---|---|---|
| Intake questions | [`templates/intake.md`](../templates/intake.md) | Same questions on every platform. Ignore the xAI-only lines (post-call email recipients, "Know caller's phone number") on Retell |
| Instructions with a **Key facts** block | [`templates/prompt.md`](../templates/prompt.md) | Section order and rules are platform-neutral. Tool names and the header comment are specific to xAI; see the platform guide for substitutions |
| Guardrail rules | [`templates/guardrails.md`](../templates/guardrails.md) | The rules are neutral. "Exactly 10 console guardrails" is an xAI limit. On Retell, every rule lives in the Instructions |
| Caller intents | [`templates/intents.md`](../templates/intents.md) | Neutral |
| Welcome lines | [`templates/welcome.txt`](../templates/welcome.txt), [`templates/welcome-business-hours.txt`](../templates/welcome-business-hours.txt) | Neutral. Use them as fixed text, word for word |
| Knowledge base files | [`templates/kb/`](../templates/kb/business.md) | Short Markdown files, one topic each. Both platforms accept `.md` |
| Email policy: URGENT, Message, none | [`templates/alerts.md`](../templates/alerts.md) and the Email policy section of `templates/prompt.md` | The rules are neutral (one recipient, at most one successful email per call, "alerted" only after a successful send, honest failure wording). **How** the email is sent differs per platform |
| Test script | [`templates/test-script.md`](../templates/test-script.md) | Calls 1 to 15, B1 to B10, F1 to F4 and T1 to T4 are neutral. "Free xAI test number", "Try it live" and the "(E) post-call email" check are specific to xAI |
| Fictional worked example | [`examples/sunny-desk-coworking/`](../examples/sunny-desk-coworking/README.md) | Built for xAI, but the content is a good model on any platform |
| Phone forwarding | [`docs/phone-systems.md`](../docs/phone-systems.md), [`skills/phone-forwarding/SKILL.md`](../skills/phone-forwarding/SKILL.md) | Forwarding to "the agent's number" works the same for any platform's number. The SIP section at the end is specific to xAI |
| Go-live checklist | [`docs/go-live-checklist.md`](../docs/go-live-checklist.md) | The Design, Tests and Phone system sections are neutral. The Console section is specific to xAI; the Retell guide has its own build checklist |
| Call review | [`docs/call-review.md`](../docs/call-review.md), [`skills/call-review/SKILL.md`](../skills/call-review/SKILL.md) | The review logic is neutral (one successful notification per qualifying call, flag unsupported success claims, unhandled urgent calls). The sources differ per platform |
| Knowledge refresh | [`skills/knowledge-refresh/SKILL.md`](../skills/knowledge-refresh/SKILL.md) | Neutral, except the upload steps |
| Business-hours mode | [`skills/business-hours-mode/SKILL.md`](../skills/business-hours-mode/SKILL.md) | Both patterns (one time-aware agent, or a separate daytime agent) apply on any platform |
| Recording consent | [`docs/recording-consent.md`](../docs/recording-consent.md) | Neutral. Not legal advice |
| Receptionist design | [`skills/receptionist-design/SKILL.md`](../skills/receptionist-design/SKILL.md) | Mostly neutral; the guardrail cap and connector wording are specific to xAI |

## What you swap per platform

| Topic | Grok Voice Agent Builder (xAI) | Retell AI |
|---|---|---|
| Test number | Free number provisioned by xAI | A Retell number you buy ($2/month for a US local number) |
| How the agent sends the URGENT and Message emails | Built-in Gmail connector, Send Message only | No built-in email tool. A custom function to an endpoint you control (recommended), or an MCP server. See the Retell guide |
| Where guardrails live | Up to 10 console guardrails, plus the Instructions | All in the Instructions. Retell's own "Guardrails" setting is a topic filter, not named rules |
| Current time for the Time of day section | The agent's time zone setting | A time variable in the prompt, such as `{{current_time_America/Chicago}}` |
| Caller's number | "Know caller's phone number" toggle | The `{{user_number}}` variable |
| Ending the call | `end_call` tool | End Call function |
| Per-call notification | Metadata-only "Call completed" email | No per-call email. Call History in the dashboard, webhooks, or post-call functions |
| Transcripts kept | 30 days | Forever by default; set a retention period per agent |
| Cost | About $0.09/min on the free number | About $0.13/min in the worked example, plus $2/month for the number |

Details and sources: [platforms/xai](../platforms/xai/README.md) and [platforms/retell](../platforms/retell/README.md).
