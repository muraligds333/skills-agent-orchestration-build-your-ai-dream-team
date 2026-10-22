# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agents building Mona's Project Pulse dashboard.

- **Planner** uses **Claude Opus 5.5 (copilot)** to research the repository and produce an implementation plan, including tasks, dependencies, edge cases, and validation expectations. Definition: `.github/agents/planner.agent.md`.
- **Designer** uses **Gemini 3.7 Flash (copilot)** to shape the dashboard's user experience, accessibility, visual hierarchy, interactions, and responsive design. Definition: `.github/agents/designer.agent.md`.
- **Coder** uses **GPT-5.5 (copilot)** to implement assigned code and app support files, following project patterns and validating the changes. Definition: `.github/agents/coder.agent.md`.
- **Orchestrator** uses **Claude Opus 5.5 (copilot)** to coordinate the Planner, Designer, and Coder, assign file scopes, sequence or parallelize work, and verify the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
