# Orbit User Documentation Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update the standalone Mintlify site so it accurately explains every major committed, user-facing Orbit capability added since v0.4.0.

**Architecture:** Treat committed application code at `orbit` commit `78d3f51` as the source of truth, then organize the result by user task rather than by release or backend subsystem. Existing pages keep their established purpose; substantial new capabilities get focused concept or guide pages and are connected through `docs.json` and contextual links.

**Tech Stack:** Mintlify, MDX, JSON, Git

**Spec:** `docs/superpowers/specs/2026-09-26-user-documentation-refresh-design.md`

## Global Constraints

- Write user-facing pages in English with concise active sentences and second-person instructions.
- Document only committed Orbit state through `78d3f51`; exclude the dirty FE-1391 working tree.
- Do not modify `/orbit`, `/orbit-api`, or `/orbit-web`.
- Do not copy implementation details from `orbit/documentation/en` or `orbit/documentation/pl` into user pages.
- Use Project, Issue, List, Board, Calendar, Activity, creator, assignee, activity log, and notifications consistently.
- Describe visible UI labels in bold and paths or literal values in code formatting.
- Keep public API limitations distinct from the private relay used by the GitHub integration.
- Use short English Conventional Commit messages without co-author trailers.

## Review Focus

- A feature present only in Orbit's uncommitted working tree must not appear in the docs; verify sensitive permission claims against `git show HEAD:<path>`.
- Coming-soon integration catalog entries must never read as available; name Discord, Jira, and GitHub as the usable integrations.
- Role and permission guidance must distinguish project Owner/Admin/Member/Viewer tiers, custom roles, and per-action grants without promising access a policy does not provide.
- Issue guidance must distinguish workflow status from its broad category and must not retain the old fixed Open/In Progress/Closed model.
- Internal links and both `navigation.pages` and `navigation.groups` must resolve to real MDX files after every new page is registered.

---

### Task 1: Correct documentation author guidance

**Files:**
- Modify: `AGENTS.md`

**Interfaces:**
- Consumes: committed Orbit terminology and content boundaries from the spec.
- Produces: accurate constraints for every later documentation task.

- [ ] **Step 1: Replace obsolete role and feature boundaries**

Update the terminology and content-boundary sections to recognize Owner, Admin, Member, Viewer, custom roles, real policy enforcement, Activity view, available integrations, automation, project labels, issue types, workflows, and image uploads. Preserve the rule that `/docs` is for users and `orbit/documentation` is for contributors.

- [ ] **Step 2: Scan guidance for contradictions**

Run: `rg -n "two roles|no policy|not wired|no public API|activity log" AGENTS.md`

Expected: only the still-correct public API boundary remains; no obsolete role or activity statement remains.

- [ ] **Step 3: Commit**

```bash
git add AGENTS.md
git commit -m "docs: refresh author guidance"
```

### Task 2: Refresh the product overview

**Files:**
- Modify: `index.mdx`
- Modify: `quickstart.mdx`

**Interfaces:**
- Consumes: the capability map in the spec and existing canonical page paths.
- Produces: an accurate overview without linking to pages before they exist.

- [ ] **Step 1: Rewrite the product capability summary**

Describe four issue views, project-specific issue types and workflows, collaboration, integrations, and automation. Remove the fixed three-status and fixed-label claims. Keep technical architecture confined to the existing short architecture section.

- [ ] **Step 2: Update first-use next steps**

Keep setup commands unchanged unless committed code proves them wrong. Point users from their first project toward existing Issues, Views, and Settings pages; focused feature links will be added alongside their canonical pages in later tasks.

- [ ] **Step 3: Commit**

```bash
git add index.mdx quickstart.mdx
git commit -m "docs: refresh product overview"
```

### Task 3: Document visible activity

**Files:**
- Modify: `concepts/activity-log.mdx`
- Modify: `concepts/dashboard.mdx`

**Interfaces:**
- Consumes: Activity view and dashboard feed behavior from committed controllers and UI components.
- Produces: correct distinction between activity entries, recent issues, and notifications.

- [ ] **Step 1: Replace the backend-only activity description**

Explain the project Activity view, its chronological entries, attribution, relative timestamps, and the types of changes users can see. Mention that account-only security history is not exposed as a separate user page.

- [ ] **Step 2: Correct the dashboard activity section**

Describe the current recent activity feed and keep issue summary cards separate from it.

- [ ] **Step 3: Verify stale claims are gone**

Run: `rg -n "not currently surfaced|no corresponding read path|isn't currently viewable|Recent Work Activity.*issue table" concepts/activity-log.mdx concepts/dashboard.mdx`

Expected: no matches.

- [ ] **Step 4: Commit**

```bash
git add concepts/activity-log.mdx concepts/dashboard.mdx
git commit -m "docs: document visible activity"
```

### Task 4: Update issue views, filtering, and shortcuts

**Files:**
- Modify: `guides/views.mdx`
- Modify: `guides/filtering-and-search.mdx`
- Modify: `guides/keyboard-shortcuts.mdx`

**Interfaces:**
- Consumes: current List, Board, Calendar, Activity, filter-bar, and shortcut behavior.
- Produces: one consistent explanation of navigating and narrowing project work.

- [ ] **Step 1: Document all four views**

Add Activity. Expand Calendar with week navigation, multi-day items, priority chips, issue previews, filtering, and upcoming deadlines. Explain that the default view comes from Preferences and view switching behavior follows the current app.

- [ ] **Step 2: Correct filters and sort fields**

Use project-defined labels and workflow statuses. Document where the filter bar is visible and avoid claiming one saved filter is scoped differently unless code enforces it.

- [ ] **Step 3: Refresh shortcut table**

Document project-page view shortcuts `1` through `4`, global shortcuts, context-specific shortcuts that are visible in the generated help modal, and the input-focus suppression rule.

- [ ] **Step 4: Commit**

```bash
git add guides/views.mdx guides/filtering-and-search.mdx guides/keyboard-shortcuts.mdx
git commit -m "docs: update issue views"
```

### Task 5: Document comments, mentions, and alerts

**Files:**
- Modify: `concepts/comments.mdx`
- Modify: `concepts/notifications.mdx`

**Interfaces:**
- Consumes: comment editing, mention tracking, notification filters, ownership checks, and delivery settings.
- Produces: an end-to-end collaboration guide from comment entry to recipient alert.

- [ ] **Step 1: Rewrite comment capabilities**

Document creation, editing, deletion, markdown image rendering, image insertion, and `@` mention suggestions. Explain own-versus-any behavior in terms of granted permissions, not internal permission names.

- [ ] **Step 2: Refresh notifications**

Mark Issue mentioned and Project invited as functional. Explain that existing users receive a normal Orbit notification while an email address without an account receives the dedicated invitation email. Document filter tabs, opening, read state, mark-all-read, and deletion.

- [ ] **Step 3: Validate notification claims against committed tests**

Run: `git -C ../orbit grep -n "IssueMentioned\|ProjectInvited" HEAD -- app tests | head -80`

Expected: evidence exists for every notification type described as functional.

- [ ] **Step 4: Commit**

```bash
git add concepts/comments.mdx concepts/notifications.mdx
git commit -m "docs: explain mentions and alerts"
```

### Task 6: Document rich text and image uploads

**Files:**
- Modify: `guides/rich-text-editing.mdx`

**Interfaces:**
- Consumes: committed attachment upload and markdown-image behavior across descriptions, comments, and templates.
- Produces: task-oriented image and formatting instructions without storage details.

- [ ] **Step 1: Replace the obsolete image limitation**

Explain paste and drag-and-drop in issue descriptions, paste behavior in comments and templates, upload progress/failure feedback, click-to-open rendering, accepted image constraints visible to users, and moderation failure behavior.

- [ ] **Step 2: Separate editor surfaces**

State which formatting features belong to issue descriptions and which text-based surfaces support markdown images without implying every Tiptap format is available everywhere.

- [ ] **Step 3: Commit**

```bash
git add guides/rich-text-editing.mdx
git commit -m "docs: document image uploads"
```

### Task 7: Add project label documentation

**Files:**
- Modify: `docs.json`
- Create: `concepts/labels.mdx`
- Modify: `concepts/projects.mdx`
- Modify: `guides/filtering-and-search.mdx`

**Interfaces:**
- Consumes: project label catalog and label permission behavior.
- Produces: `/concepts/labels` as the canonical label reference.

- [ ] **Step 1: Write the label concept page**

Cover starter labels, project scope, name/color/description editing, applying labels, create/update/delete permissions, and the visible consequence that renaming or deleting a label does not rewrite historical strings already stored on issues.

- [ ] **Step 2: Register and link the canonical page**

Add `concepts/labels` to both Core Concepts navigation arrays. Point Projects and Filtering to the new canonical page and remove remaining fixed-taxonomy language.

- [ ] **Step 3: Commit**

```bash
git add docs.json concepts/labels.mdx concepts/projects.mdx guides/filtering-and-search.mdx
git commit -m "docs: document project labels"
```

### Task 8: Add issue type documentation

**Files:**
- Modify: `docs.json`
- Create: `concepts/issue-types.mdx`
- Modify: `concepts/issues.mdx`

**Interfaces:**
- Consumes: seeded issue types, type permissions, creation UI, and per-type configuration.
- Produces: `/concepts/issue-types` as the canonical type reference and an updated issue overview.

- [ ] **Step 1: Write the issue type concept page**

Describe the starter catalog, custom types, icons/colors/descriptions, top-level versus sub-issue-only types, who can use restricted types, create/edit/delete rules, and the system-type deletion limitation.

- [ ] **Step 2: Register the page and reframe the issue page**

Add `concepts/issue-types` to both Core Concepts navigation arrays. Document type and workflow status as first-class fields, inline quick-add, creator/assignee/priority/dates/labels, and links to the focused workflow and label pages. Remove the universal fixed status lifecycle.

- [ ] **Step 3: Commit**

```bash
git add docs.json concepts/issue-types.mdx concepts/issues.mdx
git commit -m "docs: document issue types"
```

### Task 9: Add workflow guidance

**Files:**
- Modify: `docs.json`
- Create: `guides/workflows.mdx`
- Modify: `concepts/issues.mdx`
- Modify: `guides/views.mdx`

**Interfaces:**
- Consumes: per-type statuses, categories, transitions, initial status, ordering, and Board behavior.
- Produces: `/guides/workflows` as the canonical workflow task guide.

- [ ] **Step 1: Write the workflow guide**

Explain how an Admin or Owner manages one workflow per issue type, adds and reorders statuses, selects the initial status, configures allowed transitions, interprets broad categories, and resolves dead-end warnings.

- [ ] **Step 2: Register and link the guide**

Add `guides/workflows` to both Guides navigation arrays. Clarify that Board columns and available status changes follow the selected type's workflow rather than a global three-column model.

- [ ] **Step 3: Commit**

```bash
git add docs.json guides/workflows.mdx concepts/issues.mdx guides/views.mdx
git commit -m "docs: add workflow guide"
```

### Task 10: Explain hierarchy, fields, and templates

**Files:**
- Modify: `concepts/issue-types.mdx`
- Modify: `concepts/issues.mdx`
- Modify: `guides/rich-text-editing.mdx`

**Interfaces:**
- Consumes: allowed child types, issue tree, required built-in fields, custom field types, and type templates.
- Produces: complete user guidance for advanced type configuration.

- [ ] **Step 1: Add hierarchy instructions**

Cover parent-capable types, allowed child types, sub-issue-only types, creating sub-issues from an issue, breadcrumbs, collapsible List trees, and same-project/cycle constraints.

- [ ] **Step 2: Add fields and templates instructions**

Document supported custom field types, required values, select options and hints, plus reusable type templates for description, priority, and labels. Explain when defaults are applied without exposing service logic.

- [ ] **Step 3: Commit**

```bash
git add concepts/issue-types.mdx concepts/issues.mdx guides/rich-text-editing.mdx
git commit -m "docs: explain advanced issue types"
```

### Task 11: Replace role guidance and add member management

**Files:**
- Modify: `docs.json`
- Modify: `guides/roles-and-permissions.mdx`
- Create: `guides/members-and-roles.mdx`
- Modify: `guides/onboarding.mdx`
- Modify: `concepts/projects.mdx`
- Modify: `concepts/account.mdx`

**Interfaces:**
- Consumes: current membership, invitations, role tiers, custom roles, permissions, ownership transfer, and project access behavior.
- Produces: accurate conceptual and task-oriented access-control documentation.

- [ ] **Step 1: Rewrite the role model**

Explain Owner, Admin, Member, Viewer, custom roles, additive custom grants, immutable Owner guarantees, and own-versus-any comment permissions in user language.

- [ ] **Step 2: Write and register the member-management guide**

Cover choosing a project in Settings, inviting one or multiple email addresses, pending invitations, changing a base tier, assigning custom roles, removing members, transferring ownership, and permission-dependent controls. Add `guides/members-and-roles` to both Guides navigation arrays.

- [ ] **Step 3: Correct onboarding and account claims**

Remove the global first-account-is-admin framing where it conflicts with the project role model. Explain only behavior proven by current registration and onboarding code.

- [ ] **Step 4: Commit**

```bash
git add docs.json guides/roles-and-permissions.mdx guides/members-and-roles.mdx guides/onboarding.mdx concepts/projects.mdx concepts/account.mdx
git commit -m "docs: update roles and members"
```

### Task 12: Refresh Settings documentation

**Files:**
- Modify: `concepts/settings.mdx`

**Interfaces:**
- Consumes: `SETTINGS_TABS`, registered settings routes, and project-selector behavior.
- Produces: a complete current map of Account and Workspace settings.

- [ ] **Step 1: Rewrite navigation behavior**

Document per-tab paths, responsive navigation, and the project selector on project-scoped Workspace pages.

- [ ] **Step 2: Replace the availability tables**

Mark Preferences, Profile, Notifications, Security & access, Labels, Issue Types, Members, Roles & management, Integrations, and Automation available. Mark Export, Priorities, and Documents unavailable. Link each available feature page.

- [ ] **Step 3: Commit**

```bash
git add concepts/settings.mdx
git commit -m "docs: update settings reference"
```

### Task 13: Add integrations and Discord guidance

**Files:**
- Modify: `docs.json`
- Create: `guides/integrations.mdx`
- Modify: `concepts/projects.mdx`

**Interfaces:**
- Consumes: integration catalog, project selection, Discord webhook settings, and integration permissions.
- Produces: `/guides/integrations` as the entry point to all integrations.

- [ ] **Step 1: Write and register the integration overview**

Explain category filters, detail panels, project scope, access requirements, and the difference between available and coming-soon cards. Add `guides/integrations` to both Guides navigation arrays; Tasks 14 and 15 will add the focused Jira and GitHub links when their target pages are created.

- [ ] **Step 2: Document Discord setup**

Cover webhook URL entry, enabling/disabling, issue-activity and comment-activity options, and safe troubleshooting that does not reveal stored secrets.

- [ ] **Step 3: Commit**

```bash
git add docs.json guides/integrations.mdx concepts/projects.mdx
git commit -m "docs: add integrations guide"
```

### Task 14: Add Jira import guidance

**Files:**
- Modify: `docs.json`
- Create: `guides/jira-import.mdx`
- Modify: `guides/integrations.mdx`

**Interfaces:**
- Consumes: committed Jira connection, mappings, import options, progress UI, hierarchy, and external links.
- Produces: a complete task flow from credentials to repeat import.

- [ ] **Step 1: Write and register the Jira import guide**

Document prerequisites and credential fields shown by the UI, connection, mapping remote status/priority/label/type values, importing, progress and completion feedback, preserving parent-child relationships, external Jira links, and the synchronize-existing option. Add `guides/jira-import` to both Guides navigation arrays and link it from the integration overview.

- [ ] **Step 2: State repeat-import behavior carefully**

Explain visible deduplication and update behavior without promising unsupported bidirectional or continuous synchronization.

- [ ] **Step 3: Commit**

```bash
git add docs.json guides/jira-import.mdx guides/integrations.mdx
git commit -m "docs: add Jira import guide"
```

### Task 15: Add GitHub integration guidance

**Files:**
- Modify: `docs.json`
- Create: `guides/github-integration.mdx`
- Modify: `guides/integrations.mdx`
- Modify: `concepts/issues.mdx`

**Interfaces:**
- Consumes: committed Orbit and orbit-api behavior for App installation, repository management, issue linking, development actions, and status sync.
- Produces: one user guide for connecting GitHub and using the Development panel.

- [ ] **Step 1: Register the guide and document connection management**

Add `guides/github-integration` to both Guides navigation arrays and link it from the integration overview. Cover installing/connecting the GitHub App, connection health and retry states, synchronizing available repositories, adding/removing connected repositories, and disconnecting.

- [ ] **Step 2: Document issue development workflows**

Explain the hidden issue marker, automatic linked pull requests, multiple pull requests per issue, branch creation, pull-request creation with repository templates, and error feedback.

- [ ] **Step 3: Document synchronized status**

Describe open/closed/merged lifecycle, CI checks, and review badges. Avoid documenting the uncommitted standalone GitHub development permission.

- [ ] **Step 4: Cross-check committed code**

Run: `git -C ../orbit diff --quiet HEAD -- app/Services/Integrations/Github resources/js/Components/Organisms/IssueDevelopmentPanel || true`

Then use `git -C ../orbit show HEAD:<path>` for any claim whose current file is dirty.

- [ ] **Step 5: Commit**

```bash
git add docs.json guides/github-integration.mdx guides/integrations.mdx concepts/issues.mdx
git commit -m "docs: add GitHub integration guide"
```

### Task 16: Add automation guidance

**Files:**
- Modify: `docs.json`
- Create: `guides/automation.mdx`
- Modify: `guides/github-integration.mdx`

**Interfaces:**
- Consumes: committed trigger/action enums, rule builder, permissions, default GitHub rules, conditions, and execution constraints.
- Produces: a practical Automation guide linked from GitHub workflows.

- [ ] **Step 1: Write and register the rule-building flow**

Explain choosing a project, naming and enabling a rule, selecting one trigger, configuring available AND-combined conditions, adding ordered actions, saving, editing, disabling, and deleting. Add `guides/automation` to both Guides navigation arrays.

- [ ] **Step 2: List current triggers and actions**

Use user-facing labels from the committed enums. Explain the flat condition model, lack of OR/groups, replay protection in user terms, loop prevention outcome, and the default rules added when GitHub connects.

- [ ] **Step 3: Commit**

```bash
git add docs.json guides/automation.mdx guides/github-integration.mdx
git commit -m "docs: add automation guide"
```

### Task 17: Reconcile FAQ, glossary, and limitations

**Files:**
- Modify: `reference/faq.mdx`
- Modify: `reference/glossary.mdx`
- Modify: `reference/known-limitations.mdx`
- Modify: `concepts/saved-filters.mdx`

**Interfaces:**
- Consumes: every canonical page produced by Tasks 2–16.
- Produces: clean reference material with no resolved limitation presented as current.

- [ ] **Step 1: Remove resolved limitations**

Delete claims that roles do not gate actions, activity has no UI, mentions are unwired, settings are placeholders, and images cannot be inserted. Retain the lack of a public API, email-address changes, saved-filter editing and enforced context scoping, a distinct command palette or Mac-specific shortcuts, and push notifications. Add the current limits that most catalog integrations are coming soon and automation conditions are flat AND expressions without OR or nested groups.

- [ ] **Step 2: Expand the glossary**

Define issue type, workflow status, workflow category, sub-issue, custom field, template, project label, system role, custom role, integration, Development panel, and automation rule. Correct Activity and Settings definitions.

- [ ] **Step 3: Refresh FAQ answers and cross-links**

Replace obsolete role/activity answers with current task-oriented links. Add concise troubleshooting for Jira imports, GitHub connection health, and missing settings controls caused by project permissions.

- [ ] **Step 4: Run full documentation verification**

Run:

```bash
jq empty docs.json
git diff --check HEAD~17..HEAD
rg -n "three (interchangeable )?views|Open.*,.*In Progress.*,.*Closed|roles don't gate|no policy|not currently surfaced|no way to actually add an image|Integrations.*Soon|Issue mentioned.*not yet wired" --glob '*.mdx' .
```

Expected: JSON and diff checks pass; search results are either absent or explicitly describe a historical misconception rather than current behavior.

- [ ] **Step 5: Validate internal page targets**

Extract root-relative links from MDX, strip anchors, and confirm each resolves to an `.mdx` file or intentional external/app path. Confirm every path in both navigation arrays exists.

- [ ] **Step 6: Inspect repository boundaries and history**

Run:

```bash
git status --short
git log --format='%h %s' -20
git -C ../orbit status --short
git -C ../orbit-api status --short
git -C ../orbit-web status --short
```

Expected: `/docs` is clean after the final commit; other repositories contain no new changes caused by this work; recent docs commits use short Conventional Commit subjects and no co-author trailers.

- [ ] **Step 7: Commit**

```bash
git add reference/faq.mdx reference/glossary.mdx reference/known-limitations.mdx concepts/saved-filters.mdx
git commit -m "docs: reconcile user reference"
```
