---
name: "manage-staff"
description: "Adds, edits, or deletes staff profiles (coaches, instructors, bio) via Layer 3 tools. Use this to maintain your team list."
---

## Goal
To manage complex staff profiles and biographies using structured data and MCP tools while ensuring consistency across locales.

## When to Use
Trigger when the user requests to:
- "Add a new yoga instructor"
- "Update the bio of our master coach"
- "Create a staff card for the founder"
- "Delete the old bio of a resigned instructor"

## Workflow
1. **Prepare Data:** Formulate the structured JSON representing the staff profile (title, role, bio, image, email, order).
2. **I18n Consistency:** Ensure synchronization across all locales (`ru`, `en`, `es`, `pt-br`).
3. **MCP Tool Call:** Use appropriate tools (e.g., `get_staff`, `create_staff`, `update_staff`) to apply changes.
4. **Human-in-the-Loop:** 
   - **CRITICAL:** Before calling any delete tools (e.g., `delete_staff`), you MUST ask the user for explicit confirmation.
   - Summarize what will be deleted and why.

## Guardrails
- **STRICTLY FORBIDDEN** to use direct file system commands.
- Biographies should be grammatically correct across all 4 locales.
- Maintain structural parity across all 4 locales.

## Completion Checklist
- [ ] Data structured according to requirements (StaffSchema).
- [ ] User confirmation obtained for any destructive actions.
- [ ] MCP tools called for all locales.
