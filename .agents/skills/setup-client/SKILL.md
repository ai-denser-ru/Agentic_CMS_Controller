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
   - If **New**: Ask for a new project name. Clone the base boilerplate `https://github.com/ai-denser-ru/Agentic_CMS.git` into a new folder, remove its `.git` history (`rm -rf .git`), and initialize a fresh git repository (`git init`).
3. **Clean and Contextualize Boilerplate:** If a new site was just cloned:
   - Delete or rewrite boilerplate-specific files that shouldn't be in the client's repo (delete `MVP.md` and `AGENTS.md`; completely rewrite `README.md` to be about the client's new project).
   - Edit `astro.config.mjs`: remove `base: '/Agentic_CMS'` (or set it to `/` if needed) and update `site` to the client's actual production domain or a placeholder.
   - Edit `src/i18n/ui.ts`: remove or replace boilerplate strings like "Agentic_CMS" and branded texts like "Работает на Astro.js" with the client's actual brand.
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
