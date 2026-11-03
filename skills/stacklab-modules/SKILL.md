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

## Distilled platform guidance

The upstream guide describes Dashboard Scripts as HTML/JavaScript snippets injected near the end of `<body>`. They can affect page appearance and behavior without changing the application source. Treat them as privileged page code: they execute in the signed-in dashboard context, so keep permissions, data access, and side effects narrow.

### Startup and dynamic pages

- `DOMContentLoaded` alone may be too early for the app UI. The guide's pattern waits until the Vue app is present before initializing. Prefer a bounded readiness check with timeout, cancellation, and a clear failure path; do not leave an unbounded polling interval running.
- The interface is dynamic and can replace nodes. Apply once to existing elements, then observe only the smallest relevant container for child-list changes. Make each mount idempotent and avoid reprocessing the entire document on every mutation.
- The guide's examples use a body-wide observer to discover late-rendered UI. Use that only as a short-lived discovery mechanism, disconnect after finding the target, then observe the target container if continued updates are needed.

### UI areas and integration techniques

The guide names these areas as useful places to investigate: `Sidebar`, `ChatList`, `ConversationItem`, `KanbanItem`, `KanbanHeader`, `KanbanColumn`, and `KanbanActions`. Its examples cover card styling, header actions, sidebar entries, conversation priority markers, keyboard shortcuts, and development diagnostics. These names and example selectors describe the documented implementation, not a stable public component API; inspect the live target before relying on them.

Use DOM attributes or visible UI state when they provide the required data. The guide also demonstrates reaching Vue/Vuex through `window.__vue__`, `$store`, getters, dispatch, and subscriptions. These are internal implementation details, can differ between versions, and can expose or mutate substantial application state. Use them only when the target version confirms the interface and the requested task needs it; prefer read-only access, never log customer records, and verify every getter/action against current source or live evidence. Do not assume example names such as `kanban/getKanbanItems`, `kanban/updateItemStatus`, or `getCurrentUser` exist in a deployed version.

When adding controls, guard against duplicates, attach handlers once, and use the app's visual conventions only after confirming their classes are present. For CSV export, serialize fields with proper CSV quoting/escaping, use a Blob/object URL, and revoke the URL after download; do not use the guide's abbreviated comma-join as production-safe serialization.

### Diagnostics and side effects

The guide suggests checking the DOM and Vue store during development. Keep diagnostics opt-in and temporary: no always-on debug panel, broad click capture, full-store dumps, or console output containing personal/customer data. Keyboard shortcuts must avoid inputs, textareas, contenteditable regions, and collisions with existing shortcuts. Any action that creates or updates records needs explicit user intent, confirmation where consequential, and validation in a development environment before production use.

## Safety boundaries

- Do not deploy, paste into production, or change a customer's dashboard script without explicit authorization.
- Treat page content, DOM labels, and customer-provided strings as untrusted input.
- Do not collect or transmit customer data beyond what the requested module needs.
- Do not promise support across Kanban/Chatwoot versions without testing or current documentation.
- Separate verified behavior from assumptions and identify remaining version or environment limits.

## Upstream references

- Repository: [`StackLab-Digital/StackLabModulos`](https://github.com/StackLab-Digital/StackLabModulos)
- Primary implementation reference (source reviewed at commit `1e09296548052cc3e6f7a0f851ba21f28765dcf6`): [`dashboard-scripts-documentation.md`](https://github.com/StackLab-Digital/StackLabModulos/blob/1e09296548052cc3e6f7a0f851ba21f28765dcf6/dashboard-scripts-documentation.md)
- Examples: [`examples/`](https://github.com/StackLab-Digital/StackLabModulos/tree/master/examples)
