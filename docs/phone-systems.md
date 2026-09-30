# Connecting your phone system

The voice agent has its own phone number: a free xAI-provisioned number, or your own number over SIP. Your phone system decides **when** calls reach that number. The agent has no business-hours schedule of its own.

Menus and codes change. Each section links the vendor page checked on 2026-09-29. Anything marked **verify with your provider** wasn't confirmed on a vendor page.

## Before you change anything
1. Test the agent on the free xAI test number from your own cell first.
2. Screenshot and write down the current setting (the rollback value).
3. Change one thing at a time, in a quiet hour, then place test calls: during hours, after hours, and the rollback.
4. Make sure nothing the agent transfers to, or tells callers to dial, routes back to the agent (no loops).
5. Check the caller ID on forwarded calls. Some systems show your own number instead of the caller's.
6. Cost: a forwarded call can use minutes on your phone plan **and** xAI minutes.

### If staff have no desk phones
If extensions ring staff cell phones, a cell's personal voicemail can answer business calls before your phone system's voicemail (or the AI) does. Ring the cell for about **25 seconds**, then send the call to the phone system's own voicemail. Let a test call ring out to confirm it lands in the business voicemail.

## Routing modes
| Mode | Where to set it | Recommended |
|---|---|---|
| After hours: time-of-day routing sends closed-hours calls to the AI | PBX or cloud phone system schedules | **Start here** |
| No-answer overflow: unanswered calls go to the AI | Queue or user "if unanswered" setting, or carrier conditional forwarding | After after hours has run for a while. Either one time-aware agent covers after hours and daytime no-answer (its prompt checks the current time and adjusts its wording), or a separate daytime agent answers these calls (see `skills/business-hours-mode`) |
| Menu option: "press 3 for our assistant" | Auto attendant option to an external number | Optional |
| Full takeover: every call goes to the AI | Number routing | Not recommended |

## Mobile carriers (conditional call forwarding)
Carrier forwarding is dialled on the phone itself. It works on **no answer / busy / unreachable** at any hour, with **no schedule**, so it's a no-answer overflow tool, not an after-hours one. Replace `NUMBER` with the agent's 10-digit number. Calls can't be forwarded to international numbers.

| Carrier | Forward if unanswered | Turn off | Source |
|---|---|---|---|
| AT&T wireless | `*61*NUMBER#` (also `*62*NUMBER#` if unreachable, `*67*NUMBER#` if busy) | `##61#` (`##62#`, `##67#`) | [Google Voice Help: carrier examples](https://support.google.com/voice/answer/165656), [AT&T wireless call forwarding](https://www.att.com/support/article/wireless/KM1011513/). AT&T's own page doesn't list the conditional codes; **verify with AT&T** |
| Verizon wireless | `*71NUMBER` (busy or no answer) | `*73` | [Verizon Call Forwarding FAQs](https://www.verizon.com/support/call-forwarding-faqs/) |
| T-Mobile | `**61*1NUMBER#` (no reply); `**62*1NUMBER#` unreachable; `**67*1NUMBER#` busy | `##61#` (`##62#`, `##67#`); `##004#` resets all conditional forwarding | [T-Mobile self-service short codes](https://www.t-mobile.com/support/plans-features/self-service-short-codes) (also lists how to set the no-reply delay, up to 30 s) |

Notes:
- AT&T **home phone** uses different codes (e.g. No Answer Call Forwarding `*92` on, `*93#` off on some services) ([AT&T calling features](https://www.att.com/support/article/home-phone/KM1000459/)). **Verify with your provider** for landlines.
- Some phone features (for example live voicemail screening on newer phones) can grab calls before conditional forwarding triggers. If forwarding seems ignored, check the phone's voicemail features. (Reported by users, not a vendor statement: **verify**.)

## Google Voice
- **Google Voice for Google Workspace** (Standard or Premier, for ring groups): Admin console, Apps, Google Workspace, Google Voice, **Ring groups**, pick the group, **Edit Working Hours**: set the time zone, **Customized hours** and **Holiday closures**, then **After hours action**, **Forward the caller**, **Phone number** (the agent's number). For no-answer overflow: **Configuration**, **Unanswered calls**, **Forward the caller** (applies after 30 seconds). Source: [Edit ring groups](https://support.google.com/a/answer/9839752). Auto attendants have a similar after-hours action ([Set up an automated attendant](https://knowledge.workspace.google.com/admin/voice/set-up-a-voice-automated-attendant-for-your-organization)).
- **Personal (free) Google Voice**: forwarding goes to linked numbers, which Google verifies with a code. An AI agent can't complete that verification. **Verify with Google** before relying on it.

## RingCentral (RingEX)
- Per user or extension: **Settings**, **Call Forwarding and Voicemail**, **After Hours**, choose **Forward to external number**, and enter the agent's number. The work-hours **Missed calls** setting can also forward to an external number (no-answer overflow). Source: [RingEX User Guide (PDF), "Setting call forwarding for after hours"](https://assets.ringcentral.com/us/guide/ringex_user_guide.pdf) (v23.1).
- Company, site and call-queue call handling have their own business-hours and after-hours rules. The developer guide confirms after-hours rules can forward to an external phone number: [Call Handling Configurations](https://developers.ringcentral.com/guide/voice/call-routing/user-call-handling/call-handling-rules).
- Admin portal menu names change between versions: **verify with RingCentral support** for company-level rules.

## NetSapiens-based hosted PBXs
Many hosted providers resell NetSapiens under their own brand. Signs: a "Manager Portal" with Users, Auto Attendants, Call Queues, Time Frames, Inventory and Call History.
- **Users, Answering Rules**: add or edit a rule for a **Time Frame** (e.g. business hours), set simultaneous ring and "ring for" seconds, and under **Call Forwarding** use **When Unanswered** or **When Offline** to an extension or external number. Rules for different time frames can be layered for time-of-day routing. Source: [Xima, NetSapiens forwarding rule](https://guide.xima.cloud/docs/netsapiens-ccaas-forwarding-rule-for-failover) (third-party guide).
- **Call Queues**: each queue has an "if unanswered" destination (no-answer overflow). Write down the old voicemail target first.
- **Time Frames**: domain-wide schedules. Holiday frames often expire, so refresh them before relying on after-hours routing.
- **Auto Attendants**: a menu option can go to an external number. In the build we saw, an attendant's time frame couldn't be changed after creation.
- **Call History, Cradle To Grave**: shows exactly how a test call was routed.
- Bringing your own number over SIP, allowing transfers (SIP REFER) back to extensions, and passing the real caller ID usually need **the provider**. Send them the details from xAI's SIP docs (below).
- Portal versions differ: **verify with your provider**.

## Any other hosted PBX: time-of-day routing
Look for: **schedule / time frame / business hours**, **holiday**, **after-hours destination**, **no-answer destination**, **forward to external number**. The pattern is always the same:
1. Make sure the business-hours schedule and holiday list are current.
2. Set the **after-hours** destination of the main number (or its queue or auto attendant) to the agent's number as an external number.
3. Keep the phone system's voicemail as the fallback elsewhere.
4. Test during hours, after hours, and on a holiday.

## Bring your own number (Direct SIP)
xAI's SIP destination for a registered number is `sip:{number}@sip.voice.x.ai;transport=tls`. Supported codecs listed: G.711 μ-law, G.711 A-law, G.722. Numbers are registered with `POST /v2/phone-numbers` and `origin: "byo_trunk"`. xAI numbers can't be provisioned through the API. Source: [xAI SIP docs](https://docs.x.ai/developers/model-capabilities/audio/speech-to-speech/sip). Never paste SIP passwords or API keys into chat or files.
