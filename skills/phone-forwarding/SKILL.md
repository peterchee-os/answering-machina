---
name: phone-forwarding
description: >-
  Use this when routing the business's calls to the AI receptionist (or undoing
  that), preparing the phone system for it, reviewing current routing, or
  drafting what to ask the phone provider for.
---
# Phone forwarding

## Hard rules
- **Never change live routing without the owner's explicit OK in chat for that specific change**, e.g. "Yes, set the front-desk queue's after-hours destination to +1…". A plan approval, memory or note doesn't count.
- **Read-only recon first.** Before each change, screenshot it and write down the exact current value. After it, screenshot again and place a test call.
- One change at a time, in a quiet hour. Write the rollback in `routing-plan.md` before touching anything.
- The owner signs in to the portal. I never handle portal passwords. If the portal shows other customers' accounts or domains, don't open them.
- Don't contact the provider. Draft the request, and the owner sends it.

## 0. Before touching real routing
- The agent passes its phone tests on the **free xAI test number**, called from the owner's cell (see **voice-agent-setup**).
- The urgent alert has been tested end to end with a real phone call.

## 1. Recon (read-only)
Record in `receptionist/<slug>/routing-snapshot.md`, with screenshots beside it: what each main number rings (queue, user, auto attendant, voicemail); ring order and timeouts; where unanswered calls go; time frames and schedules (business hours, holidays: current or expired?); menu options; the owner's role. Flag problems, e.g. expired holiday schedules (after-hours routing will then miss holidays), or unanswered calls going to an unmonitored mailbox.

## 2. Prep the phone system (if staff have no desk phones)
If extensions ring staff **cell phones**, the cell's personal voicemail can grab business calls before the phone system's voicemail or the AI gets them. For each extension: simultaneous-ring the extension plus the cell, set the ring time to about **25 s** (shorter than a typical cell voicemail pickup), then send the call to the **phone system's own voicemail**. Test by letting a call ring out. It should land in the business voicemail, not the cell's.

## 3. Pick a mode (least risky first)
| Mode | What changes | Risk | Rollback |
|---|---|---|---|
| A. After hours (**start here**) | The after-hours time frame's destination becomes the AI number | Low | Point the time frame back to its old destination |
| B. No-answer overflow (daytime) | The front-desk queue's "if unanswered" destination becomes the agent's number: the same time-aware agent, or a separate daytime agent. See **business-hours-mode** | Low; staff answer first | Set the unanswered destination back (usually voicemail) |
| C. Menu option | "Press N for our assistant" goes to the AI number as an external number | Low | Clear the option and re-record the greeting |
| D. Full takeover | The main number goes to the AI, which transfers to staff | High; not recommended | Point the number back to its old queue or user |

Write `routing-plan.md`: mode, exact screens and fields, old value, new value, test calls, rollback, and who to call if it breaks. Get the owner's OK, change it, test it, report back, and watch the next **call-review** closely.

## 4. Safety checks for any mode
- **No loops**: no `transfer_call` target, and nothing the agent tells callers to dial, may route back to the AI.
- **Caller ID**: forwarded calls may show the business's number instead of the caller's. Test it. If it's lost, the prompt must ask for the callback number (and "Know caller's phone number" is misleading).
- **Ring time before overflow**: long enough for staff to answer (about 20 to 30 s), but shorter than any cell voicemail in the ring group (section 2). Carrier no-answer forwarding uses its own delay (some carriers let you set it).
- **Cost**: a forwarded call can use two call legs (the phone plan plus xAI minutes).
- **Voicemail stays** as the fallback on the phone system.
- **Test calls after each change**: during hours (staff should get it), after hours (the AI should), a holiday if one is configured, and the rollback.

## 5. By system (check the provider's current docs; menus change)
- **Mobile carriers** (conditional forwarding, dialled on the phone itself): AT&T wireless `*61*<number>#` (no answer), off `##61#`; Verizon `*71<number>` (busy or no answer), off `*73`; T-Mobile `**61*1<number>#` (no answer), off `##61#`. These forward on no answer at any hour. There's no schedule, so they're really mode B, and they route to whichever agent you point them at.
- **Google Voice (Workspace)**: Admin console, Google Voice, Ring groups (or Auto attendants), Edit working hours, **After hours action: Forward the caller, Phone number** (mode A); Configuration, Unanswered calls, Forward the caller (mode B, after 30 s).
- **RingCentral**: user or site settings, Call Forwarding and Voicemail (Call Handling), **After Hours**, Forward to external number (mode A). The business-hours missed-calls setting can forward to an external number (mode B).
- **NetSapiens-based hosted PBX** (often white-labelled: a "Manager Portal" with Users, Auto Attendants, Call Queues, Time Frames, Inventory, Call History):
  - Call Queues: the "if unanswered" destination (mode B). Write down the old voicemail target first.
  - Users, Answering Rules: per time frame, forward When Unanswered, Busy or Offline, plus simultaneous ring and "ring for" seconds (section 2, modes A and B).
  - Time Frames: business hours and holidays. Refresh expired holiday frames before relying on mode A.
  - Auto Attendants: a menu option to an external number (mode C). An attendant's time frame can't be changed after creation (make a new one).
  - Inventory, Phone Numbers: the number's treatment (mode D).
  - Call History, Cradle To Grave: verify how each test routed.
  - Ask **the provider** (draft it for the owner): a SIP connection and dial rule to `sip:<number>@sip.voice.x.ai;transport=tls` (G.711 or G.722) for bring-your-own-number; accepting SIP REFER so transfers work; passing the real caller ID on forwards; allowing off-net forwarding.
- **Any other hosted PBX**: look for "time-of-day routing", "schedule", "holiday", "no-answer destination" and "forward to external number", then follow the same rules.

## 6. Business-hours overflow
Daytime routing (mode B) points the staff queue's unanswered destination at either **one time-aware agent** (the after-hours agent, with Instructions that check the current time and a greeting that never says "we're closed") or a **separate daytime agent**. Never point it at an agent whose greeting or Instructions assume the office is closed. The recommended rollout is after hours first, live for 1 to 2 weeks. The full design, transfer rules and tests are in **business-hours-mode**.
