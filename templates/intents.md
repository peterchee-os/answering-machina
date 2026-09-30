# Caller intents: <Business> receptionist

The agent never transfers. It takes messages for follow-up "the next business day" after hours, or "as soon as someone is free" if it also answers daytime no-answer calls. During open hours it never says the business is closed.
A message = name (spelled back), callback number (read back), reason, urgency, then a spoken recap.

| Intent | Example phrases | Source | Action | Hours | Urgency |
|---|---|---|---|---|---|
| Hours | "Are you open now?", "Open Saturday?" | Key facts | After hours: the front desk is closed now. Open hours: yes, the team is just busy. Give the hours; offer a message | Any | Normal |
| Directions and parking | "What's your address?", "Where do I park?" | Key facts, kb/directions-parking.md | Answer; suggest a map app for turn-by-turn | Any | Normal |
| Holiday | "Open the day after Thanksgiving?" | Key facts | Give the dated closure, or say there's no holiday schedule and take a message | Any | Normal |
| Pricing, availability, tours | "How much is…?", "Can I book a tour?" | none | Never quote; take a message | Any | Normal |
| Ask for a person | "Is the manager there?" | none | No one is available right now; don't confirm who's there; take a message | Any | Normal |
| Urgent | "I'm locked out", "Water is coming through the ceiling", "It's an emergency" | none | Urgent message + one URGENT email; "alerted" only after the send succeeds; no response-time promise | Any | Urgent |
| Life-threatening | "There's smoke!", "Someone collapsed" | none | First reply: hang up and dial 911 now; alert only if name or number already known | Any | Urgent |
| Wrong number or spam | "Is this the pizza place?", robocall | none | Say who you are; one-line note at most; no email; end call | Any | Normal |
| Other / not in KB | anything else | none | Take a message; log the topic as a KB gap | Any | Normal |

## Separate daytime agent (pattern 2 in business-hours-mode, with transfers)
| Intent | Action |
|---|---|
| Existing client, urgent issue | Transfer to <on-site target> during <hours>, once; else urgent message + alert |
| Billing question | Transfer to <billing target> during <hours>, once; else message |
| Ask for a named person who isn't a target | Take a message; confirm nothing |
