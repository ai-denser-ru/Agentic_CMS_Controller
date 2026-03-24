---
name: "manage-content"
description: "Adds, edits, or deletes content nodes (like menu items, services) in the Agentic CMS by modifying Markdown and JSON files in the /src/content/ directories. Use this when a user asks to add something to the site's catalog, menu, or pages."
allowed-tools: ReadWrite, Glob, Grep, Git
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
6. **Commit and Push the Changes:** Use `git status` to see the changes, then `git add` and `git commit` them with a semantic commit message (e.g., `content(nodes): add new latte item to menu`). Finally, run `git push` to upload the changes to the client's remote repository. **Never push force**.

## Guardrails
- **Proactive Inquiry (No Guessing):** If a task or schema requirement is ambiguous, DO NOT guess. You must ask the user for clarification before executing.
- **Scope Constraint:** You are STRICTLY FORBIDDEN from writing, translating, or modifying localization files or schemas without explicit manual user confirmation.
- **Anti-Translation Rule:** NEVER copy Russian text into English/Spanish/Portuguese files to "synchronize" them. NEVER use automated translation for content without being explicitly asked. If you are asked to add a new item, add it in the requested language and ask the user if they want you to translate it for other active locales.
- **Anti-Schema Modification Rule:** If a language schema enforces fields like `vocabulary: category`, DO NOT guess or infer other schema structures. DO NOT run recursive scripts trying to "fix" schema mismatches. Run exactly what the user asks and nothing more.
- **DO NOT** modify the UI components (`.astro`, `.tsx`) or the business logic unless the user explicitly requests a design change.
- **DO NOT** delete Markdown files without explicit 'Human-in-the-loop' confirmation from the user.
- **NEVER** calculate or convert prices/currencies. Always keep the exact original numerical value and currency symbol (e.g., use '₽' for all languages) unless explicitly instructed otherwise.
- **DO NOT** stray outside the `src/content/` directory of the active CMS project for this skill.

## Completion Checklist
- [ ] Content file (Markdown/JSON) is correctly placed in the right translation folder.
- [ ] Frontmatter strictly matches the collection schema.
- [ ] Changes have been committed to the local git repository in the CMS project.
