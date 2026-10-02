# Tested setup: ChatGPT Dot, Chrome, Retell and Zapier

Verified on 2026-10-01 with the owner's existing local Chrome session. This repository is a documentation and template kit, not a runnable application. No local server, package installation, phone number or production routing is needed for the first tests.

This walkthrough verifies a browser-operated path. It does **not** establish the capabilities of Retell's ChatGPT plugin, a cloud browser, Claude, or n8n. Use the browser the owner requests; an existing Chrome login is not automatically available in an assistant's cloud browser. If the assistant cannot access that browser, have the owner operate it rather than silently switch browsers.

## 1. Start with a separate draft

1. Inspect the signed-in Retell account, workspace and existing agents. Create one clearly named `TEST Sunny Desk Receptionist` draft. Leave existing agents and routing alone.
2. Use the fictional Sunny Desk example and [no-delivery test configuration](../../platforms/retell/test-modes.md#mode-a-no-delivery-simulation). Keep the prompt, fixed greeting and attached knowledge base consistent.
3. Set **AI speaks first → Custom message**. Include the owner's recording disclosure. Enable `end_call`; disable contact memory for the isolated test. Choose retention explicitly (we used seven days).
4. Reload and verify the saved prompt and greeting. Check actual model, voice, displayed price and credit. Obtain a bounded test budget before using paid tests; trial credit is still a budget.
5. Use **Test → Test LLM → Manual Chat** first. Then use **Test Audio** with the owner's microphone permission and presence. Text success does not verify audio quality, interruptions or telephone routing. Stop a web call when the owner leaves.

No email integration or Gmail permission is needed for this stage. Phone-number purchase and the complete telephone acceptance suite come later, before real callers.

## 2. Prepare one restricted email action

Get explicit authorization for the sender, recipient and one clearly labeled synthetic email. Examples below use `sender@example.com` and `owner@example.com`; replace them with the owner's approved real mailboxes. Do not send to the example addresses.

1. In the owner's signed-in Zapier account, open **MCP servers**. Create a separate server, choose **Other**, name it for the test receptionist and retain **Managed mode**. Do not enable agentic access to other apps.
2. **Add apps → Gmail → Send Email → Advanced**. Connect the approved sender. The owner completes sign-in, password resets and permission grants directly. Review the actual OAuth scopes: limiting the MCP tool to sending does not prove the underlying Gmail grant is send-only. Our run did not independently audit the final scopes the owner approved.
3. **Show all options**, then configure:

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

Save and verify that **Send Email** is the only action configured and the fixed recipient appears in its summary. Fixed fields constrain the tool independently of the prompt. Recipient-override resistance still needs its own adversarial test.

## 3. Connect Zapier to Retell: exact fields

On Zapier's **Connect** page, generate a connection token only with the owner's approval. The token is shown once. The owner saves it in a password manager and enters it directly into Retell. Never put it in chat, source files, screenshots, logs or the repository. If exposed, the owner rotates it and updates Retell.

In Retell, **MCPs → Add MCP**:

| Retell field | Enter |
|---|---|
| Name | `TEST Sunny Desk Zapier Email` |
| URL | `https://mcp.zapier.com/api/v1/connect` |
| Headers → New key value pair → left field | `Authorization` |
| Same row → right field | Type `Bearer`, type **one space**, then paste the token |
| Query Parameters | Leave empty for this header method |

The header value is **not just the token**. Do not type quotes, angle brackets, `Authorization:` or the literal word `token` into the right field. Zapier's **Copy URL** copies the base URL; **Copy token** copies the separate credential. Its alternative **Copy full URL** embeds a credential and must be treated as a secret, not shared. Use one coherent authentication method; this walkthrough uses the recommended header method.

Click **Save** (or **Update** when editing), then **Add Tools → Tool Access Scope**. A saved connection is not evidence of authentication. Verify that `gmail_send_email` appears, select only it, and save the tool. Zapier may also list configuration/schema helper tools; they were not needed or attached for this test.

### If the tool list stays on Loading

In our run, a token entered without the `Bearer ` prefix left the selector on **Loading…**, with no useful error. Discovery succeeded immediately after the owner corrected that field. This is an observed cause, not proof that every loading failure is an invalid token.

Check the public endpoint and the **Authorization** key, then have the owner privately check the value's prefix, space and current token. Do not request a screenshot containing the value. Reload the saved draft once and retry discovery. If it remains stuck, record any explicit error and investigate service availability or vendor support; do not keep rotating credentials or send a test email to diagnose authentication. Both services document Streamable HTTP support.

## 4. Send one controlled synthetic email

Switch the prompt and greeting together to [Mode B](../../platforms/retell/test-modes.md#mode-b-one-synthetic-email). Update conflicting knowledge text, or detach the no-delivery KB for this narrow test while preserving the source. In our run we detached it; the delivery prompt contained all required fictional content and guardrails.

Use a unique subject such as `TEST ONLY - Sunny Desk receptionist delivery check - DEMO-001`. The body should identify itself as a synthetic test, use a fictional caller and callback, and say no real customer response or booking is required. Trigger the tool through **Manual Chat**, not directly from Gmail; a direct Gmail send would not test the receptionist.

Verify these stages separately:

1. Retell displays exactly one `gmail_send_email` invocation with the intended subject/body.
2. The actual tool response shows a successful provider result (our Gmail result included a message ID and `SENT`). A model's spoken claim alone is insufficient.
3. Zapier **History** shows the matching execution, time and **Success**, with the same provider message ID.
4. The owner confirms the matching message arrived in the destination inbox. Only then record inbox receipt.

If a timeout or ambiguous result occurs, inspect history before any retry: the email may already have been sent. Our one-email smoke test allows no automatic retry. One-success-per-call instructions are not a server-side deduplication guarantee. Stop after the authorized send, and get a new bounded authorization for further delivery tests.

## 5. Costs and completion

Our successful send used two Zapier tasks. Manual Chat displayed $0.0208/message; voice pricing varied with configuration. These are dated observations, not guaranteed prices. Check the selected model/context price, Retell credit, Zapier allowance and overage settings before each test batch. Do not assume a task limit always blocks charges, or enable a paid plan, card, auto-recharge or phone number just to complete this walkthrough.

Save a sanitized [test report](TEST-REPORT-2026-10-01.md). Mark text tests, web voice, actual email delivery and telephone acceptance separately. Keep the agent unpublished and routing unchanged until the remaining tests and [go-live checklist](../../docs/go-live-checklist.md) pass.

Sources checked 2026-10-01: [Retell MCP tools](https://docs.retellai.com/build/single-multi-prompt/mcp), [Zapier connection methods and transport](https://docs.zapier.com/mcp/overview/how-connections-work), [Retell testing overview](https://docs.retellai.com/test/test-overview). UI labels and prices can change; report what is actually visible.
