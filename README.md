# Release Notes Skill

An AI agent skill that turns Git changes into clear release notes for non-technical users at a fifth-grade reading level, with Humanizer built in.

Release Notes finds the previous release, checks commits and code changes, and explains what users can do or what now works better. It leaves out internal work, duplicate changes, and features that were fully reverted. Its built-in Humanizer step removes stiff wording, filler, and sales language while keeping the facts.

## Install with an AI agent

Paste this one-line prompt into your AI agent:

```text
Install the release-notes skill from https://github.com/MathRoda/release-notes-skill by copying the whole release-notes folder, including its built-in Humanizer reference and LICENSE, into this agent's supported skill or instruction location.
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

The [skill](release-notes/SKILL.md) is plain Markdown with a name and description in YAML frontmatter. It uses no provider-specific tools or APIs. The agent needs access to Git and the repository's files.

Agents with native skill support can install the whole `release-notes` folder in their supported skill location. For agents without native skill support, load `release-notes/SKILL.md` and its internal `references/humanizer.md` guide into the agent's project instructions or conversation. Installation paths and invocation syntax depend on the agent.

## Built-in Humanizer

The skill checks the Git changes, writes the draft, and runs its built-in Humanizer editing step. The core editing rules are in `SKILL.md`, with the full guide in `references/humanizer.md` inside the same skill folder. The final notes keep the same facts, limits, and required user actions, with simple words and short sentences. One installation includes the whole workflow.

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

The built-in editing guidance comes from Humanizer version 2.11.2 from [blader/humanizer](https://github.com/blader/humanizer), copyright 2025 Siqi Chen. The guide is included as an internal reference under the [MIT license](release-notes/LICENSE).
