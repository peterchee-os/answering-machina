# Voice platforms

Answering Machina's receptionist runs on a voice AI platform: the service that answers the phone, talks to the caller and runs the agent's tools. The design (Key facts, Email policy, guardrails, tests) is the same on each. See [the core](../core/README.md).

**Which one?** xAI with Grok Bot is the simpler path and the one we recommend: the voice agent sends in-call alerts through its built-in Gmail connector, and the Grok Bot template builds everything. Retell AI works with ChatGPT and Claude and has features xAI's Builder doesn't (such as warm transfers), but it takes more manual setup, and in-call email alerts need a third-party automation account (Zapier or n8n).

| | [xAI Grok Voice Agent Builder](xai/README.md) | [Retell AI](retell/README.md) |
|---|---|---|
| Status in this kit | **Tested.** Running on one real business line since 2026-09-29 | **Documented, not yet tested by us.** Written from Retell's public docs, checked 2026-09-30 |
| Usual operator | Grok Bot ([template](https://x.ai/bot/FUSB3whX23EEO5aiyTk0P)) | ChatGPT or a ChatGPT dot ([instructions](../operators/chatgpt-dot/INSTRUCTIONS.md)); Claude or another MCP assistant ([Claude](../operators/claude/README.md)) |
| Test number | Free xAI number | Retell number, $2/month (US local) |
| Published voice rate | $0.08/min audio + $0.01/min on the free number | $0.07 to $0.31/min for voice AI, depending on the model and voice, plus telephony ($0.015/min on a Retell US number) |
| Email from the agent during a call | Built-in Gmail connector (send-only) | No built-in email. Attach a Zapier or n8n MCP server (third-party account) |
| Knowledge base | File collection, searched by a tool | Knowledge base retrieved automatically on every turn ($0.005/min) |
| Transfers | Cold only (SIP REFER) | Cold, warm and agentic warm |
| Per-call notification | Metadata-only "Call completed" email | None built in; webhooks or post-call functions |
| Conversations kept | 30 days | Forever unless you set a retention period |

You can mix and match: the operator (Grok Bot, ChatGPT, a dot, Claude, or you by hand) is separate from the voice platform. See [operators](../operators/README.md). The pairings above are the ones this kit documents.

Prices change. Check [xAI pricing](https://docs.x.ai/developers/pricing) and [Retell pricing](https://www.retellai.com/pricing) before you budget.
