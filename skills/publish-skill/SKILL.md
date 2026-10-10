---
name: publish-skill
description: "Prepare, validate, package, and publish portable Agent Skills through GitHub or npm. WHEN: \"publish a skill\", \"publish SKILL.md\", \"ship a skill\", \"release an agent skill\", \"make this skill installable\", \"add a skill to my package\"."
license: MIT
compatibility: "Requires an agent with filesystem and shell access. Network access is needed for remote sources."
---

# Publish Skill

Turn a skill into a portable artifact, prove it works, then publish only to a destination the user authorized.

## Workflow

1. Inspect repository instructions, status, remotes, package metadata, and existing skill directories. Preserve unrelated changes.
2. Define concrete trigger prompts and expected outcomes. Keep one workflow per skill unless the tasks share inputs, tools, and validation.
3. Create `skills/<skill-name>/SKILL.md`. For an npm package whose docs should seed the skill, run:

   ```bash
   npx skilld author package --yes
   ```

4. Refine the generated or handwritten skill:

   - Match the directory and frontmatter `name`; use lowercase letters, numbers, and hyphens.
   - Make `description` state what the skill does and when it triggers.
   - Use imperative, ordered steps with validation and recovery.
   - Keep the body concise. Add `references/`, `scripts/`, or `assets/` only when they reduce repeated work or context cost.
   - Remove absolute paths, secrets, temporary files, symlinks, generated caches, and `.skilld/` internals.

5. Validate the portable directory:

   ```bash
   npx -y skills-ref validate skills/<skill-name>
   npx skilld add ./ --skill <skill-name> --yes
   ```

   Fix every failure, then rerun both commands. Execute bundled scripts with representative input.
6. Forward test with at least one realistic trigger prompt in a fresh agent session. Verify activation, outcome, and failure behavior. Tighten the description or workflow when the test misses.
7. Prepare the destination:

   - Standalone repository: keep skills under `skills/`, add a concise root README and license, then commit and push.
   - npm package: ensure `package.json#files` includes `skills`, run the package's checks, then confirm `npm pack --dry-run` contains every skill file.

8. Review the final diff and publish. Do not create a remote, push, tag, release, or publish an npm package without explicit authorization for that action.
9. Verify the published source by installing it into a temporary project with `npx skilld add gh:<owner>/<repo> --skill <skill-name> --yes`.
