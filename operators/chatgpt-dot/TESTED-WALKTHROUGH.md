# Tested setup: ChatGPT Dot, Chrome, Retell and Zapier

Tested on 2026-10-01 in the owner's own Chrome, already signed in. This repository is docs and templates, not an app you run. The first tests need no local server, installs, phone number or routing change.

This page tests the browser route only. It doesn't show what Retell's ChatGPT plugin, a cloud browser, Claude or n8n can do. Use the browser the owner asks for: an assistant's cloud browser doesn't share the owner's Chrome logins. If the assistant can't reach it, the owner does the steps. Don't quietly switch browsers.

## 1. Start with a separate draft

Test in the browser before you buy anything: first by text in **Manual Chat**, then with a web voice call the owner runs. Neither needs a phone number, a Retell API key or a routing change. Both used trial credit in our run.

1. Look over the signed-in Retell account, workspace and existing agents. Create one draft named `TEST Sunny Desk Receptionist`. Leave existing agents and routing alone.
2. Use the fictional Sunny Desk example and the [no-delivery test setup](../../platforms/retell/test-modes.md#mode-a-no-delivery-simulation). Keep the prompt, fixed greeting and attached knowledge base in line with each other.
3. Set **AI speaks first → Custom message** with the owner's recording notice. Turn on `end_call` and turn off contact memory. Pick how long to keep calls (we used seven days).
4. Reload and check that the prompt and greeting saved. Check the model, voice, price shown and credit left. Agree on a spending limit with the owner before any paid test; trial credit counts too.
5. Use **Test → Test LLM → Manual Chat** first. Then use **Test Audio**, with the owner there and the microphone allowed. Passing by text doesn't prove audio quality, interruptions or phone routing. Stop the web call if the owner leaves.

No email service or Gmail permission is needed yet. Buying a number and the full phone test script come later, before real callers.

## 2. Prepare one restricted email action

Get the owner's clear OK for the sender, the recipient and one clearly labeled fake (synthetic) email. The examples use `sender@example.com` and `owner@example.com`; use the owner's approved mailboxes instead. Don't send to the examples.

1. In the owner's signed-in Zapier account, open **MCP servers**. Create a separate server, choose **Other**, name it for the test receptionist and keep **Managed mode**. Don't turn on agentic access to other apps.
2. **Add apps → Gmail → Send Email → Advanced**. Connect the approved sender. The owner does the sign-in, password resets and permission grants. Check the Gmail permissions (OAuth scopes) actually granted: a send-only Zapier tool doesn't make the Gmail connection send-only. We didn't separately check the final permissions the owner approved.
3. Select **Show all options**, then set:

| Field | Setting |
|---|---|
| Subject, Body | Have AI generate a value |
| To | Set a specific value: the approved test inbox |
| Cc, Bcc | Do not include a value |
| From override, From Name, Reply To | Do not include a value; use the connected mailbox |
| Attachments, Label or mailbox, Add signature default | Do not include a value |
| Body type | Default: plain |
| Send message to Google Contacts Group/Label | Default: False |
| Render signature in HTML | Default: false |

Save. Check that **Send Email** is the only action and its summary shows the fixed recipient. Fixed fields hold whatever the prompt says. Whether a caller can talk the agent into another recipient still needs its own test.

## 3. Connect Zapier to Retell: exact fields

On Zapier's **Connect** page, generate a connection token only with the owner's OK. Zapier shows it once. The owner saves it in a password manager and enters it in Retell directly. Never put it in a chat, file, screenshot, log or this repository. If it leaks, the owner makes a new one and updates Retell.

In Retell, **MCPs → Add MCP**:

| Retell field | Enter |
|---|---|
| Name | `TEST Sunny Desk Zapier Email` |
| URL | `https://mcp.zapier.com/api/v1/connect` |
| Headers → New key value pair → left field | `Authorization` |
| Same row → right field | Type `Bearer`, type **one space**, then paste the token |
| Query Parameters | Leave empty for this header method |

The right field is **not just the token**. Don't type quotes, angle brackets, `Authorization:` or the word `token` in it. In Zapier, **Copy URL** copies the base URL and **Copy token** copies the token. **Copy full URL** puts the token in the URL, so treat it as a secret and don't share it. Pick one sign-in method; we used the header, which Zapier recommends.

Click **Save** (or **Update** when editing), then **Add Tools → Tool Access Scope**. Saving doesn't mean the sign-in worked. Check that `gmail_send_email` shows up, select only it, and save. Zapier may also list helper tools for settings or schemas; we didn't need or attach them.

### If the tool list stays on Loading

In our run, a token entered without the `Bearer ` prefix left the list stuck on **Loading…**, with no useful error. Once the owner fixed that field, the tool showed up right away. That was our cause, but a stuck list doesn't always mean a bad token.

Check the URL and the **Authorization** key. Then the owner privately checks the value: prefix, space and current token. Don't ask for a screenshot showing it. Reload the saved draft once and try again. If it's still stuck, note any error and check whether a service is down, or ask its support. Don't keep making new tokens or send a test email to check sign-in. Both services say they support Streamable HTTP.

## 4. Send one controlled synthetic email

Switch the prompt and greeting to [Mode B](../../platforms/retell/test-modes.md#mode-b-one-synthetic-email) together. Fix conflicting knowledge-base text, or detach the no-delivery knowledge base for this one test and keep its source. We detached it; the delivery prompt had all the fictional details and guardrails it needed.

Use a unique subject, such as `TEST ONLY - Sunny Desk receptionist delivery check - DEMO-001`. The body should say it's a fake test, use a made-up caller and callback number, and say no real reply or booking is needed. Trigger the send from **Manual Chat**, not from Gmail; a Gmail send wouldn't test the receptionist.

Check each step on its own:

1. Retell shows exactly one `gmail_send_email` call, with the subject and body you meant.
2. The tool's actual response shows the email provider accepted it (ours included a Gmail message ID and `SENT`). The model saying it worked isn't enough.
3. Zapier **History** shows the matching run, with the time, **Success** and the same message ID.
4. The owner confirms the message arrived in the inbox. Only then write down that it arrived.

If a send times out or the result is unclear, check the history before trying again: the email may already have gone out. This one-email test allows no automatic retry. A prompt rule of one successful send per call doesn't stop duplicates on the server. Stop after the approved send. Get a new OK, with clear limits, before more delivery tests.

## 5. Costs and completion

Our one successful send used two Zapier tasks. Manual Chat showed $0.0208 per message; the voice price changed with the settings. That's what we saw that day, not a guaranteed price. Before each batch of tests, check the model and context price, your Retell credit, and your Zapier allowance and overage settings. Don't assume a task limit always stops charges. Don't sign up for a paid plan, add a card, turn on auto-recharge or buy a number just to finish this walkthrough.

Save a [test report](TEST-REPORT-2026-10-01.md) with private details removed. Mark text tests, web voice, actual email delivery and phone tests separately. Keep the agent unpublished and routing unchanged until the remaining tests and the [go-live checklist](../../docs/go-live-checklist.md) pass.

Sources checked 2026-10-01: [Retell MCP tools](https://docs.retellai.com/build/single-multi-prompt/mcp), [Zapier connection methods and transport](https://docs.zapier.com/mcp/overview/how-connections-work), [Retell testing overview](https://docs.retellai.com/test/test-overview). Screen labels and prices can change; go by what you actually see.
