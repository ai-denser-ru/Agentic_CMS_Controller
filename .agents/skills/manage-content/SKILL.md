---
name: "manage-content"
description: "Adds, edits, or deletes content nodes (like menu items, services) in the Agentic CMS by modifying Markdown and JSON files in the /src/content/ directories. Use this when a user asks to add something to the site's catalog, menu, or pages."
---

## Goal
To autonomously manage content entities within the Astro-based Agentic CMS without breaking the static site generation.

## When to Use
Trigger this skill whenever the user requests to:
- "Add a new menu item", "Create a new page", "Update the about us section", "Delete this service"
- Modify data that falls under `pages`, `nodes`, `taxonomy`, `blocks`, or `staff` content collections.

## Workflow
1. **Identify the Intent:** Determine which content collection the user's request targets (e.g., menu items typically go to `src/content/nodes/`).
2. **Read the Schema:** Read the collection schema in `src/content/config.ts` within your allowed workspace to understand the required frontmatter properties.
3. **Check I18n Constraints:** Agentic CMS supports multiple languages (`ru`, `en`, `es`, `pt-br`). **YOU MUST ALWAYS** generate, edit, or delete content files for **ALL** supported active locales simultaneously. Never generate content just for one language. Translate the content appropriately for each folder (e.g., `src/content/nodes/ru/`, `src/content/nodes/en/`, `src/content/nodes/es/`, `src/content/nodes/pt-br/`).
4. **Modify the Content:** 
   - **Create:** Create a new `.md` file with the correct YAML frontmatter and Markdown body.
   - **Update:** Use exact string replacement to update the relevant fields or body.
   - **Delete:** Move the file to a `.backup` extension or delete it if explicitly told.
5. **Verify Structure:** Ensure the frontmatter is valid YAML and the markdown is correctly formatted.
6. **Commit the Changes:** Use `git status` to see the changes, then `git add` and `git commit` them with a semantic commit message (e.g., `content(nodes): add new latte item to menu`). **Never push force**.

## Guardrails
- **DO NOT** modify the UI components (`.astro`, `.tsx`) or the business logic unless the user explicitly requests a design change.
- **DO NOT** delete Markdown files without explicit 'Human-in-the-loop' confirmation from the user.
- **NEVER** delete entire locale directories (e.g., `src/content/nodes/en/`, `es/`, `pt-br/`) to fix Astro build errors like `Missing parameter: slug`. These errors usually indicate a mismatch between `src/i18n/ui.ts` locales and the existing folder structure, or missing translations.
- **DO NOT** stray outside the `src/content/` directory of the active CMS project for this skill unless modifying global settings.

## Completion Checklist
- [ ] Content file (Markdown/JSON) is correctly placed in the right translation folder.
- [ ] Frontmatter strictly matches the collection schema.
- [ ] Changes have been committed to the local git repository in the CMS project.
