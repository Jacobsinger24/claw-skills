# /fantasyguru

**Fantasy Football Research Agent**

A Clawpilot skill that acts as an elite fantasy football research and decision-support agent built specifically for your league. The bar is not a generic fantasy writer. It is a sharp, analytical, high-signal researcher whose job is to identify market inefficiencies, breakout candidates, undervalued quarterbacks (especially in Superflex), and weekly add/trade/start decisions that give you an edge over the rest of your league.

Operates in two auto-switching modes based on the date: **Pre-Draft Mode** (auction targets, sleepers, market inefficiencies, draft strategy) and **In-Season Mode** (waiver wire, trade targets, weekly upside, breakout watchlist, sell-high calls). Maintains a persistent league state under your personal vault so research compounds across runs.

## When it triggers

* `/fantasyguru`, `/fg`
* `/fg pre-draft`, `/fg in-season` (manual mode override regardless of date)
* "draft prep", "auction strategy", "auction values"
* "waiver targets", "who should I add", "who should I drop", "FAB bid"
* "trade for X", "trade target", "sell high", "buy low"
* "sleeper picks", "late round flyers", "diamonds in the rough"
* "Superflex QB", "QB strategy", "should I draft a third QB"
* "who should I start" (lineup decisions, In-Season Mode)
* "compare X vs Y" or "X or Y" (player comparison)
* Any fantasy football decision you are weighing

## What it does

### Pre-Draft Mode

* Builds a tiered draft board with cost ranges per player tuned to your exact league size, scoring format, and budget
* Identifies market inefficiencies versus FantasyPros / Yahoo / ESPN / FantasyGuru consensus
* Flags Superflex QB scarcity dynamics (if applicable) and surfaces backup QBs with paths to starts
* Translates coaching, scheme, and coordinator changes into actual fantasy opportunity
* Highlights sleepers and late-round flyers grounded in usage trends, not name recognition
* Builds a complete auction strategy across spend allocation, leverage points, and where the room is likely to overpay

### In-Season Mode

* Weekly waiver and free agent targets with FAB bid recommendations
* Trade targets with proposed packages, separating buy-low from sell-high
* Breakout watchlist for the players to add before the market catches up
* Coaching, scheme, and role changes translated into immediate fantasy moves
* Start/sit and lineup decisions filtered through your specific roster construction

### Player comparisons

When you ask "Player A vs Player B" or "X or Y", returns a structured comparison across talent, role, usage profile, floor, ceiling, injury risk, offensive environment, scheme fit, format-specific value, and acquisition cost. Then commits to a single recommendation.

## The league state

The skill maintains a persistent set of markdown files at a path under your personal vault. Without these files, the research does not compound across runs.

* `league.md` : League settings, draft date, opponent tendencies. Source of truth.
* `roster.md` : Your current roster, updated as you pick / add / drop / trade.
* `draft-board.md` : Live tiered target list with cost ranges. Grows across runs.
* `watchlist.md` : Pre-season risers, camp battles, preseason watch list.
* `transactions.md` : In-season log of every move.
* `weekly-notes.md` : In-season observations, lineup decisions, what worked.

On first run, the skill asks where to put these files (default suggestion: `<vault>/_meta/fantasy/`).

## Source priority

Every invocation pulls fresh data via `web_fetch` from, in order:

1. FantasyPros (consensus rankings, ECR, auction values)
2. Yahoo Fantasy Football (rankings, projections, news)
3. ESPN Fantasy Football (rankings, projections, depth charts)
4. Fantasy Guru / John Hansen (analyst tier writeups)

Paired with official NFL/team announcements, injury reports, beat reporters, and verifiable usage data. Sources are cited inline so you can verify.

If a source is unreachable on a run, the skill says so explicitly rather than silently falling back to stale priors.

## Customization (required before first use)

The skill ships with placeholders for league settings. Before first invocation, fill in the **League context** block at the top of `SKILL.md` with your actual:

* League name
* League type (1QB, Superflex, 2QB)
* Number of teams
* Scoring (PPR, half-PPR, standard, decimal yardage)
* Draft format (auction or snake, budget if auction)
* Starting roster slots
* Bench size
* Kicker / D-ST included or not
* Draft date
* Competitive context (sharp room or casual)

Without these filled in, the skill cannot tailor recommendations to your actual edge.

## Mode cutover

Default: Pre-Draft Mode before September 1 of the current NFL season, In-Season Mode after. Override the cutover date once your actual draft date is set.

## What it will not do

* Will not blindly follow consensus rankings
* Will not give vague "high upside" answers without saying upside to what
* Will not write generic fantasy content
* Will not skip the league-specific context layer
* Will not use em dashes or en dashes (cleaner cross-platform rendering, no AI-tell signature)

## Dependencies

* `web_fetch` for live source pulls
* `filesystem-*` tools for persistent state (read/write under your vault)
* `m_ask_user` for first-run setup and ambiguous decisions

## Installation

1. Copy `SKILL.md` content
2. In Clawpilot, run: `m_create_skill` with `name: "fantasyguru"` and paste the SKILL.md body as `instructions`
3. Fill in your league context block before first invocation
4. Invoke with `/fantasyguru` or any natural-language trigger above
