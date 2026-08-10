# Agent instructions

The project instructions for this repo live in **[`CLAUDE.md`](./CLAUDE.md)** — stack, structure,
non-negotiable rules, commit format, and the traps the local preview does not catch. Read it first,
whatever agent you are.

Per-subject conventions live in `.claude/skills/<name>/SKILL.md`. Load the one that matches what you
are about to touch:

| Editing                                        | Skill          |
| ---------------------------------------------- | -------------- |
| A new page, frontmatter, guide structure, MDX  | `pages`         |
| `docs.json`, groups, tabs, redirects           | `navigation`    |
| Colors, fonts, radii, logo, `custom.css`       | `design-system` |
| Prose, headings, voice                         | `writing`       |
| Code blocks, snippets, versions                | `code-samples`  |
| Product names, flow terminology, audience      | `domain`        |
| Examples with data, payloads, screenshots      | `privacy`       |

For Mintlify product knowledge (full component list, `docs.json` schema, latest features), use the
`mintlify` skill, the Mintlify MCP server (`https://mcp.mintlify.com`), or
[mintlify.com/docs](https://mintlify.com/docs). Those are the authority on the platform; the skills
above are the authority on this project, and they win where the two disagree.

Two rules worth repeating here, because they are the expensive ones:

- **Everything in this repo is public.** No holder PII, credentials, client names, or screenshots
  with real data — a committed secret must be rotated, not just deleted. See the `privacy` skill.
- **Nothing is committed without an explicit request.** Conventional Commits in English, subject
  only, no body, no agent co-author or signature. See `CLAUDE.md`.
