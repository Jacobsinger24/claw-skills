# Contributing to Claw Skills

Thanks for adding to the library. The bar is simple: every skill in this repo should be useful out of the box once placeholders are filled in.

## Adding a new skill

1. Create a directory under `skills/<skill-name>/`. Use kebab-case for the name. The name is what users will invoke (`/<skill-name>`).
2. Add two files:
   * `SKILL.md` — the actual instructions Clawpilot will execute. This is the body you'd pass to `m_create_skill`.
   * `README.md` — a short description, when to invoke, what it does, and what to customize before using.
3. Sanitize ruthlessly. Replace anything personal or identifying with `XXXX`. See [the XXXX convention](README.md#the-xxxx-convention) for what counts.
4. Update the table in the root [`README.md`](README.md) to list the new skill.
5. Open a PR.

## SKILL.md structure

There's no rigid template, but most skills benefit from these sections:

* **Purpose** — one paragraph on what this skill is for.
* **When to invoke** — explicit triggers, slash command and natural language phrases.
* **Operating principles** — non-negotiable rules the skill must follow every run.
* **Capabilities** — numbered list of what the skill can do, with output templates.
* **Anti-patterns** — things the skill must not do.
* **Output format** — how the skill should present its work.
* **Boundaries with other skills** — what this skill defers to.

## Sanitization checklist

Before opening a PR, search the file for:

* [ ] Your real name → `XXXX`
* [ ] Your manager / colleagues / customer names → `XXXX` or generic role
* [ ] Personal domain, blog name, project codename → `XXXX`
* [ ] Industry focus that's specific to you → `XXXX` or generic placeholder
* [ ] File paths with your username → use a generic `<your-path>` or `XXXX`
* [ ] References to other personal skills you haven't published → remove or generalize
* [ ] Specific email addresses, phone numbers, account IDs → remove

If in doubt, replace. The skill should make sense to a stranger who has never met you.

## Style

* **About drafted output**: many skills here advise the agent to avoid em dashes and AI tells when drafting on the user's behalf. That's a voice rule about the agent's output, not about the skill markdown itself.
* **About the skill markdown**: write for clarity. Dashes are fine if they improve readability. Plain text headings, no emojis on headings.
* Code blocks for templates and example outputs.
* Tables when comparing modes, sub-types, or capabilities.

## License

By contributing, you agree your changes are released under the MIT license.
