---
name: "manage-settings"
description: "Modifies global site configuration, navigation menus, and metadata at Layer 3. Use this when you are asked to change the global menu structure, correct site titles, or update working hours."
---

## Goal
To safely modify system configurations using the Read-Modify-Write pattern via MCP tools.

## When to Use
Trigger when the user intent is:
- "Change the navigation menu to point to /about-us"
- "Update our contact phone number in the footer"
- "Add 'ru' language fallback"

## Workflow (Read-Modify-Write)
1. **Read Current State:** Call the MCP tool `get_settings(setting_type, locale)` to retrieve the current JSON configuration.
2. **Modify Object:** Apply the requested changes to the obtained JSON object. 
   - **STRICT RULE:** Keep all other fields untouched. 
   - **I18n Parity:** You MUST repeat this for ALL supported locales (`ru`, `en`, `es`, `pt-br`) to ensure synchronization.
3. **Save Changes:** Call the MCP tool `update_settings(setting_type, locale, payload)` to commit the changes.
   - Use the modified JSON as the `payload`.

## Guardrails
- **CRITICAL:** **DO NOT** use any shell commands or direct file system access.
- **Data Integrity:** Do not rename or remove existing top-level JSON keys. Only modify their values.
- **No-Auto-Translation:** Never use automatic translation for values. Copy from source and request translation if needed.

## Completion Checklist
- [ ] Setting retrieved via `get_settings`.
- [ ] Payload validated and sent via `update_settings` for all locales.
- [ ] User is informed about the updates.
