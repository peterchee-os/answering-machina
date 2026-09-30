# Call recording and consent (US overview)

> **Not legal advice.** This is a short orientation for small-business owners, compiled from the public sources below on 2026-09-29. Laws change and courts interpret them. Ask a lawyer licensed where you and your callers are.

## Why this matters here
xAI's Voice Agent Builder records and transcribes every call ([xAI announcement](https://x.ai/news/grok-voice-agent-builder): "Every call is recorded and transcribed"). So your agent needs a recording notice at the start of every call, and the owner should approve the exact sentence **word for word**. The templates put it in the Welcome message, e.g. "This call may be recorded to help us serve you."

## One-party vs all-party consent
- **Federal law** allows recording when at least one party to the call consents: 18 U.S.C. § 2511(2)(d) ([Cornell LII](https://www.law.cornell.edu/uscode/text/18/2511)).
- **Most states** follow one-party consent. The Digital Media Law Project counts 38 states plus DC ([DMLP: Recording Phone Calls and Conversations](https://www.dmlp.org/legal-guide/recording-phone-calls-and-conversations)).
- **All-party ("two-party") consent** states, as listed by DMLP: **California, Connecticut, Florida, Illinois, Maryland, Massachusetts, Montana, New Hampshire, Pennsylvania, Washington**. DMLP's notes: Illinois' earlier statute was struck down in 2014 (it has since been revised), and Massachusetts bans *secret* recordings rather than requiring explicit consent.
- **Commonly flagged edge cases** (check before relying on them): **Nevada** (its statute reads one-party, but its supreme court has required all-party consent for phone calls); **Oregon** (one-party for phone calls, all-party for in-person conversations); **Michigan** and **Delaware** (conflicting readings). See the [Reporters Committee's Recording Guide](https://www.rcfp.org/reporters-recording-guide/) and [Justia's 50-state survey](https://www.justia.com/50-state-surveys/recording-phone-calls-and-conversations/) for state-by-state detail.
- **Interstate calls**: if the caller is in an all-party state, the stricter rule may apply. DMLP's advice is to "play it safe and get the consent of all parties."

## What a notice does
In some all-party states, a clear announcement counts as consent. For example, Washington's RCW 9.73.030(3) says consent is obtained when one party "has announced to all other parties … in any reasonably effective manner" that the call is about to be recorded, and the announcement itself must be recorded ([RCW 9.73.030](https://app.leg.wa.gov/RCW/default.aspx?cite=9.73.030)). Other states may need more. That's a question for your lawyer.

## Practical defaults in this project
1. The recording sentence is the first thing in the Welcome message, before any question.
2. **Caller can interrupt** is a console toggle. If your lawyer wants the notice to always play in full, turn it off.
3. If a caller objects, the agent apologizes, stops collecting details, points to the website, and ends the call (a guardrail in every template).
4. Conversations (recording, transcript) are kept in the console for 30 days. Decide who on your team may view them.
5. Outside the US, different rules apply (e.g. GDPR in the EU/UK). Get local advice.
