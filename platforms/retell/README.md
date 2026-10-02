# Platform guide: Retell AI

How to build the Answering Machina receptionist on [Retell AI](https://www.retellai.com/) instead of xAI's Grok Voice Agent Builder.

> **Status: isolated browser tests verified on 2026-10-01.** Manual text tests, a basic web voice conversation and one Retell → Zapier → Gmail delivery reached the owner’s inbox. No live telephone routing or full failure suite was tested. See the [sanitized report](../../operators/chatgpt-dot/TEST-REPORT-2026-10-01.md) and [tested walkthrough](../../operators/chatgpt-dot/TESTED-WALKTHROUGH.md). Remaining unconfirmed steps stay marked **TODO: verify**.

> **Which platform?** If you're choosing fresh, the [xAI build with Grok Bot](../xai/README.md) is simpler and is the one we recommend: xAI's voice agent has a built-in Gmail connector for in-call alerts, and the Grok Bot template sets it all up. Retell is a good fit if you already use ChatGPT or Claude, or want Retell's features (such as warm transfers). Expect more manual setup and a third-party automation account (Zapier or n8n) for in-call email alerts.

The design doesn't change: Key facts in the Instructions, one Email policy, honest "alerted only after a successful send" wording, after hours first, and the full test script on a test number before any real line. Read [the core](../../core/README.md) first. This page covers only what's different on Retell.

## Contents
- [Before you start](#before-you-start)
- [1. Account](#1-account)
- [2. Create the agent](#2-create-the-agent)
- [3. Instructions](#3-instructions)
- [4. Welcome message](#4-welcome-message)
- [5. Knowledge base](#5-knowledge-base)
- [6. Functions](#6-functions)
- [7. Email alerts during a call](#7-email-alerts-during-a-call)
- [8. After the call: history, extraction, webhooks, alert rules](#8-after-the-call-history-extraction-webhooks-alert-rules)
- [9. Security and data settings](#9-security-and-data-settings)
- [10. Test number and phone tests](#10-test-number-and-phone-tests)
- [11. Routing your existing number (later)](#11-routing-your-existing-number-later)
- [12. Transfers (daytime agent only)](#12-transfers-daytime-agent-only)
- [Cost](#cost)
- [Managing Retell from an AI assistant](#managing-retell-from-an-ai-assistant)
- [Build checklist](#build-checklist)
- [Limits](#limits)
- [Everything marked TODO: verify](#everything-marked-todo-verify)

## Before you start
- **Time:** an afternoon for the build and the test calls, plus setting up a Zapier or n8n account for email alerts (section 7).
- **Money:** new Retell accounts start with **$10 of free trial credit**, no card needed. Buying a phone number needs a card on file, and the number costs **$2/month** ([quick start](https://docs.retellai.com/get-started/quick-start), [testing pricing](https://docs.retellai.com/test/testing-pricing)).
- **Identity check:** Retell may ask you to complete KYC (identity) verification. Its KYC page says verification unlocks outbound calling, phone number purchases and SMS ([KYC](https://docs.retellai.com/accounts/kyc)). The receptionist doesn't need outbound calls.
- **Plain truth about email:** Retell has **no built-in "send an email" tool** for agents. To get the kit's URGENT and Message emails during a call, you attach a third-party automation service (Zapier or n8n) as an MCP server, which needs its own account (section 7). This is the main extra step compared with the xAI build, whose voice agent has a built-in Gmail connector. Without one, the receptionist can still answer questions and take messages that land in Call History, but it can't alert anyone during the call, and it must not say it has.

## 1. Account
1. Sign up at the [Retell dashboard](https://dashboard.retellai.com). You sign in yourself; never give an assistant your password.
2. New accounts use prepaid credits: usage comes off the balance, and **when the balance hits zero, calls stop** ([billing](https://docs.retellai.com/accounts/billing)). Before going live, turn on **Auto recharge** on the Billing tab (trigger $10 to $1,000, refill $20 to $2,000, as of September 2026) or check the balance daily. Credits don't expire and aren't refundable.
3. Optional: **Budget Setting** on the Billing page caps monthly spend and emails you at 80% and 100%. At 100%, calls pause, so set it well above what you expect.

## 2. Create the agent
From the [quick start](https://docs.retellai.com/get-started/quick-start):
1. **Agents** tab, **Create an agent**.
2. Choose **Single prompt**. (Retell recommends starting there; a conversation flow is for tighter control. See [choose an agent type](https://docs.retellai.com/build/choose-agent-type).)
3. Choose **Build from scratch**. Retell also offers a ready-made **Receptionist** template and **Generate from prompt**; if you use either, replace everything it pre-fills with your own files, as in the xAI guide.
4. Name it "<Business> Receptionist".
5. **Model:** open the model dropdown in the agent toolbar and pick one marked **Suggested**. The per-minute price shows next to each model ([basic settings](https://docs.retellai.com/build/single-multi-prompt/configure-basic-settings)).
6. **Voice:** pick one in **Select Voice**. Retell's platform voices cost $0.015/min; ElevenLabs voices cost $0.040/min ([pricing](https://www.retellai.com/pricing)).

Retell keeps **versions**: a new version is a draft you can edit and test; **Publish** (upper right of the agent page) makes it read-only. Both drafts and published versions can be attached to a phone number, and the `prod` and `staging` **environment tags** let you switch a number between versions, which is your rollback ([versions](https://docs.retellai.com/agent/version)).

## 3. Instructions
Paste the Instructions into the large prompt box in the middle of the agent page. Start from [`templates/prompt.md`](../../templates/prompt.md) (or your filled-in `prompt.md`) and make the changes in [prompt-changes.md](prompt-changes.md). In short:
- **Time of day:** the agent has no xAI-style time zone chip. Retell fills in time variables such as `{{current_time_America/Chicago}}` (current time in that zone) and `{{current_calendar_America/Chicago}}` (a 14-day calendar) ([dynamic variables](https://docs.retellai.com/build/dynamic-variables)). Use your own IANA zone name.
- **Caller's number:** `{{user_number}}` is the caller's number on inbound calls ([dynamic variables](https://docs.retellai.com/build/dynamic-variables)). On a forwarded call it may be your own business number (**TODO: verify** with a forwarded test call), so always read the number back.
- **Email tool:** replace the Gmail Send Message wording with your alert function (section 7).
- **Guardrails:** keep all of them in the Instructions. Retell has no list of named rules like xAI's 10 console guardrails (see section 9).
- **Length:** keep it short. Retell scales the billed minutes up when the prompt context (prompt, tool descriptions, transcript so far, tool results and retrieved knowledge-base text) passes **4,000 tokens**: billed duration = duration × tokens ÷ 4,000 ([billing exceptions](https://docs.retellai.com/accounts/billing-exceptions)). The kit's template is about 1,500 words, so a long call can cross that line (**TODO: verify** the real token count on a test call).

After pasting, save, reload the page, and check that the first and last lines match your file, as on xAI.

## 4. Welcome message
The **Welcome Message** setting below the prompt ([basic settings](https://docs.retellai.com/build/single-multi-prompt/configure-basic-settings)):
- Choose **AI speaks first**, then **Custom message**, and paste `welcome.txt` word for word. That keeps the recording sentence exact.
- Don't use **Dynamic message**: the model would write its own opener each call, so the approved recording sentence could change. Retell also bills calls under 10 seconds that use a dynamic opener as 10 seconds ([billing exceptions](https://docs.retellai.com/accounts/billing-exceptions)).
- If the agent starts talking before the caller has the phone to their ear, raise **Pause Before Speaking**.
- If the agent will also answer daytime no-answer calls, the greeting must not say "closed" (see [`skills/business-hours-mode/SKILL.md`](../../skills/business-hours-mode/SKILL.md)).

## 5. Knowledge base
From [knowledge base](https://docs.retellai.com/build/knowledge-base):
1. **Knowledge Base** tab, **Add**. Name it "<slug>-kb" and add your `kb/*.md` files as **File** sources (Retell recommends Markdown with clear `##` headings). Limits: 25 files per knowledge base, 50 MB each.
2. In the agent editor, expand **Knowledge base & memory**, select **Add**, and choose the knowledge base.
3. Leave **Chunks to retrieve** at 3 and **Similarity Threshold** at 0.6 (Retell's defaults and recommendation) unless call reviews show misses.

How it differs from xAI: Retell retrieves knowledge-base text **automatically before every reply**, with no search tool to call. Keep the **Key facts** block in the Instructions anyway. Retrieval can still miss, and the kit's rule is that the basics never depend on search. Call History shows a **Knowledge Base Retrieval** link on each reply that used it, which helps the daily review.

Cost: the first 10 knowledge bases are free, then $8/month each; calls with a knowledge base attached cost $0.005/min more.

Don't turn on **contact memory** (Knowledge base & memory) for the receptionist unless you've decided you want it to remember callers between calls ([contact memory](https://docs.retellai.com/features/contact-memory)). It isn't part of this kit's design.

## 6. Functions
In the agent's **Functions** section, **+ Add** ([function calling](https://docs.retellai.com/build/single-multi-prompt/function-calling)). Retell's prebuilt functions are End Call, Transfer Call, Press Digits and Send SMS.
- **End Call:** add it. By default the agent won't hang up on its own ([end call](https://docs.retellai.com/build/single-multi-prompt/end-call)). The kit's Wrap-up section tells it when to end the call. The prompt refers to it as `end_call` (verified as `end_call` in the 2026-10-01 test; match the prompt to the displayed name).
- **Transfer Call:** don't add it to the after-hours agent. See section 12.
- **Press Digits, Send SMS:** not used.
- **Email alerts** aren't a function here; they come from an MCP server you attach in the **MCPs** section (section 7).

## 7. Email alerts during a call
Retell's prebuilt functions don't include email, and its native integrations cover CRMs, help desks, calendars and knowledge sources (HubSpot, Salesforce, Dynamics 365, GoHighLevel, Zoho, Zendesk, Google Drive, OneDrive, Notion, Calendly, Cal.com), not Gmail or Outlook ([integrations](https://docs.retellai.com/integrations/overview)). Retell's **alert rules** can email you, but only when an aggregate metric crosses a threshold, not for a single call ([alert rules](https://docs.retellai.com/features/alerting-overview)). So there is **no built-in way for the agent to send the kit's URGENT or Message email**.

**This project doesn't build or maintain any code or endpoint for this.** Use an existing service instead: a hosted automation tool that exposes a Gmail "send email" action as an MCP server, which you attach to the agent. The two options below are **third-party services**, not maintained by this project. Each needs its own account and may cost money, and **Zapier passed one synthetic delivery test on 2026-10-01; n8n remains untested**.

### How Retell attaches an MCP server (both options)
A single-prompt agent can call tools on a remote MCP server during a call ([MCP tools](https://docs.retellai.com/build/single-multi-prompt/mcp)):
1. In the agent editor, open the **MCPs** section and select **Add MCP**.
2. Enter a name, the server URL, and the authentication under **Headers** (or **Query Parameters**). Save.
3. Select **Add Tools** and pick **only** the send-email tool.

Limits from Retell's page: the server must be publicly reachable over Streamable HTTP; Retell **can't run an OAuth sign-in**, so the server must accept a fixed header or query parameter; failed tool calls aren't retried; tool results are capped at 15,000 characters. Retell's own MCP guide names Zapier and n8n as services you can connect this way ([Retell blog](https://www.retellai.com/blog/connect-any-ai-voice-agent-to-mcp-with-retell-ai-mcp-node)).

### Option 1: Zapier MCP (hosted)
Zapier runs the MCP server for you; there's nothing to host.
- **Authentication fits Retell.** Zapier MCP works with any client that uses Streamable HTTP. For clients that can't complete an OAuth flow, Zapier issues a **connection token**, sent either as an `Authorization` header (the word Bearer, a space, then the token; Zapier's recommended way) or as a query parameter in the server URL ([Zapier MCP authentication](https://docs.zapier.com/mcp/get-started/authentication), [connect](https://docs.zapier.com/mcp/get-started/connect)). This header method was verified through Retell Manual Chat, Zapier History and inbox receipt on 2026-10-01. Telephone delivery remains to be tested. See the [exact field instructions and loading-state diagnosis](../../operators/chatgpt-dot/TESTED-WALKTHROUGH.md#3-connect-zapier-to-retell-exact-fields).
- **Set it up** (from [Zapier's help article](https://help.zapier.com/hc/en-us/articles/36265392843917-Use-Zapier-MCP-with-your-client) and [guide](https://zapier.com/blog/zapier-mcp-guide/)):
  1. At mcp.zapier.com, create a **new MCP server** and choose **Other** as the client. Make a server just for the receptionist and retain **Managed mode**. Don't reuse one you connected to ChatGPT or Claude, because every tool on it becomes reachable through callers' conversations.
  2. **Add tool:** Gmail, **Send Email** ([Zapier's Gmail MCP actions](https://zapier.com/mcp/gmail)). Connect the sending Gmail account yourself. As on xAI, a dedicated sending mailbox is safest.
  3. Open **Advanced → Show all options**: set **To → Set a specific value** to the approved inbox; set **Cc**, **Bcc**, **Attachments**, sender overrides and **Reply To** to **Do not include a value**. Leave only **Subject** and **Body** generated by AI. Set group delivery to **False** and body type to **plain**. These controls were verified on 2026-10-01; see the [field table](../../operators/chatgpt-dot/TESTED-WALKTHROUGH.md#2-prepare-one-restricted-email-action). A send-only MCP action does not prove the Gmail OAuth grant is send-only; review the actual scopes separately.
  4. On the server's **Connect** tab, select **Generate token** and copy it. Zapier shows it only once.
  5. In Retell's **Add MCP**: URL `https://mcp.zapier.com/api/v1/connect`, header `Authorization` with the word Bearer, a space and the token. Type the token in yourself; never paste it into a chat with an assistant.
- **Cost:** Zapier MCP has no separate fee; each **successful** tool call uses **2 tasks** from your Zapier plan's allowance, and failed calls use none. Check the current plan’s task limits and pay-per-task/overage settings before testing; do not assume reaching an allowance always prevents charges ([Zapier MCP usage](https://docs.zapier.com/mcp/features/usage)). The agent then reports a failed send honestly, but watch your task balance as you would Retell credits. Plan prices: [Zapier pricing](https://zapier.com/pricing).

### Option 2: n8n
n8n is a workflow tool you can self-host or use as a hosted service.
- **Authentication fits Retell.** n8n's **MCP Server Trigger** node turns a workflow into an MCP server. It supports Streamable HTTP and can require bearer-token or header authentication ([MCP Server Trigger](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.mcptrigger/)). Use the **production** URL, which n8n registers when you publish the workflow. **TODO: verify** with a Retell test call.
- **Fix the recipient.** Each parameter of an n8n tool can be filled in by the model or set by you ([use AI for parameters](https://docs.n8n.io/advanced-ai/examples/using-the-fromai-function/)). Set the Gmail tool's **To** to your team inbox yourself, and let the model fill only the subject and message.
- **Starting point:** the community template [AI-powered Gmail MCP server](https://n8n.io/workflows/3623-ai-powered-gmail-mcp-server/). Its description says it targets self-hosted n8n 1.88.0 or later, needs Gmail OAuth2 credentials (a Google Cloud project with the Gmail API enabled), and includes send, reply, get-email and send-and-wait tools. For this kit, **keep only the send tool** and delete the others, so nothing a caller says can lead to reading or replying to mail. It's a community template, not maintained by n8n, Retell or this project. **TODO: verify** that it works with Retell's MCP client as described.
- **Retell's n8n integration is for the other direction.** [Retell's official n8n integration](https://www.retellai.com/integrations/n8n) is a node you install in n8n (**Settings → Community Nodes**) so n8n workflows can start Retell calls and fetch call results, for example after `call_analyzed`. That's useful after the call (Option 3), not for sending the in-call alert.
- **Cost:** your n8n hosting or plan; this guide doesn't quote it.

### Settings and checks for either option
- **One send per call** is a prompt rule, as on xAI. These services don't stop a second send for the same call.
- **"Alerted" only after success.** The agent says "The team has been alerted" only when the tool returns success. Whether a Gmail failure inside Zapier or n8n comes back to Retell as a tool error (not a success) is **TODO: verify**. Test it with F1 to F4 below.
- **Failure tests:** run F1 to F4 on a **separate test agent**, following the [distinct failure setups](test-modes.md#failure-and-isolation-tests). F1 needs a definite first failure and a working retry; F4 needs a working sender and a controlled outside inbox. A permanently invalid token tests neither. Never break a live connection.
- **Watching for failures:** Retell's alert rules include a "Custom function failures" metric. Whether failed MCP tool calls count toward it is **TODO: verify**; otherwise the daily review of Call History is your check.
- Retell also supports **custom functions** that call any HTTPS URL, but that needs an endpoint someone writes and runs. This project doesn't provide one.

### Option 3: after the call only
A post-call function on the agent's **Workflow** page, a `call_analyzed` webhook, or Retell's Zapier or n8n integrations can send a summary after the call ends (section 8). That's useful for a "Message" email or a daily digest, but it runs after the caller hangs up, so the agent can never say "the team has been alerted" on the strength of it.

### Without any of these
For the first isolated test, use [Mode A: no-delivery simulation](test-modes.md#mode-a-no-delivery-simulation). Change the entire prompt, greeting and KB consistently, including Urgent calls, Guardrails & Escalation and Wrap-up. Do not retain sending instructions, “passed along,” “saved for the team,” or follow-up promises. Preserve 911/988 and privacy/access-code guardrails. No real callers should reach this test configuration. A later controlled delivery test uses [Mode B](test-modes.md#mode-b-one-synthetic-email). We do not recommend going live without a working alert path.

## 8. After the call: history, extraction, webhooks, alert rules
Retell sends no xAI-style "Call completed" email. What it has instead:
- **Call History** (under **Data** in the dashboard): one row per call with time, cost, duration, status, sentiment and phone numbers; each call has a recording, summary, transcript with tool calls and results, knowledge-base retrievals and detail logs. You can filter and export to CSV ([call history](https://docs.retellai.com/features/session-history)). This is the daily review's main source.
- **Post call extraction** (agent editor): built-in `call_summary`, `user_sentiment`, `call_successful` and `in_voicemail`, plus custom fields you define ([post call extraction](https://docs.retellai.com/features/post-call-analysis-overview)). Suggested fields for this kit: `urgent` (boolean), `message_taken` (boolean), `caller_name` (text), `callback_number` (text), `reason` (text), and `team_email` (selector: urgent_sent, message_sent, failed, none). They make the review's email check faster. They're the model's reading of the transcript, so the transcript's tool results stay the source of truth.
- **Webhooks:** `call_started`, `call_ended` and `call_analyzed` (plus transcript and transfer events) POSTed to a URL you choose (for example a Zapier or n8n webhook), with an `x-retell-signature` header, retried up to 3 times ([webhooks](https://docs.retellai.com/features/webhook-overview)). Optional.
- **Workflow post-call functions** (agent editor, **Workflow**): run a custom function, code or SMS after the call, with access to `{{call_summary}}` and your extraction fields, and an "only when" gate ([agent workflow](https://docs.retellai.com/agent/agent-workflow)). Optional; see Option C above.
- **Alert rules** (**Alerting** tab, **Create Alert**): email or webhook when an aggregate metric crosses a threshold, up to 10 rules per workspace ([alert rules](https://docs.retellai.com/features/alerting-overview)). Two that fit this kit:
  - **Custom function failures** above 0 in the last 1 hour, checked every 5 minutes, filtered to the receptionist agent. If failed MCP tool calls count toward this metric (TODO: verify, see section 7), that's an email to you soon after an alert email fails to send.
  - **Number of calls** below 1 in the last 24 hours, as a health check. It will fire on quiet days, so decide whether you want it.

## 9. Security and data settings
In the agent editor, **Security & fallback settings**:
- **Data Storage Settings** and **Retention:** by default Retell keeps call data **forever**. Pick a retention period (1, 3, 7, 30, 60, 90, 180, 365 or 730 days); expired data is deleted daily and can't be recovered ([data retention](https://docs.retellai.com/accounts/data-retention)). The xAI build kept 30 days. Choose what your team needs for the daily review and any disputes, and write it down. Storage modes are Everything, Everything except PII, or Basic Attributes Only.
- **Guardrails:** Retell's guardrails are a topic filter that replaces flagged content with a placeholder message and lets the call continue. Output topics include harassment, self_harm, violence and regulated_professional_advice; the one input topic catches jailbreak attempts. They add about 50 ms of latency ([guardrails](https://docs.retellai.com/build/guardrails)), and the pricing page lists Safety Guardrails at +$0.005/min. They don't replace the kit's rules, which stay in the Instructions. Be careful with the **self_harm** output topic: the kit tells the agent to give the 988 Lifeline, and a filter that replaces that reply would break the self-harm handling. Don't enable it unless a test call shows the 988 guidance still gets through (**TODO: verify**).
- **Secure URLs:** optional signed links for recordings that expire after 1 minute to 7 days.
- Recording consent still applies: Retell stores recordings and transcripts unless you turn storage down, so keep the recording sentence in the Welcome message ([`docs/recording-consent.md`](../../docs/recording-consent.md)).

## 10. Test number and phone tests
**Start without a number:** create an unpublished draft, use **Test → Test LLM → Manual Chat**, then an owner-present **Test Audio** web call. Both consumed trial credit in our run. These tests need no phone purchase, API key or production routing. A telephone test number is a later purchase for full acceptance testing.
1. **Phone Numbers** tab, **Buy New Number**, optionally enter an area code, and purchase ([purchase number](https://docs.retellai.com/deploy/purchase-number)). US and Canada only. $2/month for a local number, $5/month toll-free (toll-free inbound also costs $0.06/min). The fee recurs until you release the number.
2. Select the number, set its **Inbound agent** to the receptionist and choose the version to test. A draft works; you don't have to publish first ([phone call testing](https://docs.retellai.com/test/test-phone)). Leave the outbound agent unset.
3. **Browser tests first:** the **Test** button starts a web call (needs a microphone). Web tests consumed credit in our 2026-10-01 run; check the current selected rate and budget before starting. The **LLM Playground** and simulation tests are text and bill per message ([testing overview](https://docs.retellai.com/test/test-overview), [testing pricing](https://docs.retellai.com/test/testing-pricing)).
4. **Phone tests (required):** call the number from your cell and run the whole [`templates/test-script.md`](../../templates/test-script.md). On Retell, replace check **(E)** (xAI's post-call email) with: the call appears in Call History with a recording, transcript and summary. Check **(A)** depends on your section 7 option: one successful email-tool result per qualifying call, and the email in the team inbox.
5. Run F1 to F4 on a **separate test agent** using the [correct condition for each case](test-modes.md#failure-and-isolation-tests), never the live one. Release a purchased test number when done.

This never touches your existing phone line. Nobody reaches the agent unless they dial the Retell number.

## 11. Routing your existing number (later)
Only after the tests pass and the [go-live checklist](../../docs/go-live-checklist.md) is complete. Two ways to get your callers to the agent:
- **Forward to the Retell number** (simplest). Your phone system or carrier keeps the number and forwards after-hours or unanswered calls to the Retell number. Everything in [`docs/phone-systems.md`](../../docs/phone-systems.md) and [`skills/phone-forwarding/SKILL.md`](../../skills/phone-forwarding/SKILL.md) applies; just use the Retell number wherever it says "the agent's number". Skip the "Bring your own number (Direct SIP)" section at the end of that doc, which is specific to xAI. Check caller ID on a forwarded test call.
- **Bring the number to Retell over SIP.** With elastic SIP trunking (Twilio, Telnyx, Vonage and others), the provider points the trunk's origination at Retell's SIP server, `sip.retellai.com`, and you import the number into Retell ([custom telephony](https://docs.retellai.com/deploy/custom-telephony)). Retell charges no telephony fee on custom telephony; your provider's charges apply. This is more work and usually needs your provider. Porting a number into Retell-managed telephony isn't covered in the docs we read (**TODO: verify**).

Retell gives every workspace 20 concurrent calls free. If all slots are busy, an inbound call waits about 40 seconds, then goes to the number's fallback number if one is set, or ends ([receive calls](https://docs.retellai.com/deploy/inbound-call)).

## 12. Transfers (daytime agent only)
The kit never transfers after hours. If you build a separate daytime agent ([`skills/business-hours-mode/SKILL.md`](../../skills/business-hours-mode/SKILL.md), pattern 2), Retell's Transfer Call function supports ([transfer call](https://docs.retellai.com/build/single-multi-prompt/transfer-call)):
- **Cold transfer:** the agent drops off immediately. Method **SIP INVITE** (default) or **SIP REFER** (only if your provider supports it).
- **Warm transfer:** the agent stays on, can wait for a real person (an internal queue setting waits up to 30 seconds by default), can play a private **Whisper Debrief Message** only the destination hears, and can do a three-way introduction.
- **Agentic warm transfer:** a separate transfer agent calls the destination, talks to whoever answers, then bridges or cancels.
- Targets are E.164 numbers or SIP URIs, with an optional extension and ring duration. Transfers work on phone calls only, not web calls. After the caller is connected and the agent drops off, only the telephony fee continues.
- Caller ID: "Retell Agent's Number" or "User's Number"; the latter needs provider support, and the transfer fails if it isn't supported.

The kit's transfer rules still apply: owner-approved targets only, never a number that routes back to the agent, at most one attempt per call, and test that an unanswered transfer lands in the phone system's voicemail.

## Cost
From [Retell's pricing page](https://www.retellai.com/pricing), checked 2026-09-30. Retell bills per second, per connected call.

| Item | Price |
| --- | --- |
| Retell voice infrastructure | $0.055/min |
| Voice (text-to-speech) | $0.015/min for Retell platform voices and most providers; $0.040/min for ElevenLabs |
| Language model | Varies by model, from under $0.01/min to over $0.30/min; the pricing calculator's default line shows $0.04/min |
| Telephony on a Retell US number | $0.015/min (no Retell charge on custom telephony) |
| Knowledge base attached | +$0.005/min; first 10 knowledge bases free, then $8/month each |
| Phone number | $2/month local, $5/month toll-free |
| Concurrency | First 20 concurrent calls free, then $8 per call per month |
| Optional | Safety guardrails +$0.005/min, advanced denoising +$0.005/min, PII removal +$0.01/min, AI QA $0.10/min after 100 free minutes |

Retell sums voice AI at **$0.07 to $0.31 per minute** depending on model and voice, before telephony ([testing pricing](https://docs.retellai.com/test/testing-pricing)).

**Worked example** (our assumptions, not a quote): platform voice, a model at $0.04/min, a Retell US number and a knowledge base: $0.055 + $0.015 + $0.04 + $0.015 + $0.005 = **$0.13/min**. A 2-minute call is **$0.26**, a 3-minute call **$0.39**, plus $2/month for the number. The $10 trial credit covers roughly 75 minutes at that rate. Long prompts can raise this (the 4,000-token rule in section 3). Your forwarding phone plan may charge for the forwarded leg too, and Zapier (2 tasks per successful send) or n8n costs are separate.

For comparison, the xAI build's published rate is about $0.09/min on its free number (see the [README](../../README.md#what-it-costs)).

## Managing Retell from an AI assistant
You can build everything above by hand in the dashboard. An AI assistant can also do much of it:
- **ChatGPT and dots:** Retell's ChatGPT app (plugin). Retell's own announcement says it can build an agent, deploy to a live phone number, run test calls or chats and monitor calls. We haven't seen its tool list first-hand (**TODO: verify**). See [operators/chatgpt-dot](../../operators/chatgpt-dot/INSTRUCTIONS.md).
- **Claude, Codex, Cursor and other MCP clients:** Retell's hosted MCP server at `https://mcp.retellai.com`, authenticated with your Retell API key in an Authorization header. It can create, update and publish agents, create phone and web calls, import and provision numbers, manage knowledge bases, run tests and manage alert rules ([Retell MCP server](https://docs.retellai.com/get-started/mcp-server)). Retell's own advice: keep the key out of prompts and chat, use least privilege, keep "confirm before running tools" on, and gate publish and delete on explicit intent. See [operators](../../operators/README.md).
- **Least-privilege key:** under Settings, **API Keys**, **Add**, turn on **Restrict permissions**, and give each group No Access, Read or Edit ([API keys](https://docs.retellai.com/accounts/manage-api-keys)). For an assistant that builds and reviews the receptionist: Agent **Edit**, Testing **Edit**, History **Read**, Phone **Read**, Call **No Access** (so it can't place calls), Export **No Access**. With Phone on Read, you buy and assign the number yourself.

## Build checklist
Use this in place of the Console section of [`docs/go-live-checklist.md`](../../docs/go-live-checklist.md); the Design, Tests and Phone system sections still apply.
- [ ] Single prompt agent, "<Business> Receptionist", a Suggested model, a voice chosen
- [ ] Instructions pasted with the changes in [prompt-changes.md](prompt-changes.md); after reload, first and last lines match
- [ ] Welcome message: AI speaks first, Custom message, `welcome.txt` word for word
- [ ] Knowledge base: in-scope `kb/*.md` only, attached under Knowledge base & memory
- [ ] Functions: End Call; no Transfer Call after hours
- [ ] MCPs: a receptionist-only Zapier or n8n server, with only the Gmail send tool added and **To** fixed to the team inbox
- [ ] Email path tested end to end by phone: one successful send per qualifying call, to the team inbox only, and F1 to F4 passed on a test copy
- [ ] Post call extraction fields added
- [ ] Data retention chosen and written down; Retell guardrail topics decided (self_harm off unless tested)
- [ ] Alert rule for failures (if MCP failures count); auto recharge or a daily balance check, for Retell credits and Zapier tasks
- [ ] Test number bought; inbound agent set; outbound agent unset
- [ ] Whole test script passed by phone; results in `test-results.md`
- [ ] Version published (or the tested draft noted), and the `prod` tag points at it

## Limits
- **Untested by us.** This guide is from docs, not a deployment.
- **No built-in email.** In-call alerts need a third-party automation account (Zapier or n8n) attached as an MCP server (section 7).
- **No free number.** Testing needs a card and a $2/month number.
- **Prompt length affects price** past 4,000 tokens of context.
- **Data is kept forever** unless you set retention.
- **Calls stop at a zero credit balance** unless auto recharge is on.
- The phone and consent docs are US-focused. Retell-managed numbers are US and Canada only. **Nothing here is legal advice.**

## Everything marked TODO: verify
1. `{{user_number}}` on a forwarded call: the original caller or your business number?
2. The real prompt token count of the kit's Instructions on Retell, and whether calls cross the 4,000-token billing threshold.
3. **Verified on 2026-10-01:** the End Call tool was named `end_call` and executed in text and web voice tests.
4. **Verified for Manual Chat on 2026-10-01:** Zapier bearer authentication, tool discovery, send result and inbox receipt. Still verify delivery during a telephone call.
5. **Configuration verified on 2026-10-01:** fixed **To**, excluded CC/BCC and attachments. Adversarial recipient-isolation test F4 remains unverified.
6. n8n's MCP Server Trigger with Retell end to end (bearer-token or header authentication).
7. The n8n community Gmail MCP template: that it works with Retell's MCP client once trimmed to the send tool.
8. Whether a failed Gmail send inside Zapier or n8n reaches the agent as a tool error, not a success.
9. Whether failed MCP tool calls count toward Retell's "Custom function failures" alert metric.
10. Whether Retell's self_harm output guardrail would block the 988 guidance.
11. **Observed on 2026-10-01:** web-call tests consumed trial credit. Verify the current price before testing.
12. Porting an existing number into Retell-managed telephony.
13. The Retell ChatGPT app's actual tool list and permissions.

## Sources
All read on 2026-09-30 (PT): [quick start](https://docs.retellai.com/get-started/quick-start), [basic settings](https://docs.retellai.com/build/single-multi-prompt/configure-basic-settings), [agent types](https://docs.retellai.com/build/choose-agent-type), [versions](https://docs.retellai.com/agent/version), [dynamic variables](https://docs.retellai.com/build/dynamic-variables), [knowledge base](https://docs.retellai.com/build/knowledge-base), [function calling](https://docs.retellai.com/build/single-multi-prompt/function-calling), [end call](https://docs.retellai.com/build/single-multi-prompt/end-call), [transfer call](https://docs.retellai.com/build/single-multi-prompt/transfer-call), [custom function](https://docs.retellai.com/build/single-multi-prompt/custom-function), [code tool](https://docs.retellai.com/build/single-multi-prompt/code-tool), [MCP tools](https://docs.retellai.com/build/single-multi-prompt/mcp), [send SMS](https://docs.retellai.com/build/single-multi-prompt/send-sms), [integrations](https://docs.retellai.com/integrations/overview), [agent workflow](https://docs.retellai.com/agent/agent-workflow), [post call extraction](https://docs.retellai.com/features/post-call-analysis-overview), [webhooks](https://docs.retellai.com/features/webhook-overview), [alert rules](https://docs.retellai.com/features/alerting-overview), [call history](https://docs.retellai.com/features/session-history), [guardrails](https://docs.retellai.com/build/guardrails), [data retention](https://docs.retellai.com/accounts/data-retention), [purchase number](https://docs.retellai.com/deploy/purchase-number), [receive calls](https://docs.retellai.com/deploy/inbound-call), [custom telephony](https://docs.retellai.com/deploy/custom-telephony), [phone call testing](https://docs.retellai.com/test/test-phone), [testing pricing](https://docs.retellai.com/test/testing-pricing), [billing](https://docs.retellai.com/accounts/billing), [billing exceptions](https://docs.retellai.com/accounts/billing-exceptions), [KYC](https://docs.retellai.com/accounts/kyc), [API keys](https://docs.retellai.com/accounts/manage-api-keys), [Retell MCP server](https://docs.retellai.com/get-started/mcp-server), [pricing](https://www.retellai.com/pricing), [Retell MCP blog](https://www.retellai.com/blog/connect-any-ai-voice-agent-to-mcp-with-retell-ai-mcp-node), [Retell n8n integration](https://www.retellai.com/integrations/n8n). Third-party: [Zapier MCP authentication](https://docs.zapier.com/mcp/get-started/authentication), [Zapier MCP connect](https://docs.zapier.com/mcp/get-started/connect), [Zapier MCP usage](https://docs.zapier.com/mcp/features/usage), [Zapier MCP with your client](https://help.zapier.com/hc/en-us/articles/36265392843917-Use-Zapier-MCP-with-your-client), [Zapier MCP guide](https://zapier.com/blog/zapier-mcp-guide/), [Zapier Gmail MCP actions](https://zapier.com/mcp/gmail), [n8n MCP Server Trigger](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.mcptrigger/), [n8n AI parameters](https://docs.n8n.io/advanced-ai/examples/using-the-fromai-function/), [n8n Gmail MCP template](https://n8n.io/workflows/3623-ai-powered-gmail-mcp-server/).

*Not affiliated with Retell AI, Zapier or n8n. Product names belong to their owners.*
