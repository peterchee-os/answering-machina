# Operator: Claude (claude.ai, Claude Desktop, Claude Code)

Claude can act as the operator for a **Retell AI** build. It reaches your Retell account through Retell's hosted MCP server, then follows the same operator rules as ChatGPT: [Part B of INSTRUCTIONS.md](../chatgpt-dot/INSTRUCTIONS.md#part-b-operator-instructions-paste-this).

> **Status:** from Anthropic's and Retell's public docs, read 2026-09-30 (PT). Not yet tested end to end with this kit. Unconfirmed steps are marked **TODO: verify**.

## How it connects
- Retell's MCP server is at `https://mcp.retellai.com`. It uses Streamable HTTP and authenticates with a **Retell API key** sent in an `Authorization` header: the word Bearer, a space, then the key ([Retell MCP server](https://docs.retellai.com/get-started/mcp-server)).
- Its tools cover agents, calls, phone numbers, knowledge bases, voices, testing and alerts. Retell's own advice: use a least-privilege key, keep keys out of prompts and chat, keep tool confirmations on, read before you write, and only publish or delete on explicit intent.
- **You** create the key and type it into Claude's settings or your terminal. Never paste it into a chat with Claude or any other assistant.

## Step 1: make a restricted Retell key
In the Retell dashboard: **Settings → API Keys → Add**, name it "Claude operator", turn on **Restrict permissions**, and set each group ([API keys](https://docs.retellai.com/accounts/manage-api-keys)):

| Group | Access | Why |
|---|---|---|
| Agent (Build) | Edit | Create and edit the test agent and its knowledge base |
| Testing (Build) | Edit | Run Retell's text tests |
| History (Monitor) | Read | Read calls for test results and reviews |
| Export (Monitor) | No Access | Not needed |
| Call (Deploy) | No Access | Claude can't place phone or web calls |
| Phone (Deploy) | Read | Claude can see numbers but can't buy or reassign them. You do that in the dashboard |

TODO: verify that these groups map to the tools you need (for example, whether attaching a knowledge base or reading call details needs another group). Loosen one group at a time only if a step fails. Delete the key when you're done.

## Step 2, option 1: claude.ai or Claude Desktop (custom connector)
Custom connectors work on Claude, Cowork and Claude Desktop, on Free (one custom connector), Pro, Max, Team and Enterprise plans. The connection runs from Anthropic's cloud, not your computer ([custom connectors](https://support.claude.com/en/articles/11176164), [remote MCP](https://claude.com/docs/connectors/custom/remote-mcp)).
1. Go to **Customize > Connectors**, select **+** next to Connectors, and choose **Add custom connector**.
2. Name it "Retell" and enter `https://mcp.retellai.com` as the URL.
3. Retell uses a header, not an OAuth sign-in. Open the connector's **Request headers** section, add the `authorization` header, enter the word Bearer, a space, and your key, and mark it **Required**. Anthropic stores the value securely and doesn't show it again.
4. Select **Add**.

Anthropic notes that **Request headers is in beta and may not appear** for your account. If you don't see it, use Claude Code (option 2). On Team and Enterprise plans, an Owner adds custom connectors under **Organization settings > Connectors** first.

Retell's docs also describe adding the server to Claude Desktop's JSON config (**Settings → Developer → Edit Config**) with a URL and headers. Anthropic's docs describe that file for local servers and send remote servers through Connectors, so **TODO: verify** before relying on the JSON route.

Then make a **Project**, paste Part B into its instructions, and say "Start the interview." Turn the Retell connector on for the chat. Leave Claude's tool confirmations on, so each Retell change asks first.

## Step 2, option 2: Claude Code
From [Claude Code's MCP docs](https://code.claude.com/docs/en/mcp) and Retell's page:
1. Set an environment variable with your key in your own shell profile, for example `RETELL_API_KEY`. A name of your own works. Claude Code won't expand some reserved credential names in headers.
2. Add the server: `claude mcp add --transport http retell https://mcp.retellai.com --header "Authorization: <value>"`, where the value is the word Bearer, a space, and your key. To keep the key out of shell history and project files, put the server in `.mcp.json` instead and write the header value as the word Bearer, a space, and `${RETELL_API_KEY}`. Claude Code expands it from your environment.
3. Check it with `claude mcp list` or `/mcp` inside Claude Code. It should show as connected.
4. In a folder with the Answering Machina repository, start Claude Code, paste Part B, and say "Start the interview." Claude Code can read `templates/` and `platforms/retell/` directly.

Don't commit an `.mcp.json` with a real key in it.

## What Claude can't do here
- **Read the team inbox.** Part B uses Gmail only to check that test alerts arrived. If you have a Gmail connector in Claude, use it read-only; otherwise check the inbox yourself.
- **Buy numbers or place calls.** That's by design: with the key above it can't, and Part B forbids it anyway.

*Not affiliated with Anthropic or Retell AI.*
