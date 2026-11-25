# Project Pulse implementation plan

## Summary

Build a lightweight, static Project Pulse dashboard for Mona's contributors. It should make active projects, owners, status, recent activity, priority or risk, and a concise contributor-friendly summary easy to scan. Use the repository brief and existing agent file-scope rules; keep the app dependency-free unless repository inspection shows a necessary existing tool.

## Ordered steps and file assignments

1. **Confirm the implementation contract — Orchestrator**
   - Agree on the dashboard content hierarchy, project-card structure, CSS hooks, and JSON field names before parallel implementation.
   - Use a top-level `projects` array. Every item must include `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short `summary` to meet the contributor-friendly summary requirement.
   - Assign disjoint file ownership as below and communicate the agreed hooks and fields to both specialists.
   - **Files:** none.

2. **Create the visual system — Designer**
   - Define the responsive dashboard layout and implement accessible styling for project cards, status badges, priority/risk, typography, spacing, and focus/contrast states.
   - Provide stable selectors, including `.dashboard` and `.project-card`, for the Coder's markup.
   - **Files:** `app/styles.css` only.

3. **Prepare the project data and runnable configuration — Coder**
   - Create realistic sample projects using the agreed schema, with clear and consistent status and priority values.
   - Configure the VS Code **Run Project Pulse Dashboard** launch configuration to serve from `${workspaceFolder}/app` and open `index.html` directly, not a directory listing. Prefer an already available static server; avoid adding dependencies unless needed.
   - **Files:** `app/project-data.json`, `.vscode/launch.json`.

4. **Build the page and integrate — Coder**
   - Implement semantic page structure and render project cards from `project-data.json`, using the agreed selectors and field names. Keep the rendering accessible and handle loading, empty, and fetch-error states.
   - Integrate the Designer's stylesheet and ensure the launch URL opens the dashboard.
   - **Files:** `app/index.html`.
   - **Dependency:** Steps 2 and 3 must finish first so markup matches the stylesheet hooks and data schema. The Orchestrator owns any integration coordination, not edits to these files.

5. **Verify the integrated dashboard — Orchestrator with Coder and Designer**
   - Run the focused checks below, resolve integration defects within the original file assignments, and confirm the launch flow displays the page.
   - **Files:** no additional files unless a defect requires a scoped correction.

## Dependencies and parallel work

- Step 1 precedes implementation: the Orchestrator establishes the shared markup/data contract and explicit file ownership.
- After that contract is agreed, **Steps 2 and 3 can run in parallel** because Designer owns only `app/styles.css`, while Coder owns `app/project-data.json` and `.vscode/launch.json`.
- **Step 4 is sequential after both Steps 2 and 3**: the page must use the final CSS hooks and data schema. Keep `app/index.html` assigned to Coder and do not have Designer edit it.
- Step 5 follows integration. If changes are needed, assign them to the agent responsible for the affected file; avoid overlapping edits.

## Responsibilities

- **Designer:** own `app/styles.css`; deliver a polished, responsive, accessible visual hierarchy and stable selectors that match the agreed component contract.
- **Coder:** own `app/project-data.json`, `.vscode/launch.json`, and `app/index.html`; build the data-driven page, handle user-visible loading/error/empty states, and make the launch configuration reliably serve and open the dashboard.
- **Orchestrator:** define phases and dependencies, keep file scopes separate, coordinate integration, and verify the completed result.

## Edge cases and decisions

- Fetch the JSON through the configured local server rather than relying on `file://` access; the launch configuration must open `/index.html` (or its equivalent) so users see the dashboard rather than a server index.
- Handle an empty `projects` array and a failed JSON request visibly; do not silently substitute sample data at runtime.
- Keep status and priority values consistent across the sample data and badge styling. Do not rely on color alone to communicate either.
- Use semantic headings and readable text, responsive layout, sufficient contrast, and keyboard-visible focus.
- No existing `app/` or launch configuration is present. Reuse an available static-serving tool if possible; if none is available, surface that constraint before introducing a dependency.

## Validation expectations

- Parse `app/project-data.json` and `.vscode/launch.json` as strict JSON.
- Confirm each project has the required fields and a contributor-friendly summary.
- Start the dashboard using **Run Project Pulse Dashboard** and verify it serves from `app/` and opens `index.html`, not a directory listing.
- In the rendered page, verify project names, owners, status, recent activity, priority, and summaries come from the JSON; check the empty/error states and responsive layout.
- Check keyboard navigation, semantic structure, text contrast, and that status/priority remain understandable without color.
- There is no existing app or test harness in the repository; use focused manual browser checks and available command-line JSON validation rather than adding a test framework.
