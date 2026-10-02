# Retell: changes to templates/prompt.md

Start from [`templates/prompt.md`](../../templates/prompt.md) (or your filled-in `prompt.md`) and make only these edits. For the real setup, with alerts, keep all the other guardrails. For a test on a separate agent, follow the [test modes](test-modes.md) in the whole prompt, greeting and knowledge base. The no-delivery mode also removes sending instructions and follow-up promises outside Email policy. Then paste the result into the Retell agent's prompt box. Background for each change is in the [Retell guide](README.md).

Replace `America/Chicago` below with your own IANA time zone name (for example `America/New_York` or `America/Denver`). It must match the zone your hours are in.

## 1. Setup notes block
The HTML comment at the top is for you, not the agent. Don't paste it. On Retell its xAI lines don't apply; use the [build checklist](README.md#build-checklist) instead.

## 2. Time of day
Replace the first line of the section:

> Check the current <time zone> time at the start of every call.

with:

> The current local time is {{current_time_America/Chicago}}. The next two weeks: {{current_calendar_America/Chicago}}. Use these to decide open hours or after hours, and to date messages.

Retell fills in both variables for you ([dynamic variables](https://docs.retellai.com/build/dynamic-variables)); a variable with a zone in its name always uses that zone. If you ever build with Retell's default `{{current_time}}` instead, note that it uses the agent's time zone, or America/Los_Angeles when none is set.

## 3. Email policy
Replace the first sentence:

> These are the only emails you ever send. Use the Gmail Send Message tool, and send only to <team inbox>.

with (use the tool's exact name as it appears under **Add Tools** in Retell's MCPs section, for example Zapier's Gmail Send Email tool):

> These are the only emails you ever send. Use the <email tool name> tool, and send only to <team inbox>.

Keep "send only to <team inbox>" even when **To** is fixed in Zapier or n8n (section 7 of [the guide](README.md#7-email-alerts-during-a-call)). The fixed field is the real control; the sentence keeps the model from trying to fill in another address. Keep the rest of the section as written: the subjects (`URGENT: <Location>`, `Message: <Location>`), bodies under 300 characters, at most one successful email per call, and the failure wording.

Delete the last sentence of the section ("The provider's post-call email is separate; you don't send it."). Retell doesn't send one.

If you have no alert path at all, don't use this template's Email policy as written: see "Without any of these" in [the guide](README.md#without-any-of-these).

## 4. Tools
Replace:

> - **Gmail Send Message**: only as described in Email policy.

with:

> - **<email tool name>**: only as described in Email policy. Only a success result counts as sent; an error, a timeout or no result counts as a failed send.

In the **Messages** line, use the "on" wording, because Retell always provides the caller's number as `{{user_number}}`:

> You can offer the number they're calling from ({{user_number}}); still read it back.

If `{{user_number}}` turns out to be your business number on forwarded calls (TODO: verify with a forwarded test call), use the "off" wording instead: "Always ask for the callback number."

Replace the **Knowledge base** line with:

> - **Knowledge base**: use Key facts first. Other details from the knowledge base appear automatically under "Related Knowledge Base Contexts". If neither has the answer, or they conflict, don't guess; take a message.

Retell retrieves knowledge-base text before every reply and adds it to the prompt under that heading ([knowledge base](https://docs.retellai.com/build/knowledge-base)), so there is no search step.

Keep the **end_call** line, matching the name of the End Call function in your agent (verified as `end_call` in our 2026-10-01 test).

## 5. Real setup and test setups
- For the real setup, with alerts, Role, Key facts, Objective, Style, Urgent calls, Guardrails & Escalation and Wrap-up stay as they are.
- For no-delivery or one-email tests, follow [test-modes.md](test-modes.md) instead of keeping sections word for word that contradict the test. Check Urgent calls, Guardrails & Escalation, Tools and Wrap-up, not just Email policy. Keep the emergency and privacy protections.
- The Welcome message is not part of the prompt. Paste `welcome.txt` into **Welcome Message → AI speaks first → Custom message**.
- Keep the prompt as short as your facts allow. Retell bills extra once the full context passes 4,000 tokens ([billing exceptions](https://docs.retellai.com/accounts/billing-exceptions)).
