---
name: "manage-pages"
description: "Adds, edits, or deletes structural pages (About Us, Contacts, Privacy Policy) in the Agentic CMS. Use this when a user asks to change the site's layout, update general content on the landing pages, or create special informational sections."
allowed-tools: ReadWrite, Glob, Grep, Git
---

## Goal
To manage complex, structural entities (pages) within the Astro-based Agentic CMS while strictly maintaining consistency across all supported locales.

## When to Use
Trigger this skill whenever the user requests to:
- "Update the About Us section"
- "Create a new Landing Page for a promotion"
- "Add a Contacts page"
- "Delete the old Privacy Policy"
- Targets `src/content/pages/` collection.

## Workflow
1. **Identify the Intent:** Determine which page needs structural updates (e.g., `about`, `contacts`).
2. **Read the Schema:** Read `src/content/config.ts` to check `layout` and `title` fields.
3. **I18n Manifest Preparation:** List the existing files across all locales: `ru`, `en`, `es`, `pt-br`.
   - If a file is missing in one locale, you MUST create it during this workflow to ensure parity.
4. **Synchronized Modification:**
   - **Update:** If the user wants to update the page content, you MUST update the file in **ALL** 4 locales.
   - **Rich Content Required**: You MUST write a detailed landing page content in the Markdown BODY (below the `---` frontmatter). The frontmatter `description` field is for meta-tags only. Use headings (`#`, `##`), lists, and formatted text.
   - **Cleanup**: If you are replacing a `placeholder.md` with real content, ensure the placeholder is deleted or overwritten.
   - **Structural Parity**: If you change a common field or add a component, you MUST apply this change to all 4 files, even if the text content remains untranslated.
    - **Translation Strategy**: NIKGDA (NEVER) use automatic AI translation without explicit request. If a translation is missing, you MUST copy the content from the source language, add a note `<!-- TODO: Translate from [Source Language] -->` at the top, and REQUEST the translation from the user in your final message.
5. **Verify Structure & Audit:**
   - Ensure frontmatter strictly matches the Zod schema.
   - **AUDIT**: Run `ls -R src/content/pages/` to verify that all 4 files exist and were updated.
6. **Commit and Push:** Stage changes for all locales and commit with a semantic message (e.g., `content(pages): update about us across 4 locales`).

## Guardrails
- **CRITICAL:** **FORBIDDEN** to edit a page in only one locale. If 'about.md' is modified in Russian, its English, Spanish, and Portuguese counterparts MUST be updated too.
- **Proactive Inquiry:** If you are unsure about the layout or specific fields, ask the user.
- **No-Guessing Rule**: If you cannot translate a complex section, use the source text as a placeholder and add a comment `<!-- TODO: Translate to [locale] -->`.

## Completion Checklist
- [ ] Page file is updated/present in ALL active locale folders.
- [ ] Frontmatter matches `config.ts`.
- [ ] Changes committed and pushed to the remote repository.
