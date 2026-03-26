---
name: "setup-client"
description: "Initializes, clones, or validates a client's specific website repository based on the Agentic CMS boilerplate. Use this when the user asks to start a new project, connect a new site, or verify if the current environment is ready for content editing."
---

## Goal
To autonomously manage the lifecycle of a client website repository, ensuring it is properly cloned, connected, and structurally compatible with the Agentic CMS boilerplate before any content edits occur.

## When to Use
Trigger this skill when the user:
- Asks to "Start a new website for a client"
- Asks to "Connect to a repository"
- Asks to edit content but no valid Astro.js CMS project is found in the current workspace.

## Workflow
1. **Detect Environment:** Check the current workspace directory. 
   - Does it contain a valid Astro project? Look for `astro.config.mjs` and `src/content/`.
   - If YES, proceed to step 3 (Validation).
   - If NO, explain to the user that no client site is connected in this directory.
2. **Setup or Clone:** 
   - Ask the user: "Do you want to connect an existing site repository, or create a brand new one from the Agentic CMS boilerplate?"
   - If **Existing**: Ask for the Git URL and use `git clone <URL>` into the workspace.
   - If **New**: 
    - **Preparation**: 
      - Clone the boilerplate: `git clone https://github.com/ai-denser-ru/Agentic_CMS.git .`
      - **CRITICAL**: Do NOT delete `AGENTS.md`. This file contains your core instructions. You may delete `README.md` and `MVP.md` to replace them with client-specific content.
      - Initialize git: `rm -rf .git && git init`.
    - **Content Parity**: 
      - You MUST maintain 4-locale structural parity at all times.
      - When creating the first content nodes, do NOT simply delete all `placeholder.md` files. Instead, **replace** them with actual content for the client in **ALL 4 locales** (`ru`, `en`, `es`, `pt-br`).
      - If you don't have translations yet, copy the source language content and add `<!-- TODO: Translate -->`.
3. **Boilerplate Content Sync (Build-Safe):** If a new site was just cloned:
   - Delete boilerplate-specific management files: `MVP.md`.
   - Rewrite `README.md` to be a professional welcome for the client's project.
   - **Boilerplate Content Sync (Build-Safe):** The Agentic CMS boilerplate ALREADY contains a full structure of `placeholder.md` files for all collections and locales. 
     - **MANDATORY VERIFICATION**: For each locale `[ru, en, es, pt-br]`, verify that the following directories and files exist (do not recreate if they are already present):
       - `src/content/nodes/<locale>/placeholder.md`
       - `src/content/blocks/<locale>/placeholder.md`
       - `src/content/staff/<locale>/placeholder.md`
       - `src/content/taxonomy/<locale>/placeholder.md`
     - **ACTION**: If any of these are missing (e.g., if the user is using an outdated boilerplate), create a simple `placeholder.md` using a single `printf --` command per file (e.g., `printf -- "---\ntitle: Placeholder\ndraft: true\n---\n" > path/to/file.md`). Avoid complex nested shell loops with `cat <<EOF` to prevent bash syntax errors.
     - **Site Configuration**: NEVER edit `astro.config.mjs` or `src/i18n/ui.ts` directly. Instead, update `src/content/settings/site.json` with the client's information:
      - `site`: `https://[username].github.io`
      - `base`: `/[project-name]`
      - `defaultLocale`: `ru` (or as requested).
    - **Structural Initialization**: Ensure that `src/content/pages/` contains exactly `index.md` and `about.md` in **ALL** supported locales.
      - You MUST update the content of these pages to match the client's name and basic business type in the corresponding language.
    - **Audit Step**:
      - Run `ls -R src/content/` to verify parity.
      - Run `npx astro check` to verify build health.
      - **FORBIDDEN**: Do NOT run `npm install` to "fix" missing packages like `@astrojs/check`. These MUST already be in `package.json`. If `astro check` fails due to missing packages, report it as a boilerplate bug.
4. **Link Remote Repository (for New Sites):**
   - Ask the user to create an empty repository (e.g., on GitHub/GitLab) and provide the URL.
   - Use `git remote add origin <URL>`, then create an initial commit: `git branch -M main && git add . && git commit -m "chore: initial CMS boilerplate setup"`.
   - Push to the remote repository: `git push -u origin main`.
5. **Verify Integrity:** After cloning or detecting a project, verify its structure. It MUST have:
   - `src/content/config.ts` defining the Zod schemas.
   - `/src/content/nodes/` and `/src/content/pages/` directories.
   - If these are missing, warn the user that the repository is NOT compatible with Agentic CMS.
6. **Finalize Setup:** Once validated, inform the user that the site is ready for content editing and ask what they would like to add or change.

## Guardrails
- **DO NOT** clone a new site inside an existing Git repository (abort and tell the user to navigate to a clean workspace directory like `~/DEV/`).
- **DO NOT** execute destructive git commands (like `git reset --hard`) on an existing client repository without asking.
- **DO NOT** overwrite files if cloning into a non-empty directory.
- Always require the user to provide the exact repository URL or project name; do not guess.

## Completion Checklist
- [ ] A valid clone of a client site or the boilerplate is present in the workspace.
- [ ] The structural compatibility (Zod schemas, Astro config) has been verified.
- [ ] The user has been notified of the successful setup.
