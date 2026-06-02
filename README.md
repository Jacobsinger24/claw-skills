# Claw Skills

A growing library of Clawpilot custom skills, ready to copy, customize, and use.

Each skill in this repo is a fully formed instruction set that turns Clawpilot into a specialized agent — an executive assistant, a career coach, a writer, a coordinator, whatever you need. Drop one into your Clawpilot install, swap the `XXXX` placeholders for your own details, and go.

## What's in here

| Skill | What it does |
|---|---|
| [`/ea`](skills/ea/) | Executive assistant. Triages inbox, manages calendar, preps meetings, tracks open loops, and harvests action items from recorded meetings you accepted. |
| [`/coach`](skills/coach/) | Full-stack career coach with three modes: strategic (zoom out), weekly (zoom in), and prep (specific events). Maintains a persistent career file over time. |
| [`/intel`](skills/intel/) | Industry-led research sweep. Pulls industry pressures, competitive ecosystem moves, and internal vendor news, then synthesizes into a meeting-ready brief. Industry leads; vendor news only surfaces when it answers a pressure. Pairs with /shaper. |
| [`/shaper`](skills/shaper/) | Customer-scenario shaper. Takes a free-form customer scenario you provide, pulls fresh M365 context on the named account, asks for any final input, then reshapes the most recent /intel brief into a meeting-ready prep card (talking points, discovery questions, watch-outs, next step). |

More coming as the collection grows.

## How to use a skill

1. Pick a skill from the [`skills/`](skills/) directory.
2. Open its `SKILL.md` file. That's the full instruction set.
3. Find every `XXXX` in the file. Each one is a placeholder for something personal — your name, your domain, your industry focus, your file paths, your specific references. Replace them with your own details.
4. In Clawpilot, register the skill via `m_create_skill` (or whatever the equivalent is in your client) using the customized instructions as the body. Use the skill's folder name as the invocation name (e.g. `ea`, `coach`).
5. Invoke it with `/<skill-name>` or the natural-language triggers listed in the skill itself.

Each skill also has a `README.md` walking through what it does, what triggers it, and what you specifically need to customize.

## The XXXX convention

Anything personal or identifying in this repo is replaced with `XXXX`. That includes:

* Names (the user's name, manager's name, anyone specific)
* Domains and URLs (personal sites, company-specific links)
* Industry focus (specific verticals the user works in)
* Roles (job title, organization, team)
* File paths (anywhere a username or vault name would appear)
* Project names (specific apps, blogs, or codenames)

If you see `XXXX` in a skill, that's a "fill this in for yourself" marker. The skills are designed so that even with placeholders, the structure and operating principles still make sense.

## Why this exists

Custom skills are where Clawpilot stops being a chatbot and starts being a system that fits the way you actually work. They take time to design well. This repo is an attempt to share the design work, not the user data.

Use the templates as starting points. Fork, customize, and build your own.

## Adding a new skill

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).
