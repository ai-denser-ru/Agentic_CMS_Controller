---
name: "manage-pages"
description: "Adds, edits, or deletes structural pages (About Us, Contacts) via Layer 3 tools. Use this to change the site's layout or update general content on landing pages."
---

## Goal
To manage complex, structural entities (pages) using structured data and MCP tools while ensuring consistency across locales.

## When to Use
Trigger when the user requests to:
- "Update the About Us section"
- "Create a new Landing Page for a promotion"
- "Add a Contacts page"
- "Delete the old Privacy Policy"

## Workflow
1. **Prepare Data:** Formulate the structured JSON representing the page content and metadata.
2. **I18n Consistency:** Ensure synchronization across all locales (`ru`, `en`, `es`, \`pt-br\`).
3. **MCP Tool Call:** Use appropriate tools (e.g., `get_page`, `update_page`) to apply changes.
4. **Human-in-the-Loop:** 
   - **CRITICAL:** Before calling any delete tools (e.g., `delete_page`), you MUST ask the user for explicit confirmation.
   - Summarize what will be deleted and why.

## Guardrails
- **STRICTLY FORBIDDEN** to use direct file system commands.
- Maintain structural parity across all 4 locales.

## Completion Checklist
- [ ] Data structured according to requirements.
- [ ] User confirmation obtained for any destructive actions.
- [ ] MCP tools called for all locales.
