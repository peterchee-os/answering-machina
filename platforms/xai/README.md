# Platform guide: xAI Grok Voice Agent Builder

This is the platform Answering Machina was built and tested on. The guide lives in its original files, which this page links to rather than moves, so the published Grok Bot template and existing links keep working.

**Status:** tested on one real business line since 2026-09-29. The Builder is in beta, and its screens change.

## Where everything is

| Step | File |
|---|---|
| Build the agent in console.x.ai: template, Instructions, the 10 guardrails, Welcome message, caller-ID toggle, file collection, `end_call`, the send-only Gmail connector, the free test number, post-call notifications, phone tests | [`skills/voice-agent-setup/SKILL.md`](../../skills/voice-agent-setup/SKILL.md) |
| Manual console steps (no assistant) | [README: Manual use](../../README.md#manual-use-without-grok-bot) |
| Console checklist items | [`docs/go-live-checklist.md`](../../docs/go-live-checklist.md) (Console section) |
| Bring your own number over SIP | [`docs/phone-systems.md`](../../docs/phone-systems.md#bring-your-own-number-direct-sip) |
| Post-call email and Conversations tab | [`docs/call-review.md`](../../docs/call-review.md) |
| Troubleshooting (Try it live, pasted Instructions, missing emails) | [`docs/troubleshooting.md`](../../docs/troubleshooting.md) |
| Costs and what we haven't verified | [README: Cost details](../../README.md#cost-details), [`docs/research/pricing-factcheck.md`](../../docs/research/pricing-factcheck.md) |

## Facts that are specific to xAI

- **Free test number**, with $0.01/min telephony on top of the $0.08/min audio rate. Unused numbers are released after 30 days without calls (per the console).
- **At most 10 console guardrails**, each with a Name and a Description.
- **Gmail connector** with only Send Message enabled sends the URGENT and Message emails. The one-recipient rule is prompt-level only.
- **"Call completed" email** from xAI after each phone call over the minimum duration: metadata only, up to 3 recipients, never for Try it live.
- **Conversations** (recording, transcript, tool calls) are kept 30 days.
- **Transfers are cold** (SIP REFER).

The operator for this platform is usually Grok Bot: see [operators/grok-bot](../../operators/grok-bot/README.md).
