# Antigravity Compatibility Updates for Herdr Multi-Agent Development

## 1. Path Resolution Updates for Progressive Disclosure
- **Problem**: In Antigravity, when a skill attempts to read local file paths inside its own folder, a raw relative path (like `references/discovery-and-direction.md`) will evaluate against the workspace root (CWD), resulting in "File Not Found".
- **Solution**: Replaced raw relative paths with Markdown relative links (e.g. `[discovery-and-direction.md](./references/discovery-and-direction.md)`). According to Antigravity's Customization System Guide, this allows the agent and the IDE to properly resolve file locations within the skill's directory structure while maintaining compatibility with Antigravity's progressive disclosure.

## 2. Tool and Subagent Invocation Equivalences
- **Problem**: The skill previously assumed it was running in an environment (like Claude) that had explicit tool bindings for satellite skills (like `grill-me`, `wayfinder`, `scout`).
- **Solution**: Rewrote the workflow to rely on native Antigravity mechanisms:
  - Invoking subagents equipped with skills via `invoke_subagent` (e.g., orchestrating an interactive interview using the `grill-with-docs` skill).
  - Explicitly using standard read/web tools instead of assuming external `scout` or `wayfinder` tools.
  - Furthermore, explicitly instructed the process to **never limit the number of questions** (e.g. 3 or 4 limit) when grilling the user, ensuring deep and complete business clarification as requested by the user.
