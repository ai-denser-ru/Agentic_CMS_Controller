---
name: "manage-nodes"
description: "Adds, edits, or deletes catalog nodes (menu items, services, yoga classes) in the Agentic CMS. Use this when a user asks to update the catalog, prices, or add new items to the menu/services list."
allowed-tools: ReadWrite, Glob, Grep, Git
---

## Goal
To manage atomic catalog entities (nodes) within the Astro-based Agentic CMS while strictly maintaining structure across all supported locales.

## When to Use
Trigger this skill whenever the user requests to:
- "Add a new coffee to the menu"
- "Update the price of the yoga class"
- "Create a new news post"
- "Delete an old service item"
- Targets `src/content/nodes/` collection.

## Workflow
1. **Identify the Intent:** Determine the node type (e.g., `menu_item`, `class`, `news`).
2. **Read the Schema:** Read `src/content/config.ts` to ensure fields like `price`, `type`, and `taxonomies` are correct.
3. **I18n Manifest Preparation:** Before editing, list the paths to be modified in **ALL** locales (`ru`, `en`, `es`, `pt-br`).
   - If a locale is missing from the request, you MUST include it anyway with default values or placeholders.
4. **Synchronized Modification:**
   - **Create/Update:** Apply the change to `.md` files in **ALL** 4 locale folders simultaneously.
   - **Rich Content Required**: You MUST write a detailed description and promotional text in the Markdown BODY (below the `---` frontmatter). The frontmatter `description` field is for meta-tags/previews only; the actual page content depends on the body.
   - **Cleanup**: If this is the FIRST real content entry in a collection, you MUST DELETE the `placeholder.md` file in that locale folder to maintain cleanliness.
   - **Fields Parity**: If a new frontmatter field is added (e.g., `calories`), it MUST be added to all 4 files, even if the value is null or a placeholder.
    - **Translation Strategy**: NIKGDA (NEVER) use automatic AI translation without explicit request. If a translation is missing, you MUST copy the content from the source language, add a note `<!-- TODO: Translate from [Source Language] -->` at the top, and REQUEST the translation from the user in your final message.
5. **Verify Structure & Audit:**
   - Ensure frontmatter strictly matches the Zod schema.
   - **AUDIT**: Run `ls -R src/content/nodes/` to verify that all 4 files exist and were updated.
6. **Commit and Push:** Stage changes for all locales and commit with a semantic message (e.g., `content(nodes): add espresso to menu across 4 locales`).

## Guardrails
- **CRITICAL:** **FORBIDDEN** to create or update a node in only one locale. If it exists in `ru`, it MUST exist in `en`, `es`, and `pt-br` to prevent routing errors.
- **Proactive Inquiry:** If you are unsure about the price or category, ask the user.
- **No-Guessing Rule**: Do not convert currencies. Use the same price string across all locales unless specified.

## Completion Checklist
- [ ] Node file is present in ALL active locale folders.
- [ ] Frontmatter matches `config.ts`.
- [ ] Changes committed and pushed to the remote repository.
