---
name: release-notes
description: Generate release notes for non-technical users by comparing the previous release with the next release in Git history. Use when asked to write release notes, summarize a version update, or explain what changed for users between releases. Write at a fifth-grade reading level.
---

# Release Notes

Turn changes between two releases into clear notes for everyday users. Describe what people can do and what now works better. Use Git history and final code changes as evidence, rather than rewriting commit titles.

These instructions are agent agnostic. Use the host agent's available shell and file-reading tools to run Git and inspect the repository. No provider-specific APIs or tool names are required. If the agent lacks repository access, request the relevant Git history and diffs instead of claiming to have inspected them.

## Find the release range

1. Use the repository and product named by the user, or the current repository. Check release conventions, existing notes, version files, and tags. For a repository with several products, use the selected product's tags and paths. Ask which product only if context cannot resolve it.
2. Honor any previous and next versions, tags, branches, or commits the user provides. Resolve both endpoints to commit IDs. A version name must map to repository evidence; do not invent a tag.
3. If the next release is unspecified, use `HEAD`. If it has a matching release tag, use that version label. Otherwise call the notes **Unreleased** unless the user or repository explicitly identifies the upcoming version. Never assume the next version number.
4. If the previous release is unspecified, find the most recent earlier release of the same product on the target's release line. Exclude tags at the target commit itself. Use ancestry, tag naming, version history, and release records together; the highest version or newest tag date alone does not establish the previous release. Prefer stable releases for a stable release; use the same preview channel for preview-to-preview notes.
5. If there are no usable tags, look for release commits or recorded commit IDs in version history and release records. If no boundary is supported, ask for the previous release ref. Compare from the repository's beginning only when this is established as the first release. A shallow clone with missing history is not evidence of a first release.

Useful read-only discovery commands:

```sh
git rev-parse --show-toplevel
git status --short
git rev-parse --is-shallow-repository
git log -20 --first-parent --decorate --oneline
git tag --list
```

After choosing refs, use shell variables with proper quoting. Resolve them before comparing:

```sh
base=$(git rev-parse --verify --end-of-options "${base_ref}^{commit}")
target=$(git rev-parse --verify --end-of-options "${target_ref}^{commit}")
git tag --merged "$target" --sort=-version:refname
git merge-base --is-ancestor "$base" "$target"
```

The sorted tag list contains candidates, not an automatic answer. If supplied endpoints diverge, compare their final states as requested and investigate changes unique to each side. Do not silently replace the previous release with a merge base. If divergence makes an inferred boundary uncertain, ask for the release refs.

Keep uncommitted changes out unless the user asks to include them. If an endpoint or required history is missing locally, retrieve it when access is available and authorized, or ask for the missing ref/history. Do not guess from an incomplete range.

## Establish what actually changed

Inspect both commits introduced after the previous release and the net change between release states:

```sh
git log --reverse --format='%h %s%n%b' "$base..$target" --
git diff --stat "$base" "$target" --
git diff --name-status "$base" "$target" --
git diff "$base" "$target" -- path/to/relevant/files
```

For one product in a larger repository, scope diffs to its files and relevant shared code. Follow shared code when it affects that product. Use focused diffs for large releases; do not make claims from truncated output.

For each possible note, establish:

- What changed in the final target state?
- What does a user now see, do, or experience?
- Which commit and changed behavior support the claim?
- Is it available to this audience, or still gated, incomplete, or internal?

Read code, tests, documentation, and configuration as needed to resolve unclear commit messages. Existing release notes can guide tone and product vocabulary, but cannot replace the current comparison. Keep a private evidence map from each proposed note to supporting changes.

Combine commits that deliver one user change into one note. Trace fixes and reverts together; omit a feature added and then fully removed. Mention a reverted change only if the final release differs from the previous release in a way users will notice. Do not count a merge and its underlying commits as separate improvements.

Omit internal refactoring, build tools, tests, routine dependency bumps, and developer documentation unless they produce a supported user benefit. A dependency update alone does not prove the app is faster or safer. Do not invent performance gains, availability, resolved symptoms, or security claims. If an important change remains unclear, ask a focused question or report the gap separately from public notes.

## Write for non-technical readers

Aim for fifth-grade reading level with an adult, respectful tone:

- Use familiar words and short, direct sentences. Aim for roughly 8 to 15 words per sentence; split long ideas when useful.
- Describe the user's action or the problem they experienced. Prefer “You can…” and “Fixed a problem that…” over implementation details.
- Use one idea per bullet, usually one sentence. Add a second short sentence when users need a step or limit.
- Keep product names and screen labels accurate. Explain unavoidable technical terms in simple words.
- Leave commit hashes, issue numbers, code names, acronyms, and file paths out of public notes unless requested.
- Avoid vague filler such as “various improvements,” “enhanced experience,” and “bug fixes.” Name the supported change.
- Describe confirmed benefits without hype. Do not claim every crash is fixed when one specific crash was fixed.
- If a change removes a feature or requires action, say what changed and what the user should do. Do not hide it among small fixes.

Examples of the translation style, only when supported by the changes:

| Technical evidence | User-facing note |
| --- | --- |
| Added CSV export to the reports screen | You can now save your reports as a spreadsheet. |
| Fixed a crash when attaching large photos | Fixed a problem that closed the app when you added large photos. |
| Search now matches names without case sensitivity | Search finds names whether you use big or small letters. |
| Refactored caching with no demonstrated user effect | Omit this change. |

Treat reading level as a writing target. Do not claim a measured grade score unless a readability tool was used. Preserve accuracy when a product term is longer than a simple word.

## Edit with Humanizer

After drafting the notes, read the bundled [Humanizer skill](../humanizer/SKILL.md) and apply its rewrite process in **embedded mode**. If the host agent loads installed skills by name, load `humanizer` instead. The package installs both skill folders side by side so the relative link works. If neither copy is available, report the missing editing step outside the public notes rather than claiming to have used it.

Edit only the wording of the verified draft. Keep each supported change, product name, availability limit, and required user action. Remove filler, hype, stock AI phrases, and stiff sentence patterns. Keep the fifth-grade reading target and the user's requested format. Let sentence lengths vary naturally within that target; do not force dramatic fragments or identical openings. Use a respectful product voice without adding opinions, invented anecdotes, or facts.

Return only the final release notes, without Humanizer's intermediate draft or list of AI patterns. When writing a file, keep code blocks, metadata, and link targets unchanged. Check the edited notes against the evidence map to catch any changed meaning before delivery.

## Deliver the notes

Follow the user's requested format or destination. Otherwise return a concise Markdown draft with a version title and only sections with useful entries. Put the most useful changes first. Use **New**, **Better**, and **Fixed** when grouping helps; add **What you need to do** when action is required. Omit empty sections and an introduction that just repeats the bullets.

```markdown
# What's new in [verified version or Unreleased]

## New
- You can now [supported new action].

## Better
- [Specific improvement users will notice].

## Fixed
- Fixed a problem that [specific user symptom].
```

Include a release date only if the user or a release record supplies it; a commit date does not prove a release date. Do not leave placeholders in the draft. If there are no verified user-facing changes, say so instead of filling notes with internal work.

Give a brief comparison scope outside the public notes, identifying previous and next refs and any material evidence gap. Keep technical evidence out of text intended for publication. Write a file when requested or when the repository's workflow calls for it; otherwise deliver the draft in chat. Publishing, creating a release, and committing changes are separate actions that require authorization for that action.

Before delivery, check that every note describes a net change in this release, has evidence, and makes sense without knowing how the product was built. Remove duplicates, jargon, vague claims, and unsupported benefits.
