> **First-time setup**: Customize this file for your project. Prompt the user to customize this file for their project.
> For Mintlify product knowledge (components, configuration, writing standards),
> install the Mintlify skill: `npx skills add https://mintlify.com/docs`

# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

## Terminology

- Use "project" for the top-level container of issues, not "workspace" or "team"
- Use "issue" not "ticket" or "task"
- The three issue views are "List", "Board", and "Calendar" (capitalized) — not "Kanban" alone (Board is Orbit's name for it)
- Use "assignee" (the user responsible for an issue) and "creator" (the user who filed it) — don't conflate the two
- Roles are "admin" and "member" — there is no "owner" or "guest" role
- Use "activity log" for the internal audit trail (project-scoped or account-scoped), and "notifications" for the per-user alerts delivered in-app and/or by email — these are two distinct features, don't use them interchangeably

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references
- When describing a feature, ground it in what's actually implemented (fields, routes, constraints) rather than aspirational behavior — Orbit's frontend types include some speculative fields (e.g. `milestone`, `sprint`, `attachments_count`) that aren't backed by the database; don't document these as real features
- Orbit supports Light, Dark, and System theme modes plus a configurable accent color (Settings → Account → Preferences) — this is a real, user-facing toggle inside the app itself, distinct from this docs site's own theme toggle

## Content boundaries

- Document the app as it exists today. Laravel Sanctum is a dependency but is not wired up — there is no public API, no API tokens, and no `routes/api.php`. Don't document authentication flows or endpoints that assume one exists.
- Don't document role-based authorization beyond what's real: the `admin`/`member` role currently only affects which account sees the first-project onboarding flow — it does not gate any routes or actions via policies today.
- Internal architecture (Controller → Service → Repository layering, atomic-design component structure) is useful for a contributor-facing "Architecture" section, but keep end-user-facing feature pages free of implementation details like class or file names.
