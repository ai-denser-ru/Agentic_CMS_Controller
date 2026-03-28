---
name: "setup-client"
description: "Initializes or validates a client's website repository. Use this to ensure the environment is ready for the LUI Orchestrator to work via MCP."
---

## Goal
To ensure a valid client site instance (`Agentic_CMS_ClientName`) is present, properly structured, and accessible to the Layer 3 MCP server.

## When to Use
Trigger this skill when the user:
- "Start a new project for a client"
- "Initialize the environment"
- When MCP tools fail due to a missing directory.

## Workflow
1. **Verify Connection:** Attempt to call `get_settings` via MCP.
   - If SUCCESS: The site is ready. Inform the user and transition to content skills.
   - If FAILURE (Path not found): Proceed to Step 2.
2. **Initialization (The "USB-C" Connection):**
   - Ask the user for the Client Name and Git Repository URL.
   - **Clone:** Use `git clone` to create the site instance from the boilerplate.
   - **Isolate:** Ensure the site is in a dedicated folder (e.g., `Agentic_CMS_Site`).
3. **Bootstrap Content:**
   - Ensure the 4-locale structure (`ru`, `en`, `es`, `pt-br`) is intact.
   - Use `get_settings` and `update_settings` to set the client's business name and URLs.
4. **Validation Loop:**
   - Call `list_pages` and `list_nodes` via MCP to verify the Layer 3 connection is active.
   - Report status to the user.

## Guardrails
- **NO DIRECT FS:** After the initial `git clone`, all content operations MUST switch to MCP tools.
- **STRICT LOCALE PARITY:** Never skip a locale during setup.
- **AGENTS.md PROTECTION:** Never overwrite the controller's rules during site setup.

## Completion Checklist
- [ ] Site instance directory created and populated.
- [ ] MCP server successfully communicates with the new site.
- [ ] Basic client settings (site name, locale) initialized.
- [ ] User notified that the "Orchestrator is online".
