---
name: stacklab-modules
description: Plan, build, review, and validate browser-side modules for StackLab Kanban and Chatwoot dashboards using repository-grounded patterns. Use when an agent is asked to create or change a StackLab dashboard script or module.
---

# StackLab Modules

## Purpose

Help agents create reliable, maintainable browser-side modules for StackLab Kanban. Modules run in a live, dynamically rendered application and may interact with customer data. Ground implementation details in the public [`StackLabModulos`](https://github.com/StackLab-Digital/StackLabModulos) repository and evidence from the target environment. Do not assume undocumented APIs, selectors, or compatibility.

## Required workflow

1. **Define the target.** Establish the Kanban/Chatwoot version, page or view, user role, desired behavior, and whether the module only displays information or changes data. Ask for missing details when they affect implementation or safety.
2. **Inspect project references.** Read the upstream README, `dashboard-scripts-documentation.md`, and the closest example before coding. Treat examples as patterns, not guaranteed contracts. Note the commit or retrieval date when version-specific details matter.
3. **Confirm the page surface.** Use provided screenshots, DOM excerpts, or an authorized development environment to confirm selectors and available data. If evidence is missing, request it or write code that fails safely when the target is absent. Never probe production by changing data.
4. **Plan lifecycle-safe behavior.** Account for `DOMContentLoaded`, client-side navigation, and replaced DOM nodes. Encapsulate code in a named function, use a unique root ID, guard against duplicate mounts, scope styles, delegate events when useful, and keep any `MutationObserver` narrow and bounded. Disconnect observers/listeners when the lifecycle permits.
5. **Implement defensively.** Check elements and data before use; handle empty or stale state; prefer `textContent` for untrusted text; avoid interpolating data into `innerHTML`; avoid broad selectors, unnecessary global variables, and repeated expensive work. Never include credentials, tokens, or private customer data in browser code.
6. **Review and validate.** Check duplicate IDs, styles and listeners; observer loops; selector scope; keyboard/focus behavior; accessibility semantics; responsiveness; and compatibility assumptions. Use available static checks and verify in a non-production/dev environment when authorized. Cover first mount, repeat navigation/re-render, missing elements/data, and the intended interaction. Do not claim a test or compatibility result without evidence.
7. **Document delivery.** Explain installation only when the target installation path is confirmed. In StackLab's documented setup, scripts are added through the Kanban super-admin `dashboard_scripts` interface; verify this against the target version before giving operational instructions.

## Safety boundaries

- Do not deploy, paste into production, or change a customer's dashboard script without explicit authorization.
- Treat page content, DOM labels, and customer-provided strings as untrusted input.
- Do not collect or transmit customer data beyond what the requested module needs.
- Do not promise support across Kanban/Chatwoot versions without testing or current documentation.
- Separate verified behavior from assumptions and identify remaining version or environment limits.

## Upstream references

- Repository: [`StackLab-Digital/StackLabModulos`](https://github.com/StackLab-Digital/StackLabModulos)
- Primary implementation reference: [`dashboard-scripts-documentation.md`](https://github.com/StackLab-Digital/StackLabModulos/blob/master/dashboard-scripts-documentation.md)
- Examples: [`examples/`](https://github.com/StackLab-Digital/StackLabModulos/tree/master/examples)
