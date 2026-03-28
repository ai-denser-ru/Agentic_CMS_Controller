---
name: "manage-taxonomy"
description: "Adds, edits, or deletes categories, tags, or levels via Layer 3 tools. Use this to classify news, classes, or products."
---

## Goal
To manage taxonomic classification for content entities (news, classes, etc.) while ensuring consistency across locales.

## When to Use
Trigger when the user requests to:
- "Add a new 'Yoga' category"
- "Create a 'Beginner' level tag"
- "Update the weight of the 'News' category"
- "Delete an unused tag"

## Workflow
1. **Prepare Data:** Formulate the structured JSON representing the term (title, vocabulary, weight).
2. **I18n Consistency:** Ensure synchronization across all locales (`ru`, `en`, `es`, `pt-br`).
3. **MCP Tool Call:** Use appropriate tools (e.g., `get_term`, `create_term`, `update_term`) to apply changes.
4. **Human-in-the-Loop:** 
   - **CRITICAL:** Before calling any delete tools (e.g., `delete_term`), you MUST ask the user for explicit confirmation.
   - Summarize what will be deleted and why.

## Guardrails
- **STRICTLY FORBIDDEN** to use direct file system commands.
- Ensure `vocabulary` is one of `category`, `level`, `tag`.
- Maintain structural parity across all 4 locales.

## Completion Checklist
- [ ] Data structured according to requirements (TaxonomySchema).
- [ ] User confirmation obtained for any destructive actions.
- [ ] MCP tools called for all locales.
