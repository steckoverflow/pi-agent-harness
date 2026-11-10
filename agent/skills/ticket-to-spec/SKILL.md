---
name: ticket-to-spec
description: Turn a Jira issue into a repo spec file, or create a new Jira issue from an idea first. Use when the user says "ticket to spec", "spec this", "start DDM-123", or wants to begin work on a ticket or new idea.
argument-hint: "[JIRA-KEY] [idea text]"
---

# Ticket to Spec

Output: `specs/<KEY>.md` in the current repo, linked to a Jira issue. `build-loop` and `code-review` read it from there.

Jira tools are MCP tools: call them from codemode as `tools.mcp__jira__<name>`. Allowed projects: `DDM`, `IDP`.

## 1. Get the issue

**Key supplied** (e.g. `DDM-123`): fetch it with `jira_get_issue`. Read the summary, description, acceptance criteria, comments, linked issues, and parent epic. If `specs/<KEY>.md` already exists, update it rather than starting over.

**No key supplied:** a new issue is needed.

1. Ask for the idea if the user gave none.
2. Run step 2 (grilling) first, so the ticket is born with clear acceptance criteria.
3. Draft the issue: project (`DDM` or `IDP`), issue type (check `jira_get_project_issue_types`), summary, description, and acceptance criteria. Suggest an epic only if the user names one.
4. Show the draft and **ask for confirmation before calling `jira_create_issue`**. Creating a ticket is visible to the team.
5. Use the returned key from here on.

## 2. Close the gaps

Load the `grilling` skill and run it against the ticket. Ask only about what the ticket leaves open: scope edges, acceptance criteria, error cases, non-goals. Look up facts in the repo yourself; never ask the user for them. Skip grilling if the ticket is already unambiguous, and say so.

## 3. Write the spec

Write `specs/<KEY>.md`:

```markdown
# <KEY>: <summary>

Jira: <issue URL>

## Goal
<one paragraph: what and why>

## Acceptance criteria
- [ ] <observable, testable behaviour>

## Non-goals
- <explicitly out of scope>

## Decisions
- <decision>: <reason>

## Open questions
- <anything still unresolved, or "none">
```

Every acceptance criterion must be testable: `build-loop` turns each one into a failing test.

## 4. Sync back (ask first)

If grilling changed or added acceptance criteria on an existing ticket, offer to post them as a Jira comment with `jira_add_comment`. Do not edit the ticket description unless the user asks.

Redact secrets from anything you write to Jira or the spec.
