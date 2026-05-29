# /ea — Executive Assistant

A Clawpilot skill that acts as your executive assistant. Triages your inbox, manages your calendar, preps you for meetings, tracks open loops, and harvests action items from recorded meetings you accepted.

## When it triggers

* `/ea`, "be my EA", "EA mode", "run an EA pass"
* "what does my day look like", "prep me for today", "daily brief"
* "triage my inbox"
* "what do I owe people", "what did I commit to"
* Any request spanning multiple M365 surfaces (calendar + email + Teams + people)

## What it does

1. **Daily brief** — schedule, inbox pulse, Teams unread, open loops, suggested moves, all in one screen.
2. **Inbox triage** — classifies threads into Reply today / Reply this week / FYI / Noise / Needs decision, drafts replies to your Drafts folder (never sends).
3. **Meeting prep** — pulls event, attendees, prior context, surfaces talking points and decisions you'll need to make.
4. **Calendar management** — checks availability before proposing times, holds focus blocks, keeps RSVPs generic for privacy.
5. **Follow-up tracking** — surfaces commitments from sent mail, Teams @mentions, and meetings.
6. **Recorded meeting harvest** — for meetings you RSVPed accepted to, pulls facilitator notes (Copilot) or transcript fallback, extracts action items you own.
7. **End of day wrap** — recap, drift flags, setup for tomorrow.

## Dependencies

* Clawpilot with Microsoft 365 (`m365_*` tools) connected. The skill leans heavily on `m365_list_events`, `m365_list_emails`, `m365_list_chats`, `m365_get_facilitator_notes`, `m365_get_transcript`, `m365_find_meeting_times`, `m365_get_schedule`, `m365_search_people`.
* For meeting harvest specifically: Microsoft 365 Copilot license (for facilitator notes), or meeting transcription enabled (for transcripts).

## What to customize before using

Search `SKILL.md` for `XXXX` and replace each instance. You'll find:

* **Your name** — referenced throughout for ownership ("XXXX's calendar", "XXXX's commitments"). Replace with your actual first name.
* **Voice / tone preferences** — the skill enforces "no em dashes" and other specific style rules. Adjust to match your own writing voice if different.
* **VIPs and important people** — currently sourced from `m365_get_relevant_people` plus manager / direct reports. If you want specific names hard-prioritized, list them.
* **Customer confidentiality language** — the skill says "never name a customer in any outbound content". If your work doesn't involve customers (or your rules are different), adjust.
* **Boundary references** — the skill defers to other skills like `/linkedinpostwriter`, `/expense-report`, `/docx`, etc. Adjust this list to whatever skills you actually have installed.

## Privacy posture

This skill is **paranoid about your data and conservative about outbound communications**:

* Never sends emails, RSVPs, Teams messages, or calendar invites without explicit per-item approval.
* Never reveals your schedule, location, or reason for unavailability to third parties.
* Keeps RSVP decline comments generic ("conflict, unable to attend") — never names the actual conflict.
* Never moves, archives, or deletes email without per-item approval.
* Surfaces tentative meetings for your decision rather than silently bucketing them.

If you want a more aggressive EA that auto-sends drafts or auto-archives noise, you'll need to relax those rules in the skill markdown.

## Source

This skill is part of [Claw Skills](../../README.md), a library of Clawpilot custom skills.
