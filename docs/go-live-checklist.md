# Go-live checklist

Tick every box before routing real calls to the agent. The owner approves each live change individually.

## Design
- [ ] Intake complete; open items acknowledged
- [ ] Key facts block in the Instructions (name, address, hours, weekends, holidays, directions, parking)
- [ ] KB upload set limited to what's in scope; nothing invented; no staff names or personal numbers
- [ ] Recording sentence approved **word for word**
- [ ] Exactly 10 console guardrails (Name + Description); extras in the Instructions
- [ ] Urgent criteria agreed; one alert recipient; fixed subject; body under 300 characters

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

## Tests (by phone, from a cell, on the free xAI test number)
- [ ] Hours, weekend, holiday, address, parking: all correct from Key facts
- [ ] Price pressure: no number spoken
- [ ] Lockout / leak / power: urgent message, **exactly one** alert email, correct subject, under 300 characters
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
- [ ] Caller ID on forwarded calls checked

## First two weeks
- [ ] Daily call review running (see `docs/call-review.md`)
- [ ] KB gaps fixed through knowledge refresh
- [ ] Only then consider business-hours mode
