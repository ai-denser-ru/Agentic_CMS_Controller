---
name: "manage-nodes"
description: "Adds, edits, or deletes catalog nodes (menu items, services) via Layer 3 tools. Use this to update the catalog, prices, or add new items."
---

## Goal
To manage atomic catalog entities (nodes) using structured data and MCP tools while maintaining structure across locales.

## When to Use
Trigger when the user requests to:
- "Add a new coffee to the menu"
- "Update the price of the yoga class"
- "Create a new news post"
- "Delete an old service item"

## Workflow
1. **Prepare Data:** formulate the structured JSON for the node (fields like `price`, `type`, `taxonomies`).
2. **I18n Consistency:** Apply changes to all locales (`ru`, `en`, `es`, `pt-br`) simultaneously.
3. **MCP Tool Call:** Use appropriate tools (e.g., `get_node`, \`update_node\`) to apply changes.
4. **Human-in-the-Loop:** 
   - **CRITICAL:** Request explicit user confirmation before any deletion (e.g., `delete_node`).
   - Clearly state which node is being targeted.

## Guardrails
- **STRICTLY FORBIDDEN** to use direct file system commands.
- No automatic currency conversion unless requested.

## Completion Checklist
- [ ] Node data structured correctly.
- [ ] User confirmation obtained for deletions.
- [ ] MCP tools called for all locales.
