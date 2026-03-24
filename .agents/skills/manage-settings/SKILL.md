---
name: "manage-settings"
description: "Modifies global site configuration, navigation menus, and metadata stored in JSON files under /src/content/settings/ directory. Use this when you are asked to change the global menu structure, correct site titles, or update working hours."
---

## Goal
To safely modify system JSON configurations without breaking the strict Astro content schemas.

## When to Use
Trigger when the user intent is:
- "Change the navigation menu to point to /about-us"
- "Update our contact phone number in the footer"
- "Add 'ru' language fallback"

## Workflow
1. **Locate Settings:** Understand that settings are located in `src/content/settings/` of the active CMS project and are typically in `.json` format mapped by language (e.g., `navigation.en.json`).
2. **Read the Schema:** Always check `src/content/config.ts` first. JSON files must perfectly match the internal Zod schema structure.
3. **Parse and Update:** Read the `.json` file, manipulate the data structure ensuring keys perfectly match the schema, and write it back formatted beautifully with 2 spaces.
4. **Ensure Localization Consistency:** You MUST ALWAYS update the corresponding settings/links in **ALL** other locale files (e.g., `navigation.ru.json`, `navigation.en.json`, `navigation.es.json`, `navigation.pt-br.json`) at the same time. Never leave translations missing or out of sync.
5. **Commit:** Ensure changes are staged and committed using atomic descriptions (e.g., `chore(settings): update phone number in contact settings`). Never run `git push --force`.

## Guardrails
- **DO NOT** edit code or layout files inside `/src/layouts/` or `/src/components/`.
- **CRITICAL:** **DO NOT** rename or remove existing top-level JSON keys or their internal structures! You MUST ALWAYS preserve required metadata fields (like `id` and `locale`) and keep existing data shapes intact (e.g., do not rename `hours` to `workingHours`, do not rename `social` to `socials`, do not change `main_menu` to `mainMenu`). Only modify their string/number values.
- **NEVER** delete setting files (e.g., `navigation.en.json`, `navigation.es.json`) to "fix" Astro build errors like `Missing parameter: slug`. These errors usually indicate a mismatch between `src/i18n/ui.ts` locales and the existing folder structure or missing translation files.
- **DO NOT** introduce fields into JSON files that are not explicitly defined in `config.ts` (Zod schema) or already present in the existing file.

## Completion Checklist
- [ ] You have verified the schema constraints before writing.
- [ ] The JSON validation passes.
- [ ] Changes are committed to the target CMS repository.
