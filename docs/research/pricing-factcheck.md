# AI phone answering prices: fact-check

*Checked 2026-09-29 (PT) for the "What it costs" section of the README. The claims come from a Gemini answer. Each one was checked against the company's own pricing page, read with WebFetch (Rosie's FAQ answer was read from the page's own JavaScript bundle with `curl`, because the accordion text isn't in the HTML). Prices are USD per month unless noted. "Effective" means the plan price divided by what's included, **assuming you use all of it**. At lower volume the effective cost per call or minute goes up.*

Verdicts: **Confirmed**, **Partly right** (with the correction), or **Couldn't verify**.

## Summary

| Claim | Verdict | Correction |
| --- | --- | --- |
| Per call, average $0.70–$2.00 | Partly right | Published per-call rates run $0.70–$3.00. Within a plan's included calls, the effective cost can be much lower (about $0.10–$0.83). |
| Phone2 $0.99 per call on overages | Confirmed | Both plans: $0.99 per answered call beyond the included 500 or 1,500. Calls under 15 s are free. |
| AIRA/Upfirst $0.70–$1.50 per call by tier | Confirmed | These are the **overage** rates ($1.50, $1.00, $0.75, $0.70). Both sites publish the same prices. |
| Smith.ai about $1.80–$2.00 per AI call | Partly right | In-plan rates are $1.60–$2.00 per call, and extra calls are $2.10–$3.00. $1.80–$2.00 covers only two Pro tiers. |
| Per minute, $0.20–$0.50 | Partly right | Right for published overage rates (Trillet $0.20, Dialzara $0.35–$0.48). Rosie has no per-minute rate and works out to about $0.15–$0.20. |
| Trillet $0.20/min | Confirmed | AI Receptionist plan: $49 for 150 min, then $0.20/min. |
| Rosie AI about $0.25/min | Partly right | Rosie sells minute bundles with no per-minute overage. The effective rate is about $0.15–$0.20/min. |
| Dialzara $0.35–$0.48/min | Confirmed | Overage by plan: $0.48, $0.45, $0.40, $0.35. |
| Flat monthly, $25–$300 | Partly right | Entry plans run $0 (Smith.ai Free) to $99 (Phone2). Self-serve top tiers reach $349 (Dialzara) and $500 (Smith.ai). |
| Entry plans $25–$50 for 50–150 min or calls | Partly right | $24.95–$49 is right for Upfirst/Aira, Dialzara, Trillet and Rosie, but what's included ranges from 30 calls to 250 min. |
| Mid tier $79–$297 | Partly right | Mid tiers found were $59.95–$199. No plan at $297 was found; the closest are $299 (Rosie Growth, Upfirst/Aira Scale). |
| Goodcall $59–$99/mo, unlimited minutes, capped by unique callers | Partly right | The structure is right, but the prices are $79 / $129 / $249 monthly ($66 / $108 / $208 billed annually), with $0.50 per extra unique customer. |
| Bottom line: about $0.25 for a 1-min call, up to $1–$2 for longer | Partly right | This only holds for per-minute plans. Per-call plans charge the same for a short or a long call. A 3-minute call runs from about $0.45 to $1.44 on per-minute plans. |

---

## Per-call pricing

### Phone2: Confirmed
URL: https://www.phone2.ai/pricing

What the page says:
- **AI Start**: "$99/month or $79/mo billed annually". "500 answered calls/mo included · $0.99/call beyond".
- **AI Scale**: "$199/month or $149/mo billed annually". "1,500 answered calls/mo included · $0.99/call beyond".
- Conditions: "Calls under 15 seconds (spam, wrong numbers, hang-ups) are free and don't count." "No per-minute fees, ever." 3-day free trial, no contracts. Extra numbers $4/mo. Inbound answering covers the USA and Canada. The page is aimed at personal-injury law firms.

Effective cost per call, if all included calls are used:
- AI Start monthly: $99 ÷ 500 = **$0.198**. Annual: $79 ÷ 500 = **$0.158**.
- AI Scale monthly: $199 ÷ 1,500 = **$0.133**. Annual: $149 ÷ 1,500 = **$0.099**.
- At lower volume it's higher. For example, 100 calls on AI Start is $99 ÷ 100 = $0.99 per call.

### Upfirst: Confirmed (these are overage rates)
URL: https://www.upfirst.ai/pricing

| Plan | Monthly | Annual (per month) | Included | Per additional call |
| --- | --- | --- | --- | --- |
| Starter | $24.95 | $20 | 30 calls | $1.50 |
| Premium | $59.95 | $48 | 90 calls | $1.00 |
| Pro | $159.95 | $128 | 300 calls | $0.75 |
| Scale | $299 | $240 | 600 calls | $0.70 |
| Custom | Custom | | 600+ calls | Volume pricing |

Conditions: "We also don't bill for calls under 15 seconds or calls where the caller never speaks". Spam is blocked and not counted. 14-day free trial. Each additional agent is $9.95/month.

Effective cost per included call:
- Monthly: Starter $24.95 ÷ 30 = **$0.832**. Premium $59.95 ÷ 90 = **$0.666**. Pro $159.95 ÷ 300 = **$0.533**. Scale $299 ÷ 600 = **$0.498**.
- Annual: $20 ÷ 30 = $0.667. $48 ÷ 90 = $0.533. $128 ÷ 300 = $0.427. $240 ÷ 600 = $0.400.

### Aira (getaira.io): Confirmed (same numbers as Upfirst)
URL: https://www.getaira.io/agents/receptionist/pricing

What the page says: Starter $24.95/month, 30 calls, overage $1.50/call. Premium $59.95/month, 90 calls, $1.00/call. Pro $159.95/month, 300 calls, $0.75/call. Scale $299.00/month, 600 calls, $0.70/call. "Annual Save 20%". It includes a free phone number and spam filtering.

The plan names and prices match Upfirst's exactly. I didn't check whether the two are the same company. Note that some of Aira's own FAQ pages quote different plans (for example, "Growth is $49.95/mo for 100 calls" at https://www.getaira.io/ai-receptionist-faq/how-much-does-an-ai-receptionist-cost). The pricing page above is the one used here.

The effective per-call math is the same as Upfirst's monthly figures ($0.498–$0.832).

### Smith.ai AI Receptionist: Partly right
URL: https://smith.ai/pricing/ai-receptionist

| Plan | Price shown | Included real calls/mo | Per real call | Extra real call |
| --- | --- | --- | --- | --- |
| Free | $0 | 25 | Free | $3.00 |
| Pro | $150/mo at 75 calls | 75 / 150 / 300 | $2.00 / $1.80 / $1.67 | $2.50 / $2.30 / $2.17 |
| Enterprise | $500/mo at 300 calls | 300 / 500 / 1000+ | $1.67 / $1.60 / Custom | $2.17 / $2.10 / Custom |

Conditions: calls from known spammers are filtered and don't count, and customers can remove "up to 10% of their calls" as spam each cycle. Month to month, with 30 days' notice to cancel. Calls can be transferred to live human receptionists. Pro includes onboarding and live support. Enterprise adds full-service setup and custom prompting.

Math: the page shows only the default tier prices. The others are computed from the per-call rate: Pro 150 × $1.80 = $270, Pro 300 × $1.67 ≈ $501, Enterprise 500 × $1.60 = $800. $150 ÷ 75 = $2.00 and $500 ÷ 300 = $1.67 match the page.

Correction: "$1.80–$2.00" is only two of the Pro tiers. The full in-plan range is $1.60–$2.00, and extra calls are $2.10–$3.00.

## Per-minute pricing

### Trillet: Confirmed
URL: https://www.trillet.ai/pricing

What the page says: the **AI Receptionist** is "$49/mo", with "150 minutes included, then $0.20/min". It's for solo owner-operators, inbound only, and includes a 28-day money-back guarantee. Separately, the white-label **Studio/Agency** plans (from $99/mo) charge "$0.12/min AI usage after included minutes", and "telephony is billed separately" on those plans.

Effective: $49 ÷ 150 = **$0.327/min** if you use all 150 minutes, then $0.20/min after that.

### Rosie: Partly right
URL: https://heyrosie.com/pricing

| Plan | Monthly | Included |
| --- | --- | --- |
| Professional | $49 | 250 minutes |
| Scale | $149 | 1,000 minutes |
| Growth | $299 | 2,000 minutes |

Paying annually gives "2 months free". The overage FAQ ("What happens if I go over my minutes?") says: "If you do exceed your limit, we'll automatically move you up to the next plan - at that plan's standard price, never more. No per-minute overage surprises". 7-day free trial.

Effective: $49 ÷ 250 = **$0.196/min**. $149 ÷ 1,000 = **$0.149/min**. $299 ÷ 2,000 = **$0.150/min**.

Correction: Rosie publishes no per-minute rate. The effective rate is about $0.15–$0.20/min, not $0.25.

### Dialzara: Confirmed
URL: https://dialzara.com/pricing

| Plan | Monthly | Included talk time | Overage |
| --- | --- | --- | --- |
| Business Lite | $29 | 60 min | $0.48/min |
| Business Pro | $99 | 220 min | $0.45/min |
| Business Plus | $199 | 500 min | $0.40/min |
| Business Elite | $349 | 1,000 min | $0.35/min |

Conditions (from the page's FAQ): "Only live talk time while connected to a caller. Usage tracked to the second". Extra minutes are bought through auto-recharge or à la carte. "If it hits zero, callers go straight to voicemail." Numbers cost extra: "US and Canada local numbers are $3/month. US toll-free numbers are $5/month." 7-day free trial.

Effective: $29 ÷ 60 = **$0.483/min**. $99 ÷ 220 = **$0.450/min**. $199 ÷ 500 = **$0.398/min**. $349 ÷ 1,000 = **$0.349/min**.

## Flat monthly pricing

### Entry plans $25–$50 (50–150 min or calls): Partly right
Entry plans found: Upfirst/Aira $24.95 (30 calls), Dialzara $29 (60 min), Trillet $49 (150 min), Rosie $49 (250 min). Outside that range: Smith.ai Free $0 (25 calls), Goodcall $79 (100 unique customers), Phone2 $99 (500 calls). The price band holds for four vendors, but the included amounts run from 30 calls to 250 minutes.

### Mid tier $79–$297: Partly right
Mid tiers found: Upfirst/Aira Premium $59.95 and Pro $159.95, Goodcall Growth $129, Dialzara Pro $99 and Plus $199, Rosie Scale $149, Smith.ai Pro $150, Phone2 AI Scale $199. **No $297 plan was found** on any page checked. The nearest are $299 (Rosie Growth, Upfirst/Aira Scale).

### Goodcall: Partly right
URL: https://www.goodcall.com/pricing

| Plan | Monthly | Annual (per month, "15% OFF") | Unique customers/mo | Over the cap |
| --- | --- | --- | --- | --- |
| Starter | $79 per agent | $66 | 100 | "$0.50/customer after 100" |
| Growth | $129 per agent | $108 | 250 | "$0.50/customer after 250" |
| Scale | $249 | $208 | 500 | not stated on the page |

What the page says: "unlimited minutes and tokens". "We DO NOT charge any fees for number of calls call minutes, or tokens". A "unique customer" is a caller with a unique phone number who actually speaks to the agent in a given month. Robocalls, blocked numbers and silent callers don't count.

Effective per unique customer: $79 ÷ 100 = **$0.79**. $129 ÷ 250 = **$0.516**. $249 ÷ 500 = **$0.498**. The cost per call is lower when the same people call more than once.

Correction: the structure is right, but the prices are $79–$249 (or $66–$208 billed annually), not $59–$99.

## Bottom line claim: Partly right
"About $0.25 for a one-minute call, up to $1–$2 for longer ones."
- **Per-minute plans**: a 1-minute call is about $0.15–$0.48 (Rosie effective $0.15–$0.20, Trillet $0.20 overage or $0.33 in plan, Dialzara $0.35–$0.48). A 3-minute call is 3 × $0.15 = $0.45 to 3 × $0.48 = $1.44.
- **Per-call plans** charge the same for any length: about $0.10–$0.83 in plan if you use the whole allowance, and $0.70–$3.00 for overage or list rates (Phone2 $0.99, Upfirst/Aira $0.70–$1.50, Smith.ai $1.60–$3.00).
- **Per-caller plans** (Goodcall) are about $0.50–$0.79 per unique caller per month.

So $0.25 is reasonable for a short call on a per-minute plan, but "longer calls cost $1–$2" only applies to per-minute billing. Per-call vendors charge the same for short and long calls.

---

## xAI (Grok Voice Agent / Voice API)

**Published rate: $0.08 per minute of audio, plus $0.01 per minute for telephony on the free provisioned number. That's $0.09/min for a phone call on the free number.**

- https://docs.x.ai/developers/pricing, under "Voice Pricing": "Speech to Speech (grok-voice-think-fast-2.0) | $0.08 / min ($ 4.80 / hr)". The table also has a row that reads "$0.004 / text input". It isn't explained, and I didn't check whether it applies to Builder phone calls. Speech to Text is $0.10/hr (REST) or $0.20/hr (streaming). Text to Speech is $15.00 per 1M characters. The same table appears on https://docs.x.ai/docs/models.
- https://x.ai/news/grok-voice-agent-builder, under "What it costs": "Agents are billed at our API rate (currently $0.08 / min of audio), with voices included and no separate platform fee. Telephony on a free provisioned number is an additional $0.01 / min." Also: "Each account includes a free phone number", and existing numbers can connect over direct SIP (that provider's own charges apply).
- https://x.ai/api/voice: "Speech to Speech: Real-time voice conversations over WebSocket $0.08 / min".
- Watch out for older figures. Third-party write-ups and older cached copies of the pricing page quote **$0.05/min**. In the cached copy, that rate belonged to `grok-voice-think-fast-1.0`, marked "Deprecated". The live pages checked today say $0.08/min.
- Other possible charges I didn't verify for Builder agents: `collections_search` is listed at $2.50 per 1,000 calls and collection storage at $0.10/GiB/day for API use. The pages don't say whether these apply to knowledge-base lookups in the Builder.

**Expected cost of a typical call** (audio plus free-number telephony only):
- 2 minutes: 2 × ($0.08 + $0.01) = **$0.18**
- 3 minutes: 3 × ($0.08 + $0.01) = **$0.27**

## Our own early number

Our xAI console dashboard (the first deployment's account) showed **$0.37 of credit used over 30 days, with 755 tokens and 520 requests**. That covers 5 real phone calls (all short, a minute or two) plus several "Try it live" browser test sessions.

- $0.37 ÷ 5 calls = **$0.074 per call**. That includes the test overhead, so the honest summary is **"under 10 cents a call so far, in early testing."**
- **Not reconciled:** at the published $0.09/min, five calls of 1–2 minutes would come to $0.45–$0.90 before any browser tests, which is more than $0.37. Possible reasons, none checked: some calls were shorter than a minute, audio is billed per second, some usage hadn't posted to the dashboard yet, or the dashboard counts usage differently. Until we've reconciled it, **budget from the published rate** ($0.18–$0.27 for a 2–3 minute call), not from our early figure.
