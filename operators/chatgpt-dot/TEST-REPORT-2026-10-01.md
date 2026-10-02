# Retell setup test report: 2026-10-01 (private details removed)

Kit version tested: `4e6d83129338505db58c704db1e15971772412d5`. Run by: a ChatGPT Dot working in the owner's own Chrome, already signed in. Business: fictional Sunny Desk Coworking. We left out account addresses, workspace and agent IDs, provider message IDs and credentials. This report says what we saw. It doesn't certify the setup for real calls.

## What we set up, in order

- Created a separate unpublished single-prompt draft with GPT 5.6 Terra, the Cimo voice, English (US), a fixed custom greeting and `end_call`.
- Attached a processed fictional knowledge base for the no-delivery tests. Contact memory off. Seven-day retention, checked after a reload.
- Sent 14 typed test messages across several conversations. Early tests shared one conversation; later ones each used a fresh chat. We fixed the missing name and number confirmation and the missing final recap, then retested the message-taking path. We didn't rerun every test after the last small fix.
- The owner finished a web voice test of about 49 seconds. An earlier web test started by the assistant was stopped when the owner left; we didn't count it as a passed conversation test.
- Created a dedicated Zapier MCP server in Managed mode with Gmail Send Email only: recipient fixed, and CC, BCC and attachments turned off. The owner approved Gmail access and entered the token directly. We didn't separately check the final Gmail permissions (OAuth scopes).
- Fixed the missing `Bearer ` prefix after the tool list hung on Loading. Retell then listed `gmail_send_email`. We attached only that tool, not the helper tools.
- For the one approved synthetic email, we swapped in a limited delivery-test prompt and greeting and detached the no-alert KB (its source was kept). This was a separate test setup, not the full receptionist prompt used earlier.

## What happened

| Area | Result and limits |
|---|---|
| Text facts | Answered weekends, member access, hours and holidays, and parking in the scenarios we tested |
| Text guardrails | Tested turning down pricing, discount and booking requests; prompt injection; access codes; membership privacy; and lockouts |
| Emergency wording | Immediate-danger test gave 911 first; self-harm test gave US 988/911 guidance and stayed until the caller was ready |
| Privacy | When the caller objected to recording, it stopped taking details, didn't claim the recording was deleted or turned off, and ended the call |
| Intake | After fixes, the targeted retest passed: fictional name spelled back, callback read back, a clear yes, and a recap of name, number and reason |
| Web voice | Greeting, transcription, basic conversation, staying on the fictional business, and end_call all seen; the owner said it worked. Emergencies, message-taking and interruptions weren't tested by voice |
| Delivery | One email, triggered by the model, at 18:15:12 Pacific. Retell's tool response had a Gmail message ID and the SENT label; Zapier separately showed Success and the same ID; the owner sent a matching Inbox screenshot |
| Honest success wording | The agent said the email service accepted the message for sending; it didn't claim the email reached the inbox before that was confirmed |

The delivery response also had an output-filter warning, while still returning the unfiltered Gmail success result. Zapier History and the inbox confirmed the send. We didn't try a second send.

## Costs we saw (2026-10-01)

- Retell started with $10 of trial credit and showed $9.49 after the whole session, including earlier text and web tests. That's not the cost of one email.
- The credit shown went from $9.54 before the delivery test to $9.49 after. That's the change in balance we saw, not a worked-out per-message bill.
- Manual Chat showed $0.0208 per message. Voice estimates changed as settings and context changed, so don't treat them as a fixed quote.
- The successful Zapier email used two tasks; usage went from 0/100 to 2/100.
- We didn't need to buy a number, add a card, upgrade, publish or change any live routing.

## Still unverified

Phone and forwarded-call tests, caller ID, normal message and urgent email flows by voice, the F1–F4 failure, recovery and isolation tests, lasting duplicate prevention, rerunning the whole test suite after changes, n8n, what the Retell ChatGPT plugin can do, and the final Gmail OAuth scopes. The one working email test doesn't prove any of these.

To repeat the setup, follow the [walkthrough](TESTED-WALKTHROUGH.md). Then run the [full test script](../../templates/test-script.md) with the [Retell-specific failure setups](../../platforms/retell/test-modes.md#failure-and-isolation-tests) before sending any real calls to the agent.
