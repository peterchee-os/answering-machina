# Troubleshooting

## "I tested it and no post-call email arrived"
- **Was it Try it live?** Post-call notifications are skipped for Try it live sessions by design ("Try-it sessions are always skipped" in the settings dialog). In Conversations these show as **web**. Test with a **real phone call** to the agent's number.
- Was the call shorter than the **minimum call duration**? Calls below it get no email.
- Is **Send email when a call ends** on, with the right addresses (up to 3)? Check spam for `noreply@x.ai`.
- Did you publish? Assume only published changes are live.
- Emails usually arrived within about 2 minutes in our testing. It's not guaranteed.

## "The agent didn't know our hours" (or address, or parking)
The knowledge search (`collections_search`) can miss even simple facts. In our first build it missed a basic hours question, so the agent took a message instead.
- Put a short **Key facts** block (name, address, hours, weekends, holidays, directions, parking) at the top of the Instructions, and tell the agent to answer those **without searching**.
- Keep the KB for depth: FAQs, policies, detail.
- When facts change, update **both** the Key facts and the KB (see `skills/knowledge-refresh`).

## "Part of my Instructions disappeared after pasting"
Long multi-line pastes into the Instructions box can break or truncate.
- After pasting, save, **reload the page**, and check that the **first and last lines** match your `prompt.md`.
- If they don't, paste one section at a time, or type the broken part, and check again.
- Remove the setup-notes comment before pasting. It isn't part of the prompt.

## "Try it live doesn't work"
It needs a **microphone** ("Could not access the microphone" means the browser has none or no permission). Run it on a computer with a mic. The panel also has a text box, but we didn't test typed sessions.

## "The urgent alert email didn't arrive"
- Check Conversations: did the call show a `gmail_send_message` tool call? If not, the agent didn't classify it as urgent, or the connector isn't attached or signed in.
- Is the connector's **Send Message** tool enabled?
- Is the recipient address in the Instructions exactly right? Check the group's spam and moderation settings.
- The body must stay under 300 characters, plain text.
- Did the tool call return an error? The agent should retry once, then tell the caller it couldn't reach the team. The daily call review should catch every urgent call without a matching URGENT email.
- Is the xAI account out of credit? Calls can fail silently. Turn on auto top-up or check the balance regularly.

## "The agent can't hear who's calling" / reads the wrong number
**Know caller's phone number** is off by default. When on, the agent sees the caller ID. Forwarded calls may show your own business number, so always read the number back.

## "Calls loop" or "the transfer rang the AI again"
A transfer target or forwarding rule points back at the agent's number. Use direct lines or cells that never route to the AI. After hours, don't add transfers at all.

## "Business calls went to someone's personal voicemail"
Staff cells in the ring group answered with their own voicemail. Ring cells for about 25 seconds, then send the call to the phone system's voicemail (see `docs/phone-systems.md`).

## "The free test number stopped working"
The console notes that unused provisioned numbers are released after 30 days with no calls. Call it now and then, or add a new one.
