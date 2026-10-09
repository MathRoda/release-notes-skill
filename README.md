# Release Notes Skill

A pair of AI agent skills that turn Git changes into clear release notes for non-technical users at a fifth-grade reading level.

Release Notes finds the previous release, checks commits and code changes, and explains what users can do or what now works better. It leaves out internal work, duplicate changes, and features that were fully reverted. Humanizer edits the draft to remove stiff wording, filler, and sales language while keeping the facts.

## Install with an AI agent

Paste this one-line prompt into your AI agent:

```text
Install both release-notes and humanizer from https://github.com/MathRoda/release-notes-skill in this agent's supported skill or instruction location, keeping their folders side by side and preserving humanizer/LICENSE; use Humanizer as the final editing step for release notes.
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

Both [Release Notes](release-notes/SKILL.md) and [Humanizer](humanizer/SKILL.md) are plain Markdown with a name and description in YAML frontmatter. They use no provider-specific tools or APIs. The agent needs access to Git and the repository's files.

Agents with native skill support can install the `release-notes` and `humanizer` folders side by side in their supported skill location. For agents without native skill support, load both `SKILL.md` files into the agent's project instructions or conversation. Installation paths and invocation syntax depend on the agent.

## How the skills work together

Release Notes checks the Git changes and writes the draft. It then reads the bundled Humanizer instructions and edits the wording. The final notes keep the same facts, limits, and required user actions, with simple words and short sentences. You only need to ask for release notes; the editing step is part of the workflow.

## Example output

The exact notes depend on the changes found in your repository:

```markdown
# What's new in 1.5.0

## New
- You can now save your reports as a spreadsheet.

## Fixed
- Fixed a problem that closed the app when you added large photos.
```

## Humanizer credit

This package includes Humanizer version 2.11.2 from [blader/humanizer](https://github.com/blader/humanizer), copyright 2025 Siqi Chen. Its instructions are included unchanged under the [MIT license](humanizer/LICENSE).
