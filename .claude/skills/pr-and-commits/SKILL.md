---
name: pr-and-commits
description: How I write commit messages and pull request descriptions — Conventional Commits, and PR bodies that say what was done instead of restating the diff. Apply when writing a commit, opening a PR, or editing a PR description.
---

# Commits and pull requests

## Commits: Conventional Commits

Every commit subject follows the spec:

```
<type>[optional scope][!]: <description>
```

- **type** — one of `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`,
  `chore`, `revert`.
- **scope** — optional, in parentheses. Use it when the repo has parts worth naming
  (`fix(auth):`). Skip it in a small repo where everything is one thing.
- **description** — imperative mood, lowercase, no trailing period. "add the charger poll", not
  "Added the charger poll." or "adds polling".
- **`!`** — goes before the colon for a breaking change (`feat(api)!: drop the v1 endpoints`), and
  the body must then carry a `BREAKING CHANGE:` footer explaining the break.

Keep the subject under about 72 characters.

Picking the type is usually obvious, with two traps worth naming. `style` means formatting with no
behavior change, not visual design work on a UI. `refactor` means the behavior is identical
afterwards; if a user could notice the difference, it is a `feat` or a `fix`.

### The body

Optional. Add one when the change needs a *why* that the diff cannot show: a constraint you worked
around, a decision between two options, a non-obvious consequence. Blank line after the subject,
wrapped prose, no bullet-by-bullet retelling of the files you touched.

Close with the trailer when I am pairing with Claude:

```
Co-Authored-By: Claude <noreply@anthropic.com>
```

## Pull requests: say what you did, not what the diff says

**The golden rule: never restate the change.** No before-and-after quotes of edited text, no
walkthrough of the modified lines, no paragraph defending a wording or naming choice. The diff is
right there and it is the source of truth. A body that paraphrases it is noise, and it goes stale
the moment someone pushes another commit.

A normal PR body is two or three lines:

- **Why this exists** — the purpose, or the thing that prompted it.
- **What changed** — one sentence at the altitude of behavior or intent, not line edits.
- **What a reviewer cannot see from the diff** — only if there is something. A merge that went
  wrong, a check I skipped, a known risk, a follow-up that is deliberately out of scope.

That third point is the only reason a PR body ever grows. Real context the diff lacks earns its
space: a lost source project that explains why a rewrite was the only option, a privacy problem the
change quietly fixes, a verification step that was not possible in the session. Length that comes
from explaining the diff more slowly does not.

Skip the sections that pad a template. No "Checks" list of things that obviously pass, no "Notes"
restating the commit body, no screenshots of unchanged UI.

Close the body with:

```
🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

### Example

A one-paragraph text edit, in full:

```markdown
Follow-up to #1, which was merged before these commits landed, so the live site
still shows the old copy.

Rewrites the bio paragraph and updates the meta description to match. One file,
no structural changes.
```

Not a before-and-after of both paragraphs, and not a bullet arguing why a date was left out.
