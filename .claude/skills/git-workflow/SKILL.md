---
name: git-workflow
description: How I work in git — a worktree per task, Conventional Commits, and PR bodies that say what was done instead of restating the diff. Apply before starting work on a task, and when writing a commit, opening a PR, or editing a PR description.
---

# Git workflow

## Work in a worktree, not in the main checkout

The primary checkout is mine. I switch branches in it while a session is running, without saying
so. A session that assumes the branch it checked out is still the branch it is on will commit
somewhere unintended, and the failure is silent — `git commit` succeeds, the push reports
"Everything up-to-date", and the mistake only surfaces later when the PR has nothing in it.

So take a worktree per task. It is a separate directory with its own checked-out branch, sharing
one object store, so my branch switching cannot reach it.

```sh
git worktree add ../bma98.github.io-<task> -b <branch> origin/master
```

Work there for the whole task: edit, commit, push, open the PR. When it is merged:

```sh
git worktree remove ../bma98.github.io-<task>
git branch -d <branch>
```

`git worktree list` shows what is currently checked out where. A branch can only be checked out in
one worktree at a time, which is the property doing the work here.

In Claude Code, `isolation: "worktree"` on a subagent gives that agent its own worktree and cleans
it up if nothing changed. There are also `EnterWorktree` and `ExitWorktree` tools for moving the
current session into one.

### If you are in the main checkout anyway

Branch the task first, and **verify the branch immediately before every commit and push**:

```sh
git branch --show-current
```

Never infer the current branch from a `checkout` earlier in the session. That inference is exactly
what breaks. If a commit lands in the wrong place, the fix is `git branch -f <intended> <sha>` and
then resetting the branch that wrongly received it.

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

## Merging

I merge my own PRs. Do not merge without being asked.

A PR merged from the GitHub UI takes whatever is on the branch **at that moment**. If a session
pushes more commits after I have merged, they are stranded on a dead branch and need a fresh PR, so
push before saying the work is ready rather than after.
