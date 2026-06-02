# /intel — Industry-Led Research Sweep

A Clawpilot skill that pulls and synthesizes industry pressure, competitive ecosystem moves, and internal vendor news into a meeting-ready brief. Industry-led by design: customer pressures lead, vendor news only shows up when it answers a pressure the industry actually has.

The bar is not "everything that happened this week". The bar is "what's keeping your customers' CIOs, CISOs, and CTOs up at night, and what can you walk into a meeting and credibly say about it."

## When it triggers

* `/intel`, "weekly intel", "intel sweep", "what's new this week"
* `/intel <topic>` for a focused pull (e.g. `/intel <competitor product>`, `/intel <regulation>`, `/intel <workload feature>`)
* Any scheduled automation that wraps `/intel` (e.g. a biweekly Monday brief)

## What it does

1. **Pulls industry signals first** (regulator newsrooms, comprehensive trade aggregators, ecosystem trackers). Sets the pressure landscape for the whole brief.
2. **Pulls competitor and partner ecosystem signals next**. Names which industry pressure each move is responding to.
3. **Pulls internal vendor signals last**, filters ruthlessly. Internal items only surface in the "answers a pressure" section if they materially tie to something in industry signals. Everything else goes into a demoted "captured but not pressure-tied" bucket.
4. **Synthesizes** into a structured brief: TL;DR → Primary-segment pressures + buyer questions → Secondary-segment pressures + buyer questions → Ecosystem moves → Internal-that-answers-a-pressure → Internal-captured-not-tied → Convergence → For the Shaper.
5. **Saves the brief** to a markdown file under `<your-notes-vault-path>\_meta\intel\` for the sibling /shaper skill to consume.

## What it does NOT do

* Does not pull customer-specific news (earnings, exec changes, M&A on named accounts). That's the /shaper's domain.
* Does not summarize internal vendor news for its own sake. Internal news must answer an external pressure or it gets demoted.
* Does not invent pressures to fill the sections. If a cycle is genuinely quiet, it says so and runs shorter.

## Pairs with /shaper

This skill is half of a two-skill pair. The output is consumed by the sibling [`/shaper`](../shaper/) skill, which takes a customer scenario you feed it plus the most recent /intel brief and produces a meeting-ready prep card. Lock your problem-tag set before standing up /shaper; both skills key off the same canonical tags.

## Dependencies

* Clawpilot with web fetch and (recommended) Microsoft 365 tools (`m365_search_emails`, `m365_list_emails`) for Ring 1 internal signals.
* A vendor roadmap MCP / API if available. For Microsoft skills, the Release Communications MCP (`release-communications-get_recent_azure_updates`, `release-communications-get_recent_m365_roadmaps`). For other vendors, use whatever roadmap / what's-new source exists.
* A writable notes vault (Obsidian, OneDrive folder, synced Notion export) for storing briefs. The path is configurable; if it's outside Clawpilot's filesystem MCP allowlist, the skill uses PowerShell `Set-Content` instead.
* PowerShell on the host (for the vault write fallback).

## What to customize before using

Search `SKILL.md` for `XXXX` and `<your-notes-vault-path>` and replace each instance. The big ones:

* **Your name** — referenced throughout for ownership.
* **Your role / vendor / workload / industry** — the opening paragraph names all four. Replace with your actual role, vendor (the company whose products you sell), workload (the specific product family you cover), and industry vertical (e.g. healthcare, financial services, retail, public sector).
* **Industry segments** — the brief produces two segment-specific sections (e.g. "Provider Pressures" and "Payor Pressures"). Replace with your industry's two most material sub-segments.
* **Ring 3 reliable source list** — the actual industry publications you read. Pick 4 to 6 reliable ones (avoid paywalled / bot-blocked ones).
* **Ring 3 bot-blocked source list** — over time you'll discover which sources don't fetch reliably. List them so future runs don't waste cycles.
* **Ring 2 competitor list** — your primary competitor, secondary competitors, partner ecosystem players.
* **Ring 1 internal sources** — the recap emails you read, internal newsletter senders (`XXXX@<vendor>.com`), any GBB / specialist digests.
* **Problem tag set** — two tiers (workload-pressure tags + industry-pressure tags). The example schema is illustrative; replace with the actual customer problems your workload solves for your industry.
* **Vault path** — where to save briefs. The skill is opinionated about a `_meta\intel\` subfolder; adjust if your vault structure is different.
* **Style rules** — the example bans em dashes, en dashes, and a list of marketing filler words. Customize to your own voice.

## Privacy posture

* Operates at **industry segment level only**. Never pulls or stores named-account / customer-specific news.
* Saves briefs to your personal vault. Briefs are not emailed unless an external automation explicitly wraps this skill with an email step.
* The "For the Shaper" handoff section is pressure-led, not customer-led. Customer framing happens downstream in /shaper.

## Source

This skill is part of [Claw Skills](../../README.md), a library of Clawpilot custom skills.
