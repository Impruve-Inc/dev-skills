# Cleaning up an existing skill

Read this before editing an existing skill, on either path.

## Inventory before editing

List what the current skill makes the agent do: each step, branch, guard, and output format. Every item on the list either survives the cleanup or is removed for a stated reason, such as "duplicates step 3" or "depends on a tool that no longer exists". The adversarial reviewer checks the new skill against this list.

## What to fix

Check each of these and fix only what applies:

- **Dependencies.** Does the skill depend on other skills, plugins, tools, MCP servers, scripts, or files that the target users don't have? Bring the needed guidance into this skill, or state the requirement and how to check it is met.
- **Claims and sources.** Open every cited source and confirm it says what the skill claims. Remove or correct misquotes and claims the source doesn't support.
- **Links and paths.** Confirm every relative link and file path exists.
- **Stale content.** Look for dated facts, version numbers, retired features, and history ("we used to…", "since the migration…").
- **Narration.** Look for discovery stories, dates, ticket numbers, and people's names.
- **Harness lock-in.** Look for frontmatter fields, commands, or tool names that only one app understands, when the skill is meant to run in several. The precedence section of the standards reference lists the portable fields.
- **Bloat.** For each sentence, ask whether the agent would get the task wrong without it. Delete sentences that fail. Treat ALL-CAPS "ALWAYS" and "NEVER" as a sign that the reason is missing: state the reason instead.
- **The skill's own docs, changelog and eval cases.** Fix them when they disagree with the skill. Follow the repository's versioning and changelog rules.
- **Scripts.** Read every bundled script before running it. Check that it handles errors and lists its dependencies, and that it contains no secrets. Run its tests if they exist.

## What to leave alone

Leave working passages alone even if you would have phrased them differently. A cleanup that rewrites everything is hard to review and can lose behavior without anyone noticing. Keep the diff as small as the fixes allow, and give a reason for each change in the handoff report.
