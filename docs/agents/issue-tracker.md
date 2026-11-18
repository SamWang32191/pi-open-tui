# Issue tracker: GitHub

Read and publish issues and specs in `SamWang32191/pi-open-tui`. Pass
`--repo SamWang32191/pi-open-tui` to the `gh issue` and `gh pr` commands below
so they target the fork even when a checkout defaults to upstream.

## Issue operations

- **Publish:** `gh issue create --title "..." --body-file -`. Pass Markdown on stdin to preserve line breaks.
- **Fetch:** `gh issue view <n> --json number,title,body,labels,comments,author,createdAt,updatedAt`.
- **List:** `gh issue list --state open --json number,title,body,labels,comments`; adjust `--state` and `--label` as needed.
- **Sub-issues:** Use `gh issue create --parent <parent>` or `gh issue edit <parent> --add-sub-issue <child>`. If native sub-issues are unavailable, put `Part of #<parent>` at the top of the child body.
- **Dependencies:** Use `gh issue create --blocked-by <blocker>` or `gh issue edit <child> --add-blocked-by <blocker>`. Count open blockers with `gh issue view <child> --json blockedBy --jq '[.blockedBy.nodes[] | select(.state == "OPEN")] | length'`. If native dependencies are unavailable, put `Blocked by: #<n>, #<n>` at the top of the child body. Work can start only when all blockers are closed.
- **Comment and label:** Use `gh issue comment <n> --body "..."` and `gh issue edit <n> --add-label "..."` or `--remove-label "..."`.
- **Close:** Post the reason as a comment, then run `gh issue close <n>` if the issue is still open.

## Pull requests as a triage surface

**PRs as a request surface: no.**

If enabled, discover open PRs whose authors are not owners, members, or
collaborators. Read the body, comments, and diff with `gh pr view`,
`gh pr view --comments`, and `gh pr diff`. Apply the same triage states with
`gh pr comment`, `gh pr edit`, and `gh pr close`.

Issues and PRs share a number space. To resolve a bare `#42`, try
`gh pr view 42`, then `gh issue view 42`.

## Wayfinding

- **Map:** Use one issue labeled `wayfinder:map` for Notes, Decisions-so-far, and Fog.
- **Tickets:** Link tickets as sub-issues of the map. Label each `wayfinder:<type>` (`research`, `prototype`, `grilling`, or `task`). If native sub-issues are unavailable, list the tickets in the map's checklist.
- **Pick:** Take the first open, unassigned ticket in map order with no open blockers.
- **Claim:** Run `gh issue edit <n> --add-assignee @me` before other writes in the session.
- **Resolve:** Comment with the answer, close the ticket, and add a short summary and link to Decisions-so-far.
