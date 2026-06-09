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

## What's in this version (v2)

The `SKILL.md` in this repo is a **fully-worked example** anchored on a real 10-team Superflex Half PPR auction league (Sydney's Boys, $220 budget, 2-keeper rule). Owner team names have been swapped to Team A through Team J for privacy. Player names, prices, and calibration logic are real and instructive.

Three additions over the previous version:

### Room calibration model
A formal framework for anchoring auction $ recommendations to your specific league's historical prices, not generic markets. The worked example shows how Sydney's Boys consistently underpays QBs by 25-35% versus generic Superflex consensus, and how that intelligence flips the optimal QB build (two Tier-2 QBs > one elite + a dart). To use in your league, replace the `Room calibration model` section with your own room's price curves once you have at least one prior draft to anchor on.

### Workhorse identification framework
A five-signal model for finding mid-tier players (RB13-36 / WR13-36) with ceiling outcomes: target share, designed touches, red zone usage, route participation, and snap-share trajectory. Players hitting 3+ signals qualify as workhorse-tier targets; 4+ are anchors; all 5 are league-winners. Persisted to `workhorse-board.md` and refreshed across runs.

### Keeper landscape math
For leagues with keepers, the skill tracks which prior-year cheap-keeper players cannot be re-kept and therefore return to auction. This creates a structural supply shock every year that is routinely underpriced by the room. The worked example shows the 2026 forced-back-to-auction list (Drake Maye, Brock Bowers, Bo Nix, Bucky Irving, Rome Odunze, Brian Thomas Jr., etc.) — every year your league should produce its own equivalent list.

## Customization (required before first use)

The skill ships with the Sydney's Boys league context as a worked example. Before first invocation in your own league, replace the **League context** block at the top of `SKILL.md` with your actual:

* League name
* League type (1QB, Superflex, 2QB)
* Number of teams
* Scoring (PPR, half-PPR, standard, decimal yardage)
* Draft format (auction or snake, budget if auction)
* Starting roster slots
* Bench size
* Kicker / D-ST included or not
* Draft date
* Keeper rules (if any)
* Competitive context (sharp room or casual)
* Your league's owner names (or replace with Team A through Team N for privacy)

Then replace the **Room calibration model** section with your own room's historical price curves and per-owner spending profiles. If you do not have prior draft data, leave the Sydney's Boys priors as a starting point and update after your first auction.

Without these filled in, the skill will quote Sydney's Boys-specific intelligence that may not match your league dynamics.

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
