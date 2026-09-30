# Go-live checklist

**Required.** This checklist and the test script (`templates/test-script.md`) aren't optional. Tick every box before routing real calls to the agent. The owner approves each live change individually.

**Test on the free xAI number first.** Before you route any real line, run the whole test script by phone on the free xAI test number and fix anything that fails. Only then change your phone system.

## Design
- [ ] Intake complete; open items acknowledged
- [ ] Key facts block in the Instructions (name, address, hours, weekends, holidays, directions, parking)
- [ ] KB upload set limited to what's in scope; nothing invented; no staff names or personal numbers
- [ ] Recording sentence approved **word for word**
- [ ] Exactly 10 console guardrails (Name + Description); extras in the Instructions
- [ ] Urgent criteria agreed; one team inbox (`<team inbox>`); fixed subjects; bodies under 300 characters; approved alternative set or "none"; owner told that the one-recipient rule is prompt-level only
- [ ] One **Email policy** section in the Instructions (urgent: one URGENT email; message: one Message email; everything else: none)
- [ ] Time of day wording in place; Welcome message doesn't say "closed" if the agent will answer daytime calls

## Console
- [ ] Instructions pasted; after reload, **first and last lines match** `prompt.md`
- [ ] Welcome message verbatim; Caller can interrupt decided
- [ ] "Know caller's phone number" decided (tradeoff explained); prompt matches the choice
- [ ] File collection files all **Ready** and attached
- [ ] Tools: `end_call` only (after hours); no `transfer_call`
- [ ] Gmail connector signed in **by the owner** with the dedicated mailbox; **Send Message only**; screenshot saved
- [ ] Post-call notifications: on, up to 3 recipients, minimum duration set (e.g. 10 s)
- [ ] Published; Live badge showing
- [ ] Screenshots before/after every change in the setup log

## Tests (required; by phone, from a cell, on the free xAI test number, before any real routing)
- [ ] Every call in `templates/test-script.md` run and passed
- [ ] Hours, weekend, holiday, address, parking: all correct from Key facts
- [ ] Price pressure: no number spoken
- [ ] Lockout / leak / power: urgent message, **one successful** URGENT email, correct subject, under 300 characters, no Message email; "the team has been alerted" only after the send
- [ ] Pricing or other message: **one successful** Message email; hours-only call: no email
- [ ] Failure paths F1 to F4 run on a **separate test agent** (connector broken there, never on the live agent): honest failure wording, no "saved" or "passed along", outside addresses refused
- [ ] If one agent covers day and night: time-boundary calls T1 to T4 (just before and after closing and opening, and a listed holiday)
- [ ] Smoke / medical: "Please hang up and dial 911 now" is the first reply
- [ ] Person request: no transfer, no names
- [ ] Post-call email arrived for each qualifying call (not expected for Try it live)
- [ ] Conversations show recording, transcript, tool calls; Evaluation appears after a few minutes
- [ ] Every guardrail exercised once

## Phone system
- [ ] Read-only recon written down (current routing, timeouts, voicemail, schedules)
- [ ] Holiday schedule current
- [ ] Staff cells ring ~25 s then go to the **phone system's** voicemail (if no desk phones)
- [ ] Rollback steps written before the change
- [ ] After-hours destination changed with the owner's explicit OK
- [ ] Test calls: during hours (staff), after hours (agent), rollback works
- [ ] If the agent also takes daytime no-answer calls: a daytime call that rings out gets the agent, which never says "closed" and promises follow-up "as soon as someone is free"
- [ ] Caller ID on forwarded calls checked

## Every day (daily health check)
- [ ] Place one known test call (for example, ask the hours) and check the answer and the post-call email. At minimum, confirm in the console that the agent shows **Live** and the xAI account has credit (auto top-up on, or a low-credit check). Calls can fail silently when credit runs out.

## First two weeks
- [ ] Daily call review running (see `docs/call-review.md`), including urgent calls with no matching URGENT email
- [ ] KB gaps fixed through knowledge refresh
- [ ] Only then consider business-hours mode (recommended): one time-aware agent, or a separate daytime agent (see `skills/business-hours-mode`)
