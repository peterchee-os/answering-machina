---
name: business-hours-mode
description: >-
  Use this when the owner wants the AI to answer daytime calls that staff miss
  (overflow during open hours), after the after-hours receptionist has been
  live for a while, or when changing the daytime agent's greeting, transfers or
  routing.
---
# Business-hours mode (daytime overflow)

The staff queue always rings first. The AI answers **only when nobody picks up** during open hours. After hours stays with the existing after-hours agent.

## When to offer it
- The after-hours agent has run **live on real routing for 1 to 2 weeks** with clean call reviews: no alert failures, and the KB gaps are fixed.
- The owner wants fewer daytime calls going to voicemail. Recommend waiting if call reviews still show fixes every week.

## Design
- **A separate agent**, e.g. "<Business> Daytime Receptionist", with its own Instructions, guardrails, number and post-call notifications. Don't reuse the after-hours agent: its greeting and rules assume the office is closed.
- **Share the knowledge base**: attach the same file collection (collections can be attached to more than one agent). Copy the same **Key facts** block into its Instructions. From then on, **knowledge-refresh** updates both agents.
- **Its own number**: a free SpaceXAI number or a SIP number. Whether an account can have more than one free number, and at what cost, is unverified. Check in the console before promising.
- **Greeting** (no "we're closed"): "Thanks for calling <Business>, this is <Name>. This call may be recorded to help us serve you. Our team is with other callers right now. I can answer questions, take a message, or try to reach someone for you. How can I help?" Use the same approved recording sentence, word for word. Drop "try to reach someone" if there are no transfer targets.
- **Messages and urgent alerts**: same as after hours (spell the name back, read the number back, recap, no response-time promises; 911 first for anything life-threatening). A non-urgent message gets one `Message: <Location>` email through the same Gmail Send Message tool; an urgent call gets only one `URGENT: <Location>` email. Promise follow-up "today" only if the owner approves that wording. Otherwise say "as soon as possible". Keep the same difficult-caller guardrails, the no-deals-or-freebies rule, including the US-only 988 rule.

## Transfer rules
- Add `transfer_call` with **only the owner-approved targets**: reason, E.164 number and hours for each. Use direct lines or cells that **never route back** to the queue or the AI. Never use the main number or the queue that just rang out.
- Transfer only for the listed reasons (e.g. an existing client's urgent issue goes to the on-site manager; billing goes to the office manager), only inside that target's hours, and **at most one attempt per call**. If the caller asks for "anyone", offer to take a message rather than transferring.
- Before transferring, tell the caller who you're connecting them to and that if nobody answers, they can leave a voicemail. Transfers are cold (no announce), so the caller lands wherever the target's no-answer goes. Point each target's unanswered calls to the **phone system's voicemail**, not a personal cell voicemail (see **phone-forwarding** section 2).
- Never read out a target's number or say where a person is. Never confirm that someone works there if the caller is fishing.
- Guardrails (10 max): swap "no transfers" for "transfer only to listed targets, for listed reasons, once". Keep 911 first, privacy, no prices, no sensitive data, urgent handling, and no response-time promises. Keep the message-email and difficult-caller rules in the Instructions. Put the rest in the Instructions.

## Routing (with **phone-forwarding** rules: recon, screenshots, owner's OK per change)
- Set the staff queue's (or front-desk user's) **unanswered destination during business hours** to the daytime agent's number. Keep a ring time that gives staff a fair chance (about 20 to 30 s). The after-hours time frame keeps pointing to the after-hours agent.
- Rollback: set the unanswered destination back to its old value (usually voicemail).

## Tests (add these to test-script.md; call from a cell during open hours)
1. Staff answer: the AI never picks up.
2. Nobody answers: after the ring time, the AI answers with the daytime greeting. It must not say "closed".
3. Hours and parking answered from Key facts.
4. Transfer that answers: correct target, the caller is told first, one attempt.
5. Transfer that doesn't answer: the caller reaches the phone system's voicemail, not a personal cell voicemail.
6. Asking for a person who isn't a target: message taken, and nothing given away about them.
7. Urgent during the day: an allowed transfer or a message, plus exactly one alert email.
8. Smoke or medical: "hang up and dial 911" first.
9. Loop check: nothing the agent does leads back into the queue or the AI.
10. The same call placed after hours reaches the after-hours agent.
11. A post-call email arrives, and its subject names the daytime agent. That's how **call-review** tells the two agents apart.

## Launch
Get the owner's OK for the routing change, make it in a quiet hour, place tests 1, 2 and 10 right away, and watch the next few days of **call-review**. Save memories: daytime agent name, number, transfer map, ring time and the routing change.
