# /shaper — Customer Scenario Shaper

A Clawpilot skill that takes a customer scenario you provide, pulls fresh M365 context on the named customer, asks you for any final additional context, then reshapes the most recent /intel brief into a meeting-ready prep card (talking points, discovery questions, watch-outs, recommended next step).

This is the second half of a two-skill pair. The first half is [`/intel`](../intel/) — the researcher. /shaper is the operator.

## When it triggers

* `/shaper <scenario>` — free-form natural language describing the customer, segment, pressures, persona, and meeting context
* "shape this for <customer>", "prep card for <customer>"
* Any time you need a meeting-ready output from a free-form customer scenario plus the latest intel

## What it does

1. **Parses the scenario** for customer name, segment, pressures named, persona, meeting context.
2. **Loads the most recent /intel brief** from your vault. Warns and gates if the brief is more than 14 days old.
3. **Pulls fresh M365 context on the named customer** in parallel: recent emails (90 days), upcoming events (30 days), Teams chats.
4. **Surfaces what it found** and explicitly asks: "anything else to factor in?" Waits for your reply before synthesizing.
5. **Matches scenario pressures to /intel items** via the canonical problem-tag set (shared with /intel).
6. **Produces a structured prep card**:
   * Customer snapshot
   * Top talking points (3 to 5, ranked by customer impact)
   * Discovery questions (3 to 5, Curiosity-disciplined)
   * Watch-outs (what NOT to lead with for this profile)
   * Recommended next step (one concrete, time-boxed CTA)
   * Sources (citations from underlying /intel items)
7. **Saves the prep card** to `<your-notes-vault-path>\_meta\shaper\YYYY-MM-DD-<customer-slug>.md`.

## What it does NOT do

* Does not run if no /intel brief exists. Tells you to run /intel first.
* Does not pull industry / competitive / vendor research itself. That's /intel's job. /shaper synthesizes only what's already on disk plus M365 context plus your input.
* Does not invent M365 context, customer details, or deal-stage facts. If it's not in your inbox or Teams or calendar, it's not in the card.
* Does not send the prep card outbound (email, Teams) without you explicitly asking. Default is save-and-display only.
* Does not produce more than 5 talking points or 5 discovery questions. Discipline matters; this is a prep card, not a dossier.

## Pairs with /intel

This skill is half of a two-skill pair. It consumes briefs produced by the sibling [`/intel`](../intel/) skill. The two skills share a canonical problem-tag set (defined in /intel's instructions). **If you change the tag set in /intel, you must update /shaper to match.** Otherwise the matching logic in Step 4 will silently miss items.

## Dependencies

* Clawpilot with Microsoft 365 connected (`m365_search_emails`, `m365_list_events`, `m365_search_chats`).
* An existing /intel skill that writes briefs to a known vault location with a `For the Shaper` section in the expected format.
* A writable notes vault for storing prep cards.
* PowerShell on the host (for vault read/write fallback when the vault is outside the filesystem MCP allowlist).

## What to customize before using

Search `SKILL.md` for `XXXX` and `<your-notes-vault-path>` and replace each instance. The big ones:

* **Your name** — referenced throughout for ownership.
* **Your role / vendor / workload / industry** — the opening paragraph names all four. Replace with your actual context. These should match what you set in /intel.
* **Vault path** — where /intel writes briefs and where /shaper writes prep cards. Same vault as /intel.
* **Style rules** — voice rules, banned filler, em-dash policy. Match what you set in /intel.
* **Problem tag set reference** — the skill says "matches via the canonical tag set defined in your /intel skill." Make sure the tag set in your customized /intel matches the one your customized /shaper references.

## Privacy posture

* Prep cards are saved to your personal vault and contain the customer name in the filename and body. That's intended for your private use.
* M365 enrichment data (email subjects, meeting attendees, Teams thread topics) is used in your reasoning context only. The saved prep card summarizes at the topic level ("recent thread about pricing"), never quoting verbatim email content.
* Default behavior is save-and-display only. The skill never sends the prep card to anyone, including the customer.

## Source

This skill is part of [Claw Skills](../../README.md), a library of Clawpilot custom skills.
