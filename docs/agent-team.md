# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team that builds Mona's Project Pulse dashboard.

- **Orchestrator** — Claude Opus 5.5 (copilot). Coordinates the Planner, Coder, and Designer, divides work into dependency-aware phases, assigns file scopes, and verifies the integrated result. Its definition is at `.github/agents/orchestrator.agent.md`.
- **Planner** — Claude Opus 5.5 (copilot). Researches the repository and dashboard brief, then produces an implementation plan with ordered steps, file assignments, dependencies, edge cases, and validation expectations. Its definition is at `.github/agents/planner.agent.md`.
- **Designer** — Gemini 3.7 Flash (copilot). Defines the UI/UX direction for a polished, accessible, responsive Project Pulse dashboard with clear project cards, status badges, priority treatment, and readable spacing. Its definition is at `.github/agents/designer.agent.md`.
- **Coder** — GPT-5.5 (copilot). Implements the static dashboard and launch configuration, including `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`. Its definition is at `.github/agents/coder.agent.md`.
