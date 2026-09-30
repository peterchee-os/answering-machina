# Urgent alerts: Sunny Desk Coworking

xAI's post-call email is metadata only (no summary, no urgency flag), so the agent sends the urgent alert itself.

| Item | Value |
|---|---|
| Sending mailbox | receptionist-alerts@example.com (dedicated; the owner signed in to the Gmail connector personally) |
| Connector tools enabled | Send Message only |
| Recipient | alerts@example.com (group: owner + community manager) |
| Subject | `URGENT: Sunny Desk` |
| Body | Plain text, under 300 characters, no links |
| Body format | `Urgent after-hours call. Caller: <name>, <number>. Issue: <one sentence>. Called <day date> at <time> Pacific.` |
| When | Once per urgent call, as soon as name, number and issue are known |
| Urgent criteria | lockout; leak or flood; discovered break-in; power, heating or cooling failure; caller says emergency |
| Who reads alerts | Group members get phone push notifications for the group label |

Example: `Urgent after-hours call. Caller: Jordan Lee, 555-555-0100. Issue: Locked out of suite 210, key card not working. Called Saturday May 1 at 9:14 PM Pacific.`

## Message emails
For every non-urgent call where a message is taken, use the same Gmail Send Message tool to send exactly one plain-text email to alerts@example.com with subject `Message: Sunny Desk` and a short body containing the caller, callback number, one-sentence reason and time. Do not send it for spam or robocalls or calls with no message. An urgent call gets only the urgent email, never both; send at most one email per call. The provider's post-call summary email stays enabled separately.

Risk (explained to the owner): callers can trigger any enabled connector tool, so the connector is send-only and the Instructions name one recipient.

## Test log (fictional)
| Test | Result |
|---|---|
| Lockout by phone | 1 email, subject correct, 154 characters |
| Leak by phone | 1 email, no 911 advice |
| Smoke | "Please hang up and dial 911 now" first; no alert (no name given) |
| Hours question | no email |
