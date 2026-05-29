# /coach — Career Coach

A Clawpilot skill that acts as your full-stack career coach. Three explicit modes: strategic (zoom out), weekly (zoom in), and prep (specific events). Maintains a persistent career file over time so your data accumulates and the coaching gets sharper.

The bar is not a cheerleader. It's a coach who tells the truth, pushes back when your calendar disagrees with your stated goals, and grounds advice in your actual data instead of generic career platitudes.

## When it triggers

* `/coach`, "career coach", "coach mode", "how am I doing", "am I drifting"
* **Strategic**: `/coach strategic`, "let's zoom out", "stay or move", "what's my narrative"
* **Weekly**: `/coach weekly`, "review my week", Friday afternoon reviews
* **Prep**: `/coach prep`, "prep me for my 1:1", "prep me for the skip level", "promo case prep", "interview prep", "I have a hard conversation"
* Any career decision you're wrestling with

## What it does

### Mode A — Strategic
Pulls your stated goals, your time map (where your calendar says your priorities actually are), and your output signal (what your sent mail and public writing say you're becoming known for). Names the tension between them. Picks the one move worth making this quarter.

### Mode B — Weekly
Time map, shipped artifacts, theme of the week, drift signal vs. stated priorities, promo evidence captured to the career file. Ends with one question to carry into next week.

### Mode C — Prep (six sub-types)
* **C1** — Manager 1:1 prep
* **C2** — Skip-level or executive 1:1 prep
* **C3** — Promo case prep (evidence walkthrough + gap analysis)
* **C4** — Difficult conversation prep
* **C5** — Public talk / panel / podcast prep (strategic frame only)
* **C6** — Offer or interview prep

## The career file

The skill maintains a persistent markdown file that accumulates your north star, weekly check-ins, decisions log, promo evidence, themes, and open questions. Without this file the coaching doesn't get smarter over time.

* **Default location**: a path under your personal knowledge vault. On first run the skill asks you where to put it.
* **Update rules**: weekly mode appends, strategic mode updates north star + decisions, prep mode reads but rarely writes.
* **Never overwrites**. Always appends or edits in place with visibility.

## Dependencies

* Clawpilot with Microsoft 365 connected. Uses `m365_list_events` heavily for time maps, `m365_list_emails` for output signal, `m365_get_facilitator_notes` for commitments harvest.
* A writable directory for the career file (suggest somewhere in your personal notes / wiki / vault).
* Works best alongside the [`/ea`](../ea/) skill, which it can pull time-map data from.

## What to customize before using

Search `SKILL.md` for `XXXX` and replace each instance. You'll find:

* **Your name** — referenced throughout. Replace with your first name.
* **Career file default path** — currently `XXXX`. Replace with where you actually want the file to live (e.g., your Obsidian vault, your OneDrive notes, a plain folder).
* **Public presence references** — the skill mentions a personal blog and LinkedIn as output signal sources. If you have different surfaces (Substack, talks, podcast), swap them in.
* **Industry / role-specific references** — the strategic mode looks for industry-specific signals. Replace with your actual industry focus, role context, and org type.
* **Boundary references** — the skill defers to `/linkedinpostwriter` for posts, `/docx` for promo packets, `/ea` for calendar actions. Adjust to your installed skill set.

## Privacy posture

This skill is **strict about other people's privacy**:

* Never writes anyone else's name into the persistent career file. Uses role / relationship instead ("your manager", "a peer in the same org").
* Never stores customer names anywhere. Generalizes to industry only.
* Never repeats third-party private info even within the coaching session itself.

## Operating philosophy

If the skill flatters you, it's broken. If it tells you exactly what to do without asking the harder question first, it's broken. If it gives you generic LinkedIn-thought-leader career advice instead of grounding everything in your actual data, it's broken.

The whole point is the friction between what you say you want and what your behavior shows you want. Don't soften that.

## Source

This skill is part of [Claw Skills](../../README.md), a library of Clawpilot custom skills.
