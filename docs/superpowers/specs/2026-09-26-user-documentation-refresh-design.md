# Orbit user documentation refresh design

## Objective

Bring the standalone Mintlify documentation up to date with the user-facing
behavior added to Orbit since the documentation repository was last changed.
The result must help people use Orbit. It must not duplicate the contributor
and implementation documentation stored in `orbit/documentation/en` and
`orbit/documentation/pl`.

## Baseline and source of truth

- The documentation baseline is commit `97d1fa6`, created on 23 August 2026 at
  13:55 CEST.
- The matching Orbit baseline is commit `b4de8b4`, the v0.4.0 release.
- The implementation target is committed Orbit state `78d3f51` on branch
  `FE-1391`, including committed work through 26 September 2026.
- Uncommitted changes in the Orbit working tree are explicitly excluded because
  they are still in progress.
- Application code, routes, policies, tests, migrations, and frontend behavior
  are authoritative. Orbit's technical documentation is supporting evidence,
  not content to copy.
- `orbit-api` may be inspected to verify the GitHub relay behavior. `orbit-web`
  is context only and does not define application features documented here.

## Audience and writing rules

The audience is an Orbit user or project administrator. Pages remain in English
and follow the existing Mintlify voice: concise active sentences, second person,
sentence-case headings, and bold labels for visible UI controls.

Pages describe what a user can do, where to find it, which role or permission is
needed, and important limitations. They do not name controllers, services,
database tables, routes, enum cases, or internal event-processing mechanics.

Orbit terminology remains consistent:

- Project is the top-level container.
- Issue is the unit of work.
- The issue views are List, Board, Calendar, and Activity.
- Creator and assignee are distinct.
- Activity log and notifications are distinct.
- Project roles are Owner, Admin, Member, Viewer, plus project-defined custom
  roles. The old two-role description is obsolete.

## Information architecture

Existing pages should be updated when a feature naturally belongs to them. New
pages should be added when a feature needs a complete task-oriented explanation.

### Existing pages to revise

- `index.mdx` and `quickstart.mdx`: update the product overview and next steps.
- `concepts/projects.mdx`: explain project membership and project-scoped
  configuration.
- `concepts/issues.mdx`: replace the fixed status/type model with issue types,
  workflows, hierarchy, templates, and custom fields.
- `concepts/comments.mdx`: document editing, mentions, formatting, images, and
  the permissions that control comment actions.
- `concepts/dashboard.mdx` and `concepts/activity-log.mdx`: describe the visible
  recent activity feed and the project Activity view.
- `concepts/notifications.mdx`: document mention notifications, filters, read
  state, and deletion without claiming unfinished invitation behavior.
- `concepts/settings.mdx`: reflect the per-tab URLs, project selector, enabled
  Workspace pages, and remaining unavailable pages.
- `guides/views.mdx`: add Activity and current Calendar behavior.
- `guides/filtering-and-search.mdx`: use project-defined labels, workflow
  statuses, and the currently supported view behavior.
- `guides/keyboard-shortcuts.mdx`: document the current generated shortcut list
  and view shortcuts.
- `guides/rich-text-editing.mdx`: document image paste/drop/upload behavior in
  issue descriptions, comments, and templates.
- `guides/roles-and-permissions.mdx`: replace the obsolete admin/member-only
  explanation with project membership, system tiers, custom roles, and actual
  permission enforcement.
- `reference/faq.mdx`, `reference/glossary.mdx`, and
  `reference/known-limitations.mdx`: remove resolved limitations and add current,
  user-relevant constraints.

### New user-facing pages

- `concepts/labels.mdx`: create, edit, color, describe, delete, and apply
  project-specific labels.
- `concepts/issue-types.mdx`: built-in and custom types, type restrictions,
  hierarchy, fields, templates, and deletion constraints.
- `guides/workflows.mdx`: initial status, status categories, transitions,
  ordering, and dead-end warnings.
- `guides/members-and-roles.mdx`: invite and manage project members, assign base
  tiers and custom roles, transfer ownership, and understand access boundaries.
- `guides/integrations.mdx`: integration catalog, project selection, permissions,
  Discord configuration, and the difference between available and coming-soon
  integrations.
- `guides/jira-import.mdx`: credentials, field mapping, hierarchy, existing-item
  synchronization, progress, and deduplication as visible to the user.
- `guides/github-integration.mdx`: connect the GitHub App, manage repositories,
  link pull requests, create branches and pull requests from an issue, and read
  lifecycle, CI, and review state.
- `guides/automation.mdx`: rule triggers, AND conditions, ordered actions,
  enable/disable behavior, defaults created by GitHub connection, and current
  builder limits.

`docs.json` will place these pages in the existing Core Concepts and Guides
groups. No new technical-documentation group will be introduced.

## Content boundaries

The refresh will document committed functionality only. In particular:

- GitHub branch and pull-request creation is documented as available in the
  committed application, but the separate permission currently present only in
  the dirty Orbit working tree is not documented.
- Public Orbit API authentication remains unavailable. The private `orbit-api`
  relay used by the GitHub integration is not presented as a public API.
- Catalog cards marked coming soon are clearly separated from Discord, Jira,
  and GitHub, which are usable.
- Priorities, Documents, and Export remain unavailable settings pages unless
  committed code proves otherwise.
- Implementation details from `orbit/documentation` are omitted even when they
  were useful to verify behavior.
- Architecture and deployment pages are not broadly rewritten. Only user-facing
  setup text that became factually incorrect may be corrected.

## Commit strategy

Changes will be split into approximately 14 to 17 independently reviewable
Conventional Commits. Each commit will cover one user concept or closely coupled
set of pages. Planned slices are:

1. documentation baseline and contributor guidance;
2. navigation and product overview;
3. visible activity and dashboard behavior;
4. issue views and Calendar behavior;
5. comments, mentions, and notifications;
6. image uploads and rich-text editing;
7. project labels;
8. issue types and issue model;
9. workflows;
10. hierarchy, templates, and custom fields;
11. members, roles, and permissions;
12. settings navigation and availability;
13. integrations and Discord;
14. Jira import;
15. GitHub integration;
16. automation;
17. FAQ, glossary, and limitations cleanup.

Exact grouping may change when a page depends on navigation or links introduced
in the same commit. Commit messages stay short, in English, and use forms such
as `docs: document project labels`. No co-author trailers will be added.

## Verification

Before completion:

- parse `docs.json` as JSON;
- run the documentation project's available validation or Mintlify command if
  one is configured locally;
- validate MDX/frontmatter with the locally available tooling;
- scan all internal documentation links and navigation entries for missing
  targets;
- search for obsolete claims, including three views, fixed labels, fixed
  statuses, two roles, unavailable activity UI, unavailable integrations, and
  unsupported image insertion;
- compare sensitive claims about roles, permissions, GitHub, Jira, and
  automation with committed application code and tests;
- confirm only `/docs` changed and the `/orbit`, `/orbit-api`, and `/orbit-web`
  working trees were not modified;
- inspect the final Git history and ensure the requested Conventional Commits
  are present without co-author trailers.

## Success criteria

A reader can discover every major committed Orbit capability added since v0.4.0,
follow the steps needed to use it, and understand its current constraints. No
page contradicts the committed application state, and no user-facing page drifts
into contributor-focused implementation detail.
