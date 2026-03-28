---
name: "manage-blocks"
description: "Adds, edits, or deletes content blocks (Promo, Testimonials, CTA) via Layer 3 tools. Use this to change the site's interactive elements and promotional sections."
---

## Goal
To manage reusable content blocks (promo, testimonial, cta) using structured data and MCP tools while ensuring consistency across locales.

## When to Use
Trigger when the user requests to:
- "Add a new testimonial from a customer"
- "Update the main promo banner on the landing page"
- "Create a Call-to-Action block for the footer"
- "Deactivate an old promotional banner"

## Workflow
1. **Prepare Data:** Formulate the structured JSON representing the block content and metadata (block_type, placement, weight, active).
2. **I18n Consistency:** Ensure synchronization across all locales (`ru`, `en`, `es`, `pt-br`).
3. **MCP Tool Call:** Use appropriate tools (e.g., `get_block`, `create_block`, `update_block`) to apply changes.
4. **Human-in-the-Loop:** 
   - **CRITICAL:** Before calling any delete tools (e.g., `delete_block`), you MUST ask the user for explicit confirmation.
   - Summarize what will be deleted and why.

## Guardrails
- **STRICTLY FORBIDDEN** to use direct file system commands.
- Maintain structural parity across all 4 locales.
- Ensure `block_type` is one of `promo`, `testimonial`, `cta`.

## Completion Checklist
- [ ] Data structured according to requirements (BlockSchema).
- [ ] User confirmation obtained for any destructive actions.
- [ ] MCP tools called for all locales.
