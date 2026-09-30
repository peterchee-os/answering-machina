# Urgent alerts and message emails: <Business>

xAI's post-call email is metadata only (no summary, no urgency flag), so the agent sends its own emails. The rules live in one place, the **Email policy** section of `prompt.md`; this file records the design.

## Design
| Item | Value |
|---|---|
| Sending mailbox | <sending mailbox> (dedicated; the owner controls it and signs in to the console's Gmail connector themselves) |
| Connector tools enabled | **Send Message only** (everything else off) |
| Recipient | <team inbox> (the ONE address the agent may email; a group is fine) |
| Cases (mutually exclusive) | Urgent call: exactly one `URGENT: <Location>` email. Non-urgent call with a message: exactly one `Message: <Location>` email. Spam, sales pitches, robocalls, wrong numbers, silent and answer-only calls: no email |
| Limit | At most one email per call. A retry after a failed send isn't a second email |
| Body | Plain text, under 300 characters, no links |
| Urgent body | `Urgent call. Caller: <name>, <number>. Issue: <one sentence>. Called <day date> at <time> <time zone>.` |
| Message body | `Message. Caller: <name>, <callback number>. Message: <one-sentence reason>. Called <day date> at <time> <time zone>.` |
| When | As soon as name, number and issue (or reason) are confirmed, before the wrap-up |
| Urgent criteria | <e.g. lockout; leak or flood; break-in; power or heating failure; caller says emergency> |
| Fallback phone number | <fallback phone number> or "none". Offered only if an urgent send fails twice. Must never route back to the agent |
| Who reads alerts | <people on the group>; how they're notified (phone push, forwarding) |

## Success vs attempt
The agent says "I've marked this urgent" while it sends. It says "the team has been alerted" **only after the send tool returns success**. If the send fails, it retries once. If that fails too, it says plainly that it couldn't reach the team right now, tells anyone in danger (fire, medical, break-in, threats) to call 911, and otherwise offers the fallback phone number or says the message is saved and the team will see it. It never claims an alert was sent when it wasn't. The daily call review catches urgent calls that have no matching URGENT email (see `docs/call-review.md`).

## Example (fictional)
`Urgent call. Caller: Jordan Lee, 555-555-0100. Issue: Locked out of suite, door app not working. Called Saturday May 2 at 9:14 PM Pacific.` (138 characters)

## Risk
Anyone who can talk to the agent can trigger its enabled connector tools. With only Send Message enabled and one recipient named in the Instructions, the worst case is an unwanted email to your own team inbox. Don't enable read, forward, reply or delete tools on this connector.

## Tests
- [ ] Lockout by phone: exactly one URGENT email, correct subject, body under 300 characters, no Message email; "the team has been alerted" said only after the send
- [ ] Leak by phone: one URGENT email, no 911 advice unless someone is in danger
- [ ] Smoke: "dial 911" first; alert only if name or number was given
- [ ] Pricing message by phone: exactly one Message email
- [ ] Normal call (hours question): no email
- [ ] Recipients actually see it (phone notification, group delivery)
