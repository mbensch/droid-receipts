---
description: Remove steering from the project. Deletes STEERING.md, the tech-stack skill, and removes the steering section from AGENTS.md.
---

Remove project steering rules. Follow these steps exactly:

## Step 1: Confirm with the user

Use the AskUser tool to ask:

> "This will permanently delete STEERING.md, remove the .factory/skills/tech-stack/ skill, and remove the steering section from AGENTS.md. Are you sure you want to proceed?"

Options:
- Yes, remove everything
- Cancel

If the user cancels, stop and report that no changes were made.

## Step 2: Delete STEERING.md

Check if `STEERING.md` exists in the project root.
- If it exists, delete it.
- If it does not exist, note that it was not found (skip silently).

## Step 3: Delete the tech-stack skill

Check if `.factory/skills/tech-stack/` exists in the project root.
- If it exists, delete the entire `.factory/skills/tech-stack/` directory including `SKILL.md`.
- If the `.factory/skills/` directory is now empty, delete it too.
- If the `.factory/` directory is now empty, delete it too.
- If the directory does not exist, note that it was not found (skip silently).

## Step 4: Remove steering section from AGENTS.md

Check if `AGENTS.md` exists in the project root.
- If it does not exist, skip this step.
- If it exists, read its contents and look for the steering section added by `/init-steering`. The section starts with:

  ```
  ## Steering Rules (Non-Negotiable)
  ```

  Remove that heading and everything beneath it that belongs to that section (until the next `##` heading or end of file). Also remove the blank line immediately preceding the heading if one exists.

  Leave all other content in AGENTS.md completely untouched. If the file becomes empty after removal, delete the file.

## Step 5: Report results

Summarize what was done, listing each action taken and whether it succeeded or was skipped:
- `STEERING.md` -- deleted or not found
- `.factory/skills/tech-stack/` -- deleted or not found
- `AGENTS.md` steering section -- removed or not found
