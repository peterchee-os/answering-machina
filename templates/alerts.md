# Urgent alerts and message emails: <Business>

xAI's post-call email is metadata only (no summary, no urgency flag), so the agent sends its own emails. The rules live in one place, the **Email policy** section of `prompt.md`; this file records the design.

## Design
| Item | Value |
|---|---|
| Sending mailbox | <sending mailbox> (dedicated; the owner controls it and signs in to the console's Gmail connector themselves) |
| Connector tools enabled | **Send Message only** (everything else off) |
| Recipient | <team inbox> (the ONE address the agent is told to email; a group is fine). Prompt-level only unless an independent allowlist is configured and verified (see Risk) |
| Cases (mutually exclusive) | Urgent call: one successful `URGENT: <Location>` email. Non-urgent call with a message: one successful `Message: <Location>` email. Spam, sales pitches, robocalls, wrong numbers, silent and answer-only calls: no email |
| Limit | At most one successful email per call. A second attempt only after a failed first attempt; never resend after a success |
| Body | Plain text, under 300 characters, no links |
| Urgent body | `Urgent call. Caller: <name>, <number>. Issue: <one sentence>. Called <day date> at <time> <time zone>.` |
| Message body | `Message. Caller: <name>, <callback number>. Message: <one-sentence reason>. Called <day date> at <time> <time zone>.` |
| When | As soon as name, number and issue (or reason) are confirmed, before the wrap-up |
| Urgent criteria | <e.g. lockout; leak or flood; break-in; power or heating failure; caller says emergency> |
| Approved alternative | <approved alternative, if any> or "none" (for example, another number that a person answers). Offered only after both tries of a send fail. Must never route back to the agent. With "none", callers are asked to call back during front desk hours (open hours) or the next business day (after hours), plus 911 for any danger |
| Who reads alerts | <people on the group>; how they're notified (phone push, forwarding) |

## Success vs attempt
The agent says "I've marked this urgent" while it sends. It says "the team has been alerted" **only after the send tool returns success**. If the send fails, it retries once. If that fails too, it doesn't say the message is saved or that the team will see it, because neither is true. It says: "I wasn't able to send your message to the team just now, so I can't confirm they've received it. I'm sorry about that." Then it tells anyone in danger (fire, medical, break-in, threats) to call 911, and offers the approved alternative if there is one; otherwise it asks the caller to call back during front desk hours (open hours) or the next business day (after hours). It never claims an alert was sent when it wasn't.

The same honesty applies to Message emails: the agent says the team will follow up only after the Message send succeeds, and never says "I've passed that along" when it hasn't. After two failed tries it uses the same failure wording.

What's left after a failed send: the transcript in Conversations (the spoken recap has the name, number and reason, and the tool calls show each failed attempt) and the provider's "Call completed" email (caller number, time and a link). That's enough for a person to follow up, but only if someone looks, so the daily review treats every failed send as something to act on. The daily call review catches urgent calls that have no matching URGENT email (see `docs/call-review.md`).

## Example (fictional)
`Urgent call. Caller: Jordan Lee, 555-555-0100. Issue: Locked out of suite, door app not working. Called Saturday May 2 at 9:14 PM Pacific.` (138 characters)

## Risk
Anyone who can talk to the agent can trigger its enabled connector tools, and a caller can try to talk it into things. Two different controls are at work here, and they do different jobs:
- **Send-only access** limits which actions are available. With only Send Message enabled, the agent can't read, search, forward, reply to or delete mail. It doesn't, by itself, control who an email goes to.
- **The recipient rule** ("send only to <team inbox>") lives in the Instructions. The agent is instructed to email only the team inbox, but that's a prompt-level restriction. Unless an independent recipient allowlist is configured outside the agent and verified by a test, a persuasive caller or a model mistake could get an email sent to another address, with text the caller influenced.

So the worst case is not just "an unwanted email to your own team inbox". Keep the connector send-only (don't enable read, forward, reply or delete tools), use a dedicated sending mailbox with nothing sensitive in it, keep bodies short and plain, run the "email my outside address" test in the test script, and check the sending mailbox's Sent folder during the daily review.

**Future hardening:** prefer an alert tool with a fixed destination over letting the model choose the recipient. Examples: a webhook tool that can only post to one endpoint, a sending account that can only deliver to one address, or a mailbox or mail-flow rule that blocks anything not addressed to the team inbox. We haven't tested any of these with the voice agent yet. Whatever you use, verify it with a test call that asks for an outside address. Until then, list "Recipient restriction is prompt-level only" in your known limits.

## Tests
- [ ] Lockout by phone: one successful URGENT email, correct subject, body under 300 characters, no Message email; "the team has been alerted" said only after the send
- [ ] Leak by phone: one URGENT email, no 911 advice unless someone is in danger
- [ ] Smoke: "dial 911" first; alert only if name or number was given
- [ ] Pricing message by phone: one successful Message email
- [ ] Normal call (hours question): no email
- [ ] Recipients actually see it (phone notification, group delivery)
- [ ] Failure paths on a separate test agent (see `templates/test-script.md`): first send fails then succeeds; both urgent sends fail; a Message send fails; a caller asks for an outside address (refused)
