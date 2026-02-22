---
name: create-jira-epic
version: 1.0.0
user-invocable: false
description: |
  Create a well-structured Jira Epic with consistent formatting optimized for both human readers and AI agents.
  Invoked internally by the /jira-create command. Handles clarifying questions, optional codebase investigation, and MCP-based ticket creation.
---

# Create Jira Epic

## Epic Description Format

Every epic MUST use this exact structure. Do not deviate.

```markdown
## Background
[One paragraph. Business context, strategic driver, or user need that motivates this body of work.]

## Goals
* [Concrete goal or outcome this epic aims to achieve]
* [Another goal]

## Acceptance Criteria
* [High-level, testable condition that signals the epic is complete]
* [Another condition]

## Out of Scope
[Only if the user explicitly calls out exclusions. Omit this section entirely otherwise.]

## Additional Information / Links
* [Designs, documentation, dependencies, related tickets or initiatives]
```

### Rules

- **Background**: One paragraph. Describe the business or product context. Reference specific systems or domains when it helps scope the work. Do not summarise the goals here -- that is what the Goals section is for.
- **Goals**: 2-5 bullet points. Each goal describes an intended outcome, not a task ("Reduce p99 checkout latency below 300ms", not "Optimise checkout service"). If there is only one goal, use a single bullet.
- **Acceptance Criteria**: 2-6 high-level, testable conditions that signal the epic is complete. These are coarser than story-level criteria -- they describe the epic's delivered state, not individual story outcomes.
- **Out of Scope**: Only present when the user explicitly provides exclusions. Never add speculatively.
- **Additional Information / Links**: Only include when there is genuinely useful context. Never duplicate information already present in other Jira fields. Omit if empty.
- If any section lacks information, use AskUser to prompt the user rather than leaving placeholders.
- Clear, concise, professional tone throughout.
- **Always use Markdown formatting.** The MCP tools convert Markdown to ADF internally. Never use Jira wiki markup (`h2.`, `{{code}}`, `{code}`, `#` for numbered lists). Use `## Heading`, `` `code` ``, triple-backtick fenced code blocks, and `1.` for numbered lists.

### Summary Line

- Concise noun phrase or action phrase describing the body of work.
- Not a commit message -- no `feat:` prefix.
- Examples: "Checkout performance optimisation", "Migrate authentication service to OAuth 2.0"

## Workflow

### 1. Parse the Request

Extract from the user's message or the context passed from `/jira-create`:
- The strategic or product driver (for Background)
- Desired outcomes (for Goals)
- High-level completion conditions (for Acceptance Criteria)
- Parent initiative (if mentioned)
- Explicit exclusions (for Out of Scope)
- Team preference (if mentioned)
- Any links or references

### 2. Ask Clarifying Questions

Use AskUser when genuinely ambiguous. Common questions:

- **Missing goals**: "What specific outcomes should this epic achieve?"
- **Scope ambiguity**: "This sounds like it spans multiple epics. Should we scope it down, or keep it broad?"
- **Missing parent**: "Should this epic live under a specific initiative, or stand alone?"
- **Vague criteria**: "How will we know this epic is done? What is the measurable outcome?"
- **Missing team**: "Which team should own this epic?" (only if team cannot be inferred from the parent -- see step 6)

Do NOT ask about format -- the format is fixed. Do NOT ask questions you can answer by investigating the codebase.

### 3. Investigate the Codebase (When Applicable)

When the epic involves technical work in the current repo and codebase analysis was requested in `/jira-create`:

- Search relevant source files, services, config, and architectural boundaries to understand current state.
- Use findings to write an accurate Background paragraph and Goals.
- Add relevant file paths, service names, or architecture notes to Additional Information if they would help the implementer.

Skip this step for product or business-level epics, or when the user's repo is unrelated.

### 4. Draft the Description

Write the description following the mandatory format. Before creating, review:

- Is Background one paragraph focused on context and driver?
- Are Goals outcome-oriented, not task-oriented?
- Are Acceptance Criteria high-level and testable?
- Is Out of Scope only present if the user explicitly said something is excluded?

### 5. Create the Ticket

Use `atlassian___createJiraIssue` via the `manage-jira` skill for API mechanics:

- `issueTypeName`: Always `"Epic"`
- `parent`: Set when the user provides a parent initiative
- `projectKey`: Derive from the parent ticket's project, or ask the user

### 6. Set Team

Every ticket **must** have a team assigned to appear on the board. Determine the team immediately after creating the ticket:

1. **If a parent initiative was provided**, fetch it with `atlassian___getJiraIssue` and read the team field (`customfield_11100`).
   - If the parent has a team, use **AskUser** to offer:
     - *"Use parent's team: \<Team Name\>"*
     - *"Specify a different team"*
   - If the parent has no team, ask the user: *"Which team should own this epic?"*
2. **If no parent was provided**, ask the user: *"Which team should own this epic?"*
3. **Set the team** on the newly created ticket via `atlassian___editJiraIssue`:
   ```
   atlassian___editJiraIssue(cloudId, issueIdOrKey: "PROJ-456", fields: {"customfield_11100": "<team-uuid>"})
   ```
   To resolve a team name to its UUID, look at the `customfield_11100` value on an existing ticket that belongs to that team (see `manage-jira` skill for details).

## What This Skill Does NOT Cover

- Other issue types (Initiative, Story, Bug) -- separate skills.
- Transitioning, editing, or commenting on existing tickets -- use `manage-jira` skill.
- Sprint or other custom field assignment -- use `manage-jira` skill.
