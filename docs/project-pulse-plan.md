# Project Pulse Implementation Plan

## Summary

Build Mona's Project Pulse as a lightweight static dashboard for contributors. It should make active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary easy to scan. Use the repository's static HTML/CSS/JSON approach without adding dependencies.

The `app/` directory is currently empty. `.vscode/launch.json` does not exist; `.vscode/tasks.json` is present and should remain untouched. The implementation must open the dashboard at `index.html` from the `app/` directory, not show a directory listing.

## Ordered Implementation Steps

### 1. Align on the design and data contract

**Designer owns `app/styles.css`.** Establish the information hierarchy, responsive layout, readable spacing, accessible contrast, and visible project cards. Include stable `.dashboard` and `.project-card` hooks, status badges, and clear priority treatment. Do not rely on color alone to convey status or risk.

**Coder owns `app/project-data.json` and `.vscode/launch.json`.** Populate a top-level `projects` array. Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a brief contributor-friendly `summary` as required by the brief. Use illustrative, internally consistent sample projects unless Mona supplies factual project data.

Create a strict JSON VS Code launch configuration named `Run Project Pulse Dashboard`. Serve with `python3 -m http.server 5500`, set the working directory to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`.

**Handoff:** Designer tells Coder the intended card structure and CSS hooks. Coder confirms the JSON field names and sample values. No additional JavaScript file or third-party dependency is planned.

### 2. Implement the dashboard page

**Coder owns `app/index.html`**, after the design and data contract are available. Use the exact title `Project Pulse`, reference `styles.css`, and load `project-data.json`. Render each project as a visible `.project-card`, including its name, owner, status, recent activity, priority, and summary.

Use semantic HTML and accessible labels/headings. Since the page fetches JSON, it must be previewed over HTTP rather than opened as a local `file:` URL. Provide a clear empty state for an empty projects array and a readable error state if data cannot be loaded. Insert data as text, not HTML, so project content is not interpreted as markup.

### 3. Integrate and verify

**Orchestrator owns integration and reporting; it does not implement the app.** Check that the HTML, CSS hooks, JSON shape, and launch settings agree. Have each specialist fix issues only in their assigned files. Ask Designer to review the rendered hierarchy and accessibility; ask Coder to address implementation or launch defects.

## File Assignments

- `app/styles.css` — **Designer:** responsive visual system and dashboard/card styling; include `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- `app/project-data.json` — **Coder:** valid JSON with a top-level `projects` array; each project includes the five required fields plus `summary`.
- `app/index.html` — **Coder:** semantic page titled `Project Pulse`, stylesheet and data references, `.project-card` markup, rendering of all project fields, empty/error states.
- `.vscode/launch.json` — **Coder:** strict JSON launch configuration named `Run Project Pulse Dashboard`; serve from `app/` and open `/index.html`.
- `docs/project-pulse-plan.md` — **Planner:** this plan; no application code.

## Dependencies and Work Ordering

The Designer's CSS work and Coder's data and launch configuration can run in parallel: they have separate file ownership, and the brief fixes the data schema and preview behavior.

The HTML implementation must follow those tasks because it depends on the agreed CSS hooks and the data field names. Integration and browser verification must follow all implementation work.

The Orchestrator should keep file scopes explicit. Do not assign overlapping edits to Designer and Coder. Route requested changes to the owner of the affected file, then repeat the relevant checks.

## Edge Cases and Risks

- Empty or unavailable project data should not leave a blank or broken dashboard.
- Missing or malformed project fields should not break rendering; avoid showing undefined values.
- Status and priority meaning must remain understandable without color.
- Responsive layout and long activity or summary text should not overflow cards.
- Browser fetch requires the HTTP preview; opening `index.html` directly from disk is not a valid data-loading test.
- Port `5500` may already be in use. If the launch fails for that reason, resolve the port conflict or choose a consistent alternate port and URL.
- No project-specific facts or visual brand guidance are present in the brief. Treat sample data as illustrative and keep styling aligned with a contributor dashboard.

## Validation Expectations

1. Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`; both must parse successfully.
2. Inspect `app/index.html` for the exact title, stylesheet and JSON references, `.project-card` markup, and visible name, owner, status, recent activity, priority, and summary fields.
3. Inspect `app/styles.css` for `.dashboard`, `.project-card`, card treatment, accessible contrast, and responsive behavior.
4. Start **Run Project Pulse Dashboard**. Confirm VS Code opens `http://localhost:<port>/index.html` and displays the dashboard rather than a directory listing.
5. In the browser, verify multiple cards, all project fields, narrow viewport layout, and the empty/error states.
6. Run `bash scripts/validate-exercise.sh` as a repository-level regression check. The Step 3 workflow also checks expected file/key phrases and JSON parsing, but it does not prove the app runs; browser verification is still required.
7. Orchestrator reports the checks performed, any limitations, and the successful launch result.

## Open Questions

- Are Mona's real project names, owners, activity, statuses, and priorities available? If not, use clearly illustrative sample entries.
- Is there brand guidance beyond the brief's request for a polished, readable dashboard? If not, Designer chooses a restrained, accessible visual direction.