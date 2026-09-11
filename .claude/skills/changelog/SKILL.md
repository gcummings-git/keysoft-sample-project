---
name: changelog
description: Generate a release changelog from recent git commits and write it to CHANGELOG.md. Use when cutting a release or asked to summarize what changed since a tag.
context: fork                       # run in an isolated sub-context: the verbose git log/diff output never touches the main session, only the summary returns
background: false                   # wait for the result instead of it landing later
allowed-tools: ["Bash(git log:*)", "Bash(git diff:*)", "Bash(git tag:*)", "Read", "Write"]  # read-only git + read/write the changelog; nothing else
argument-hint: "[since-tag]"        # quoted: unquoted [since-tag] would parse as a YAML list, not a string
---
# Changelog

Write a release changelog for this repository into `CHANGELOG.md` at the repo root.

The argument is: `$ARGUMENTS`

## 1. Resolve the commit range

Run `git tag -l --sort=-v:refname` to list tags.

- If the argument **is an existing tag**, the range is `<argument>..HEAD` and the new section is titled `Unreleased`.
- If the argument is **not an existing tag** (e.g. `v1.2.0` for a release you're about to cut), use it as the new section's title. The range is `<latest tag>..HEAD`, or the full history if the repo has no tags.
- If no argument was given, title the section `Unreleased` and use `<latest tag>..HEAD` (or full history).

## 2. Gather the changes

- `git log <range> --no-merges --format='%h %ad %an%n%s%n%b' --date=short` for commit messages.
- `git diff --stat <range>` (or `git log --stat` when there's no base tag) to see which files each change touched. Look at a specific commit's diff with `git log -p -1 <sha>` only when its message is too vague to classify.

## 3. Write CHANGELOG.md

Follow the [Keep a Changelog](https://keepachangelog.com) format. Group entries under `Added`, `Changed`, `Fixed`, `Removed` — omit empty groups. Each entry is one line in plain language describing the user-visible effect, followed by the short SHA in parentheses. Collapse duplicate or trivial commits (e.g. several "Initial commit"s) into a single entry. Date the section with today's date.

If `CHANGELOG.md` already exists, Read it first and insert the new section above the previous newest one, keeping everything else intact. Otherwise create it with a `# Changelog` heading.

## 4. Report back

Return only a short summary to the caller: the section title, the commit range used, how many commits were included, and the counts per group. Do not echo the raw git output.
