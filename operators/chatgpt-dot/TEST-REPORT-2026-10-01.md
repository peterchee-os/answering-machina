# Sanitized Retell setup report — 2026-10-01

Source kit: `4e6d83129338505db58c704db1e15971772412d5`. Operator: ChatGPT Dot workflow using the owner's existing local Chrome session. Business: fictional Sunny Desk Coworking. Account addresses, workspace/agent IDs, provider message IDs and credentials are omitted. This report records observed behavior, not production certification.

## Configuration and sequence

- Created a separate unpublished single-prompt draft, with GPT 5.6 Terra, Cimo voice, English (US), fixed custom greeting and `end_call`.
- Attached a processed fictional knowledge base for no-delivery tests. Contact memory off; seven-day retention verified after reload.
- Ran 14 manual user messages across several conversations. Early tests shared context; later cases used fresh chats. Fixed name/number confirmation and final recap omissions and retested the intake path; did not rerun the entire suite after the final narrow fix.
- Owner completed a roughly 49-second web voice test. An earlier worker-started web test was stopped when the owner left; it was not counted as a successful conversation test.
- Created a dedicated managed Zapier MCP server with Gmail Send Email only; recipient fixed and CC/BCC/attachments excluded. Owner approved Gmail access and entered the token directly. Final OAuth scopes were not independently audited.
- Corrected the missing `Bearer ` prefix after tool discovery hung on Loading. Retell then listed `gmail_send_email`; attached only that tool, not the metadata helpers.
- For one authorized synthetic delivery, replaced prompt and greeting with a limited delivery test and detached the no-alert KB (preserved its source). This was a distinct test configuration, not the earlier full receptionist prompt.

## Observed results

| Area | Result and limits |
|---|---|
| Text facts | Weekends, member access, hours/holidays and parking answered in the tested scenarios |
| Text guardrails | Tested pricing/discount/booking refusal, prompt injection, access codes, membership privacy and lockout handling |
| Emergency wording | Immediate-danger test gave 911 first; self-harm test gave US 988/911 guidance and stayed until ready |
| Privacy | Recording-objection test stopped intake, did not claim deletion or disabled recording, and ended |
| Intake | After corrections, fictional name spelling, callback readback, explicit confirmation and name/number/reason recap passed the targeted retest |
| Web voice | Greeting, transcription, basic conversation, fictional-business scope and end_call observed; owner said it worked. No voice emergency/intake/interruptions coverage established |
| Delivery | One model-triggered email at 18:15:12 Pacific; Retell tool response contained Gmail message ID and SENT label; Zapier independently showed Success and the same ID; owner supplied matching Inbox screenshot |
| Honest success wording | Agent said the email service accepted the message for sending; did not claim inbox receipt before confirmation |

The delivery response also contained an output-filter warning while returning the unfiltered Gmail success result. Zapier History and inbox receipt confirmed the send. No duplicate retry was attempted.

## Dated cost observations

- Retell began with $10 trial credit and showed $9.49 after the complete session, including earlier text and web tests. This is not the cost of one email.
- Before/after the delivery-stage test, displayed credit changed from $9.54 to $9.49. This is an observed balance difference, not a reconstructed per-message invoice.
- Manual Chat displayed $0.0208/message. Voice estimates varied as configuration/context changed; do not reuse them as a guaranteed quote.
- The successful Zapier email consumed two tasks; usage went from 0/100 to 2/100.
- No number purchase, card addition, paid upgrade, publication or live routing change was needed.

## Still unverified

Telephone/forwarded-call acceptance, caller ID, normal message and urgent-email flows over voice, F1–F4 failure/recovery/isolation tests, durable duplicate prevention, whole-suite regression after changes, n8n, Retell ChatGPT plugin capabilities, and final Gmail OAuth scopes. The working email smoke test does not establish these.

Follow the [walkthrough](TESTED-WALKTHROUGH.md) to reproduce setup, then complete the [full test script](../../templates/test-script.md) with the [Retell-specific failure setups](../../platforms/retell/test-modes.md#failure-and-isolation-tests) before any production routing.
