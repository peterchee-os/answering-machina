---
name: voice-agent-setup
description: >-
  Use this when creating or updating the receptionist in xAI's Grok Voice Agent
  Builder (console.x.ai): template, instructions and guardrails, welcome
  message, caller-ID toggle, file collection, tools, the send-only Gmail alert
  connector, test number, post-call notifications, and test calls.
---
# Voice agent setup (console.x.ai)

The Builder is in beta and its UI changes. This skill matches the console as seen on 2026-09-29. Anything marked (unverified) wasn't seen on screen. If a control has moved, describe what you actually see. Never claim a feature exists without seeing it.

## What the console looks like
- **Voice Agents** (left menu, Beta): the agent list, **Create agent**, and templates: Customer Support, Sales Associate, Appointment Scheduler, Personal Assistant, Lead Qualification, Start from scratch. The credit balance is at the bottom of the left menu.
- Each agent has **Try it live** and **Publish**, a Draft or **Live** badge, and five tabs:
  - **Configuration**: Instructions (with Improve with Grok); guardrail chips with **+ Add guardrail** (max 10); a time zone chip; Model (Latest); **Welcome message** with a **Caller can interrupt** toggle; **Know caller's phone number** (off by default); **Tools** (+ Add tool); **Connectors** (Gmail, Outlook, Google Calendar, Custom MCP server); **File collections** (+ Add files).
  - **Speech**: Voice, Pronunciation, Keyterms, Language (Auto-detect), Speaking speed, Follow-up after silence.
  - **Deployment**: **Phone numbers** (Add number; the screen says unused provisioned numbers are released after 30 days of no calls), **Post-call notifications** (Manage), and **Code integration**.
  - **Conversations**: kept for 30 days. You can filter by conversation ID or caller number, duration and date. Each call has a **Call** view (recording, transcript, tool calls), **Raw events** and **Evaluation** (appears a few minutes after the call). Try it live sessions are labelled "web".
  - **Insights**: Overview and Analysis (unverified what they show).

## Ground rules
- The **owner signs in** to console.x.ai and Google in my browser. I drive the browser only after they've signed in. I never ask for, paste, read or store API keys, SIP passwords or signing secrets. If a step needs a key, I write the command with `$XAI_API_KEY` and the owner runs it.
- **Screenshot before and after every change**, and log each step in `receptionist/<slug>/setup-log.md`.
- Get the owner's explicit OK for each of these: creating or deleting an agent, **Publish**, uploading the KB, connecting an account, adding a number, and changing post-call notifications. Stop before any purchase or credit top-up.

## Steps
1. **Account check** (read-only): confirm credits, and look at the agent list. If there's a stray agent (e.g. "Untitled"), ask whether to reuse it or delete it. Zero Data Retention blocks Conversations and collections (where to check it is unverified).
2. **Create from a template**: **Customer Support** for a receptionist (Appointment Scheduler if booking is the main job). Name it "<Business> Receptionist". Screenshot what the template pre-fills. In one build, it pre-added a Gmail connector that wasn't signed in. Remove anything the design doesn't use.
3. **Instructions**: paste `prompt.md` (without its header comment) over the template text.
   - **Verify the paste.** Long multi-line pastes can break or truncate. Save, reload, and check that the **first and last lines** match `prompt.md`. If they don't, paste one section at a time (or type the broken part) and check again.
   - Don't use Improve with Grok unless the owner asks. If they do, diff the result against `prompt.md`.
   - **Guardrails**: add the 10 from `guardrails.md`, each with its **Name** and **Description**. The console caps them at 10, and the extras already live in the Instructions.
   - Time zone: the business's zone. Model: Latest.
   - **Welcome message**: on, text from `welcome.txt` verbatim. Leave Caller can interrupt on unless the owner wants the recording notice to always play in full.
   - **Know caller's phone number**: explain the tradeoff and let the owner choose. On: the agent sees the caller ID, so it can offer "Is the number you're calling from best?", which is faster and catches mistyped numbers. But the caller ID can be wrong (forwarded calls may show the business's own number, and blocked or spoofed numbers), and the number becomes part of what the model sees. Off: the agent must ask for and read back every number. That's slower but never assumes anything. Either way, the prompt must say what to do.
4. **Speech**: pick a voice. Add a **Pronunciation** for the business name and **Keyterms** for names callers say. Language: Auto-detect unless the owner wants one language. Test every language you rely on.
5. **Knowledge**: File collections, + Add files. Create "<slug>-kb" and upload only the in-scope `kb/*.md` files. Wait until each file shows Ready. Collections can be shared by more than one agent (which **business-hours-mode** uses).
6. **Tools**: `end_call` (the console may name it `end_call_2`). Don't add `transfer_call` after hours. `api_request` only if the owner has an endpoint.
7. **Alert and message-email connector** (Gmail), with the owner's OK:
   - The owner clicks Connectors, Gmail, Add, and signs in with the **dedicated sending mailbox** they control, not a personal inbox. The console warns that tool responses may be shared with anyone who talks to the agent, including callers. The Gmail Send Message tool is used for both email types: the urgent `URGENT: <Location>` alert and the non-urgent `Message: <Location>` email. Send at most one email per call; the provider's post-call summary email stays enabled separately.
   - Open the connector and enable **only Send Message**. Turn off Search, Get Message, drafts, labels, Forward, Reply All and Trash. Screenshot the "1 tool enabled" view.
   - Tell the owner the risk plainly: callers can trigger any enabled tool, so the agent can send email. That's why it's send-only and why the Instructions name one recipient. Record the sending mailbox in memory.
   - Google Calendar only if the design books appointments. Check that it can read availability, not just create events.
8. **Try it live** (browser test): it needs a **microphone**, so run it on the owner's computer if mine has none. The panel also has a text box, but typed tests weren't checked. These sessions show as "web" in Conversations, and they **never trigger post-call emails**. Run 3 or 4 script calls, fix, repeat. Then, with the owner's OK, **Publish** and confirm the Live badge. Assume only published changes are live, so publish after each approved edit.
9. **Test number**: Deployment, Add number. Choose a free SpaceXAI-provisioned number (appears to be US only; created in the console, since the API can't provision them) so the owner can call from their own cell **before any real routing changes**. The screen says unused numbers are released after 30 days without calls. Bring your own number over SIP: the provider trunks to `sip:<number>@sip.voice.x.ai;transport=tls`, and the owner registers it through the console or `POST /v2/phone-numbers` with `origin: "byo_trunk"`. Record `sip_host` and the trunk ID, never the password.
10. **Post-call notifications** (Manage): turn on **Send email when a call ends** and add up to **3 addresses** (each gets the same email) and a **minimum call duration** (10 to 1800 s; 10 works). What it sends: from `noreply@x.ai`, subject "Call completed: <agent> (<duration>)", listing caller, destination, duration, channel, time, who ended the call and a **View conversation** link. There's **no summary and no urgency flag**. It's skipped for Try it live and web sessions by design, so **test it only with a real phone call**.
11. **Phone tests**: the owner calls the test number from their cell and runs `test-script.md`. After each batch, check Conversations (recording, transcript, tool calls, then Evaluation a few minutes later), one post-call email per qualifying call, and for urgent tests exactly one alert email at the fixed address with the right subject and a body under 300 characters. If a call shows as "web", it wasn't a phone test.
12. **Record** `test-results.md` (call # | pass/fail | what happened | fix), re-test failures, and get sign-off. Save memories: agent name, template, collection, numbers, connector and sending mailbox (tools enabled), caller-ID toggle state, post-call recipients and minimum duration, last passing test date, known issues. Then go to **phone-forwarding**.

## Limits to tell the owner
- **Cost** (xAI, checked 2026-09-29): $0.08 per minute of agent audio, plus $0.01/min on a free provisioned number. Forwarded calls also use minutes on the business's phone plan. Anything else (extra numbers, knowledge-search charges inside the Builder) is unverified; check https://docs.x.ai/developers/pricing.
- The post-call email is metadata only. Details live in Conversations for 30 days, so copy anything that needs keeping longer.
- No API to export or configure an agent was seen, so configuration is by hand. That's why the log and screenshots matter.
- Transfers are cold (SIP REFER), with no whisper or announce.
- There's no business-hours schedule in the agent. The phone system decides when calls reach it.
- Concurrency limits for Builder calls aren't documented. Porting a number into xAI and xAI's SIP IP ranges aren't documented either.
