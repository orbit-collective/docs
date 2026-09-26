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
- The four issue views are "List", "Board", "Calendar", and "Activity" (capitalized) — not "Kanban" alone (Board is Orbit's name for it)
- Use "assignee" (the user responsible for an issue) and "creator" (the user who filed it) — don't conflate the two
- Project role tiers are "Owner", "Admin", "Member", and "Viewer". Projects can also define custom roles; don't call any tier a "guest" role.
- Use "workflow status" for a type-specific state and "workflow category" for its broader To do, In progress, or Done grouping. Don't describe every issue as moving through one global three-status workflow.
- Use "project label" for an entry in a project's configurable label catalog. Don't present the six starter labels as a fixed global enum.
- Use "activity log" for the project history shown on the dashboard and in a project's Activity view, and "notifications" for per-user alerts delivered in-app and/or by email. These are distinct features; don't use them interchangeably.

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references
- When describing a feature, ground it in what's actually implemented (fields, routes, constraints) rather than aspirational behavior — Orbit's frontend types include some speculative fields (e.g. `milestone`, `sprint`, `attachments_count`) that aren't backed by the database; don't document these as real features
- Orbit supports Light, Dark, and System theme modes plus a configurable accent color (Settings → Account → Preferences) — this is a real, user-facing toggle inside the app itself, distinct from this docs site's own theme toggle
- Available integrations are Discord, Jira, and GitHub. Other cards in the integration catalog are marked as coming soon and must not be described as usable.
- Issue descriptions, comments, and issue type templates can upload images. Describe the controls available on each surface instead of treating every editor as identical.

## Content boundaries

- Document the app as it exists today. Laravel Sanctum is a dependency but is not wired up — there is no public API, no API tokens, and no `routes/api.php`. Don't document authentication flows or endpoints that assume one exists.
- Document actual project authorization. Orbit enforces project membership, four system role tiers, custom role grants, resource policies, and own-versus-any comment permissions. Verify sensitive access claims against committed policies and tests.
- Project labels, issue types, per-type workflows, custom fields, templates, hierarchy, integrations, and automation are real user-facing features under Settings. Describe their visible behavior and current limitations.
- The private `orbit-api` relay supports the GitHub integration. It is not a public Orbit API and must not be presented as one.
- This repository contains user documentation. The contributor-focused implementation guides in `orbit/documentation/en` and `orbit/documentation/pl` may be used to verify behavior, but their architecture and extension instructions do not belong in user-facing pages here.
- Internal architecture (Controller → Service → Repository layering, atomic-design component structure) is useful for a contributor-facing "Architecture" section, but keep end-user-facing feature pages free of implementation details like class or file names.
