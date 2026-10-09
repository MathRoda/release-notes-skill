# Release Notes Skill

An AI agent skill that compares releases in Git and writes clear notes for non-technical users at a fifth-grade reading level.

It finds the previous release, checks commits and code changes, and explains what users can do or what now works better. It leaves out internal work, duplicate changes, and features that were fully reverted.

## Install with an AI agent

Paste this one-line prompt into your AI agent:

```text
Install the release-notes skill from https://github.com/MathRoda/release-notes-skill by reading release-notes/SKILL.md and adding it to this agent's supported skill or instruction location.
```

## Use

In the project you want release notes for, ask:

```text
Use the release-notes skill to write release notes from v1.4.0 to v1.5.0.
```

Or let the agent find the previous release and compare it with the current commit:

```text
Use the release-notes skill to write notes for the next release.
```

## Agent compatibility

The [skill](release-notes/SKILL.md) is plain Markdown with a name and description in YAML frontmatter. It has no provider-specific tools, APIs, or dependencies. The agent needs access to Git and the repository's files.

Agents with native skill support can install the `release-notes` folder in their supported skill location. For agents without native skill support, load `release-notes/SKILL.md` into the agent's project instructions or conversation. Installation paths and invocation syntax depend on the agent.

## Example output

The exact notes depend on the changes found in your repository:

```markdown
# What's new in 1.5.0

## New
- You can now save your reports as a spreadsheet.

## Fixed
- Fixed a problem that closed the app when you added large photos.
```
