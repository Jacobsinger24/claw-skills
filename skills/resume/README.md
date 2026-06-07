# /resume — Professional Resume Writer

A Clawpilot skill that acts as your professional resume writer. Five explicit modes: build (intake + master construction), tailor (master to a specific role/JD), bullet (workshop a single accomplishment line in chat), review (critique an existing resume), and ats (keyword + format audit against a target JD).

The bar is not a wordsmith on the surface. It's a resume strategist who decides what evidence belongs on the page, what language hiring managers actually respond to, what an ATS parser will accept, and what narrative arc the document tells when read in 8 seconds vs 8 minutes. Equal parts branding, writing, strategic thinking, and revenue-function industry expertise.

The example vocabulary in this skill is calibrated to Microsoft MCAPS sellers (GBB, SSP, TSA, CSA, ATU/STU, ACR, MEDDPICC, Solution Plays, etc.). The structure works for any enterprise sales org once you swap the terms.

## What's in this version

This skill carries a packaged reference layer on top of the five modes. Three sections that get consulted on every run:

* **2025-2026 best-in-class trends** — AI-augmented ATS behavior (Workday, Greenhouse, iCIMS, Lever, Ashby, MyHire), the 7.4-second scan, AI-written-resume detection patterns to avoid, length norms by seniority bucket, keyword optimization in the AI-ATS era, quantification standards, and what sits at the top of a 2026 resume.
* **Editorial rules baked from execution** — ten hard rules that emerged from real resume builds (no personal $ paired with team $ on the same page, scope statements pass the "owned vs did" test, scope statements end with the charter-triplet of measurement + people-system + product-feedback, promotion citations use "in recognition of" not "for", and more).
* **Visual systems library** — three named, packaged systems (Editorial / Microsoft-Native / Consulting Pitch). Each is fully ATS-safe. Pick one per outbound based on the target audience.

Plus a **Render contract** section that documents the exact python-docx recipe (Calibri 11pt, letter-spaced caps, hairline rules, numbered bullets, page 2 continuity header, same-color hyperlinks) for executive presence on paper without breaking ATS parsing.

## When it triggers

* `/resume`, "help me with my resume", "review my resume"
* **Build**: `/resume build`, "let's rebuild my resume", "start my resume from scratch"
* **Tailor**: `/resume tailor <role>`, "tailor my resume for X", "cut me a version for this posting" (always paste the JD)
* **Bullet**: `/resume bullet`, "rewrite this bullet", "make this stronger"
* **Review**: `/resume review`, "tear apart this resume", "is this any good"
* **ATS**: `/resume ats`, "is this ATS friendly", "what keywords am I missing"

## What it does

### Mode A — Build
Pulls existing evidence from your coach career file, your /ea commitment log, your sent mail to senior recipients, and your accepted recurring customer meetings. Interviews you for gaps role-by-role. Writes a maximalist `master.md` (4–6 pages, all evidence, never circulated) that becomes the source of truth every outbound resume is generated from.

### Mode B — Tailor
Reads `master.md` + the JD you paste. Parses required and preferred quals. Runs an honest gap analysis (strong match / light match / gap). If 3+ requireds are gaps, flags the stretch before drafting. Reshapes content from the master into a 2-page tailored `.docx` saved under `career\tailored\<role>-YYYY-MM-DD.docx`. Also produces a one-paragraph "why I fit" note suitable for a sponsor intro or hiring manager note.

### Mode C — Bullet
Workshops a single accomplishment line in chat. Pushes you for situation + action + measurable outcome. Produces three variants (outcome-first, action-first, scope-first), each labeled by when to use it. Fast, chat-only, optional save to master.

### Mode D — Review
Critiques an existing resume against headline read, scope statements, quantification rate, action verb quality, voice, vendor fluency, ATS safety, narrative arc, and length. Outputs a triaged findings list (must fix / should fix / nice to have / strengths to preserve). Never silently rewrites.

### Mode E — ATS
Format pass-fail audit + keyword coverage matrix against a target JD. Estimates keyword match as a range (not a precise score). Surfaces the top 5 honest swaps to make right now and the keywords NOT to add (would be overclaiming).

## The career folder

The skill maintains a private folder on disk that holds everything it produces.

* **Default location**: `<your-private-folder-path>\career\`. On first run the skill checks if it exists and creates it (with a `README.md` describing the privacy posture and file layout) if not.
* **Files**:
  * `master.md` — maximalist source of truth, never circulated as-is.
  * `current.docx` — latest polished outbound, overwritten when you designate a new active outbound.
  * `tailored\<role>-YYYY-MM-DD.docx` — per-target tailored versions, write-once, history preserved.
  * `evidence-inbox.md` — rolling capture of accomplishments to triage at next build.
* **Update rules**: only `master.md` and `evidence-inbox.md` get edited in place. Tailored docs are write-once.

## Privacy posture

This is the strictest privacy skill in the library. The threat model is "colleagues stumbling into my resume drafts via sharing, link previews, or search."

* Owner-only at the filesystem ACL. Skill never generates sharing links for anything in the folder.
* Skill never links any file in the folder inside Teams messages, emails, calendar invites, Loop pages, or shared documents.
* Skill never moves files out of the folder into a SharePoint site, shared cloud folder, or external location on your behalf.
* Outbound `.docx` resumes leave the folder only when **you** personally attach one to a recruiter or hiring manager.
* Tenant admin retains technical access to any cloud-synced folder. That's known and accepted. The goal is colleague-private, not absolute-private. If you want absolute-private, use a non-synced local path.
* Customer names never appear in any artifact (master or outbound). Always generalized to industry segment.
* Manager / colleague names never appear in outbound. Internal context (level, comp band, manager names, sponsor relationships) lives only in master.md's "Internal context" section.

## Dependencies

* Clawpilot with Microsoft 365 connected (`m365_list_emails`, `m365_list_events`) for evidence harvest.
* The [`/docx`](../../skills/docx/) skill (or your client's equivalent) for rendering `.docx` files with ATS-safe formatting.
* PowerShell on the host (for the folder bootstrap and the privacy-rule README write).
* Works best alongside [`/coach`](../coach/) (provides the persistent career file the skill pulls promo evidence from) and [`/ea`](../ea/) (provides the inbox_entries commitment log).

## What to customize before using

Search `SKILL.md` for `XXXX` and `<your-private-folder-path>` / `<your-notes-vault-path>` and replace each instance. The big ones:

* **Your name** — referenced throughout for the resume voice and personalization.
* **Private folder path** — currently `<your-private-folder-path>\career\`. Replace with the actual directory where you want resume artifacts to live (Desktop, Documents, an external drive, anywhere you control).
* **Notes vault path** — currently `<your-notes-vault-path>\_meta\coach\career.md`. Replace with wherever your `/coach` skill stores its career file.
* **Vendor / org-specific vocabulary** — the entire "Vendor / GTM domain fluency" section uses Microsoft MCAPS as a worked example (MCAPS, STU, ATU, GBB, SSP, TSA, CSA, CSAM, ACR, MEDDPICC, Solution Plays, AZ-/MS-/SC-/AI- cert codes, IC and M-level naming, Modern Work / Security / Azure / Data and AI / BizApps solution areas, HLS / FSI / Retail / Manufacturing / Public Sector industries). Swap the entire section for your vendor's vocabulary. The **structure** (org nouns, KPI nouns, methodology nouns, leveling nouns, cert nouns) holds across any enterprise sales org; the **terms** are vendor-specific.
* **Industry focus** — examples in the privacy section reference healthcare provider / payor / life sciences segments. Swap for your actual industries.
* **Boundary references** — the skill defers to `/coach`, `/ea`, `/docx`, and `/linkedinpostwriter`. Remove or rename any you don't have installed.

## Style rules built in

* No em dashes, no en dashes — anywhere, including date ranges in outbound docs.
* No first-person pronouns in bullets.
* No "responsible for", "helped with", "assisted in", "worked on", "involved in".
* No "results-driven", "team player", "passionate about", "best-in-class", "synergy", "navigate the landscape", "in today's fast-paced world", "leverage" as a verb.
* Compound-modifier hyphens (multi-step, line-of-business, customer-facing, end-to-end) are allowed and encouraged.
* Customize the banned-word list to your own voice if you want different tells flagged.

## Operating philosophy

If the skill writes bullets without numbers, it's broken. If it inflates scope to make you sound more senior than your evidence supports, it's broken. If it produces a "creative" two-column design with icons and a skills sidebar, it's broken (the ATS will reject it). If it shares anything to anyone on your behalf, it's broken.

The whole point is: a resume that survives an ATS parser AND a hiring panel pressure test AND tells a coherent narrative about who you're becoming. Quantify or strike. Honest or cut. Private until you personally hit send.

## Source

This skill is part of [Claw Skills](../../README.md), a library of Clawpilot custom skills.
