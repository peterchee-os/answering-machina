# Urgent alerts: <Business>

xAI's post-call email is metadata only (no summary, no urgency flag), so the agent sends the urgent alert itself.

## Design
| Item | Value |
|---|---|
| Sending mailbox | <sending mailbox> (dedicated; the owner controls it and signs in to the console's Gmail connector themselves) |
| Connector tools enabled | **Send Message only** (everything else off) |
| Recipient | <team inbox> (ONE address; a group is fine) |
| Subject | `URGENT: <Location>` (fixed) |
| Body | Plain text, under 300 characters, no links |
| Body format | `Urgent after-hours call. Caller: <name>, <number>. Issue: <one sentence>. Called <day date> at <time> <time zone>.` |
| When | Once per urgent call, as soon as name, number and issue are known |
| Urgent criteria | <e.g. lockout; leak or flood; break-in; power or heating failure; caller says emergency> |
| Who reads alerts | <people on the group>; how they're notified (phone push, forwarding) |

## Message emails
For every non-urgent call where a message is taken, use the same Gmail Send Message tool to send exactly one plain-text email to `<team inbox>` with subject `Message: <Location>` and a short body containing the caller, callback number, one-sentence reason and time. Do not send it for spam or robocalls or calls with no message. An urgent call gets only the urgent email, never both; send at most one email per call. Keep the provider's post-call summary email enabled separately.

## Example (fictional)
`Urgent after-hours call. Caller: Jordan Lee, 555-555-0100. Issue: Locked out of suite, door app not working. Called Saturday May 2 at 9:14 PM Pacific.` (150 characters)

## Risk
Anyone who can talk to the agent can trigger its enabled connector tools. With only Send Message enabled and one recipient named in the Instructions, the worst case is an unwanted email to your own alert address. Don't enable read, forward, reply or delete tools on this connector.

## Tests
- [ ] Lockout by phone: exactly one email, correct subject, body under 300 characters
- [ ] Leak by phone: one email, no 911 advice unless someone is in danger
- [ ] Smoke: "dial 911" first; alert only if name or number was given
- [ ] Normal call (hours question): no email
- [ ] Recipients actually see it (phone notification, group delivery)
