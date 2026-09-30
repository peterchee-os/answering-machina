# Contributing to Answering Machina

Thanks for helping. This project is a set of plain-Markdown skills and templates, so most contributions are edits to text.

## Good contributions
- **Console changes**: xAI's Voice Agent Builder is in beta and changes. If a screen, limit or behaviour differs from what the skills say, open an issue or PR with the date you saw it and what's on screen. Screenshots help; remove account names, numbers and emails first.
- **Phone systems**: steps for a carrier or PBX we don't cover, with a link to the vendor's own documentation. Mark anything you couldn't confirm "verify with your provider".
- **Templates and examples**: clearer wording, better tests, new industries (e.g. clinics, salons, trades) as fictional examples.
- **Lessons from real deployments**: what broke and how you fixed it, generalized.

## Rules
1. **No real personal data.** No real business names from your deployment, people, phone numbers or email addresses. Use `555-555-01xx` numbers and `example.com` addresses.
2. **No secrets.** Never commit API keys, SIP passwords, signing secrets or tokens, even in examples.
3. **Verified or marked.** State only what you saw or can link. Otherwise write "(unverified)" or "verify with your provider". Don't invent prices or limits.
4. **Keep skills generic and short.** Skills are read by an AI agent at run time. Put site-specific detail in `examples/` and long explanations in `docs/`.
5. Skills keep valid frontmatter: `name` (matches the folder) and a `description` that starts with "Use this when …".
6. **Not legal advice.** Keep the disclaimer on anything about recording or consent.

## Process
- Fork, branch, edit, and open a PR describing what changed and how you verified it.
- Run a quick check for private data before pushing, for example: `grep -rniE "@[a-z0-9-]+\.(com|net|org)" . | grep -v example.com`.
- By contributing, you agree your contribution is licensed under the MIT License in `LICENSE`.
