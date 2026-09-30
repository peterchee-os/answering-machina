---
name: knowledge-refresh
description: >-
  Use this when the owner asks to update what the receptionist knows, when the
  business's website, prices, hours or holidays change, or when the monthly
  knowledge-refresh routine fires.
---
# Knowledge refresh

What the agent knows lives in two places, and both must stay in sync:
- **Key facts** in the Instructions (`prompt.md`): name, address, hours, weekend and holiday rules, parking. The agent answers these without searching.
- **The file collection** named in memory (`receptionist/<slug>/kb/*.md`), for depth.
If a separate daytime agent exists (**business-hours-mode**, pattern 2), it shares the collection, so a KB change reaches both agents. The Key facts are pasted into **each** agent's Instructions separately.

## Steps
1. **Gather sources**: the website and docs in memory, plus pending `call-review/proposals-*.md`. Fetch public pages with the web fetch tool, falling back to the browser. For private docs, ask the owner to share them to my computer.
2. **Extract** into the same layout (see **receptionist-design** section 1): hours, holidays for this year and next, locations and access, services, published prices with sources, policies, FAQs. Skip marketing copy and anything internal.
3. **Diff**: write the candidates to `kb-next/`, plus any Key facts change as `prompt-next.md`. Produce `kb-diff-<date>.md` with each added, changed or removed fact (old value, new value, source). Flag past or missing holidays, prices without a source, contradictions, and anything the Key facts say that the sources no longer support.
4. **Propose**: a short summary to the owner (most important first) with the file path. Change nothing yet.
5. **On explicit approval** (the owner signs in to the console):
   - Archive the old files to `kb-archive/<date>/`, then copy the approved `kb-next/` into `kb/`.
   - Replace the changed files in the collection (upload the new one, remove the old one), wait for Ready, and screenshot it.
   - If Key facts, hours, urgent rules or the team inbox changed: update `prompt.md`, paste it into each affected agent, reload, check the **first and last lines** match, and **Publish** with the owner's OK.
6. **Verify**: re-run the matching tests (hours and holidays **by phone**; the Try it live panel is fine for the rest). Record them in `test-results.md`.
7. **Record** the refresh date and changes in memory, and mark the proposals used as applied.

If nothing changed, say "KB is current as of <date>" in one line, or say nothing when a routine ran it. Never upload unapproved changes, and never invent facts.
