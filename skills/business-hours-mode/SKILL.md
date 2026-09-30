---
name: business-hours-mode
description: >-
  Use this when the owner wants the AI to answer daytime calls that staff miss
  (no-answer overflow during open hours), usually after the after-hours
  receptionist has been live for a while, or when changing daytime wording,
  transfers or routing.
---
# Business-hours mode (daytime overflow)

The staff queue always rings first. The AI answers **only when nobody picks up** during open hours. Two patterns are supported:

| Pattern | What it is | Choose it when |
|---|---|---|
| **1. One time-aware agent** | The after-hours agent also takes daytime no-answer calls. Its Instructions check the current time and adjust the wording. `templates/prompt.md` is written this way | You want the same behaviour day and night (answer the basics, take messages, no transfers). The first real deployment of this kit uses it |
| **2. A separate daytime agent** | Its own agent, number, Instructions and guardrails; after hours stays with the after-hours agent | **Recommended when daytime behaviour should differ**, e.g. transfers to staff during the day |

## When to offer it
- **Recommended rollout**: the after-hours agent has run **live on real routing for 1 to 2 weeks** with clean call reviews: no alert failures, and the KB gaps are fixed. Recommend waiting if call reviews still show fixes every week.
- The owner wants fewer daytime calls going to voicemail. The owner can choose to start sooner (the first deployment added daytime backup on day one with pattern 1). If so, say it's earlier than recommended, run the daytime tests below first, and watch **call-review** closely.

## Pattern 1: one time-aware agent
- **Instructions**: use the **Time of day** section from `templates/prompt.md`. During open hours the agent says the team is busy helping others, answers "are you open?" with yes, and promises follow-up "as soon as someone is free". After hours it says the front desk is closed and promises follow-up "the next business day". During open hours it must **never** say the business or front desk is closed.
- **Greeting**: the Welcome message is fixed text, so it must work at any hour. No "we're closed" (see `templates/welcome.txt`).
- **Time zone**: check the agent's time zone setting. The wording depends on it.
- **No transfers**: keep the after-hours rule (no `transfer_call`). If the owner wants daytime transfers, use pattern 2.
- **Same number, same emails**: route the daytime no-answer destination to the agent's existing number. Messages and alerts follow the same Email policy.

## Pattern 2: a separate daytime agent
- **A separate agent**, e.g. "<Business> Daytime Receptionist", with its own Instructions, guardrails, number and post-call notifications. Use this pattern when daytime behaviour should differ from after hours (for example, transfers).
- **Share the knowledge base**: attach the same file collection (collections can be attached to more than one agent). Copy the same **Key facts** block into its Instructions. From then on, **knowledge-refresh** updates both agents.
- **Its own number**: a free SpaceXAI number or a SIP number. Whether an account can have more than one free number, and at what cost, is unverified. Check in the console before promising.
- **Greeting** (no "we're closed"): "Thanks for calling <Business>, this is <Name>. This call may be recorded to help us serve you. Our team is with other callers right now. I can answer questions, take a message, or try to reach someone for you. How can I help?" Use the same approved recording sentence, word for word. Drop "try to reach someone" if there are no transfer targets.
- **Messages and urgent alerts**: same as after hours (spell the name back, read the number back, recap, no response-time promises; 911 first for anything life-threatening; "alerted" only after the send succeeds). Use the same **Email policy** section: only to `<team inbox>`, one `URGENT: <Location>` email for an urgent call or one `Message: <Location>` email for a non-urgent message, never both. Follow-up wording is "as soon as someone is free" (promise "today" only if the owner approves that wording). Never say the business is closed. Keep the same difficult-caller guardrails and the no-deals-or-freebies rule, including the US-only 988 rule.

## Transfer rules (pattern 2 only)
- Add `transfer_call` with **only the owner-approved targets**: reason, E.164 number and hours for each. Use direct lines or cells that **never route back** to the queue or the AI. Never use the main number or the queue that just rang out.
- Transfer only for the listed reasons (e.g. an existing client's urgent issue goes to the on-site manager; billing goes to the office manager), only inside that target's hours, and **at most one attempt per call**. If the caller asks for "anyone", offer to take a message rather than transferring.
- Before transferring, tell the caller who you're connecting them to and that if nobody answers, they can leave a voicemail. Transfers are cold (no announce), so the caller lands wherever the target's no-answer goes. Point each target's unanswered calls to the **phone system's voicemail**, not a personal cell voicemail (see **phone-forwarding** section 2).
- Never read out a target's number or say where a person is. Never confirm that someone works there if the caller is fishing.
- Guardrails (10 max): swap "no transfers" for "transfer only to listed targets, for listed reasons, once". Keep 911 first, privacy, no prices, no sensitive data, urgent handling, and no response-time promises. Keep the message-email and difficult-caller rules in the Instructions. Put the rest in the Instructions.

## Routing (with **phone-forwarding** rules: recon, screenshots, owner's OK per change)
- Set the staff queue's (or front-desk user's) **unanswered destination during business hours** to the agent's number: the same agent's number for pattern 1, the daytime agent's number for pattern 2. Keep a ring time that gives staff a fair chance (about 20 to 30 s). The after-hours time frame keeps pointing to the after-hours agent.
- Rollback: set the unanswered destination back to its old value (usually voicemail).

## Tests (add these to test-script.md; call from a cell during open hours)
1. Staff answer: the AI never picks up.
2. Nobody answers: after the ring time, the AI answers. It must not say "closed" (ask "are you open right now?" to check), and a message gets "as soon as someone is free".
3. Hours and parking answered from Key facts.
4. (Pattern 2) Transfer that answers: correct target, the caller is told first, one attempt.
5. (Pattern 2) Transfer that doesn't answer: the caller reaches the phone system's voicemail, not a personal cell voicemail.
6. Asking for a person who isn't a target: message taken, and nothing given away about them.
7. Urgent during the day: an allowed transfer (pattern 2) or an urgent message, plus one successful URGENT email.
8. Smoke or medical: "hang up and dial 911" first.
9. Loop check: nothing the agent does leads back into the queue or the AI.
10. The same call placed after hours gets the after-hours wording (pattern 1) or reaches the after-hours agent (pattern 2).
11. A post-call email arrives. For pattern 2, its subject names the daytime agent; that's how **call-review** tells the two agents apart. For pattern 1, **call-review** uses the call time.
12. (Pattern 1) Time boundaries: calls a few minutes before and after closing (and opening), and on a listed holiday. The routing and the agent's wording should agree on each side of the boundary; a holiday the phone system treats as closed must also be in the Key facts, or the agent will use open-hours wording (T1 to T4 in `templates/test-script.md`).

## Launch
Get the owner's OK for the routing change, make it in a quiet hour, place tests 1, 2 and 10 right away, and watch the next few days of **call-review**. Save memories: the pattern, daytime agent name and number (pattern 2), transfer map, ring time and the routing change.
