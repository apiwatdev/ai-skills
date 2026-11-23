# Git analysis reference

Use this guide for an accurate, read-only assessment. Commands are illustrative: validate refs and use safe quoting for names; never interpolate untrusted branch names into shell strings without proper argument handling.

## Baseline read-only commands

```bash
git rev-parse --is-inside-work-tree
git rev-parse --show-toplevel
git symbolic-ref --quiet --short HEAD
git remote -v
git branch -vv
git status --short
git rev-parse HEAD
```

Automatic refresh after identifying the correct remote:

```bash
git fetch origin
```

If fetch fails, stop and ask whether stale refs may be used. `git fetch` updates remote-tracking refs/FETCH_HEAD; it does not incorporate changes into the local working branch. Do not use `git pull`, `git fetch --prune`, `checkout`, `reset`, or branch-changing commands.

Example only (substitute actual validated refs from repository):

```bash
# Source/local/remote state (when tracking branch exists)
git rev-list --left-right --count origin/feature/example...HEAD
git log --oneline origin/feature/example..HEAD

# Current target and source revisions
git rev-parse origin/develop
git rev-parse HEAD

# Graph and branch contribution
git merge-base --all origin/develop HEAD
git log --oneline --decorate --graph --max-count=80 origin/develop...HEAD
git log --oneline origin/develop..HEAD

# Contribution-style diff: merge base -> source
git diff --stat origin/develop...HEAD
git diff --name-status origin/develop...HEAD
git diff --find-renames origin/develop...HEAD

# Snapshot difference: target tip -> source tip (NOT necessarily MR changes)
git diff --stat origin/develop HEAD
git diff --name-status origin/develop HEAD

# Patch equivalence aid (cherry-picks / rebases); not absolute proof
git cherry -v origin/develop HEAD
```

If no upstream exists, do not treat `@{u}` as defined. Detect a corresponding remote branch carefully; if unclear, ask for the intended source. If a source ref is `origin/feature/example`, compare that ref to the fetched target instead of `HEAD` after the user's selection. When choosing local HEAD, do not accidentally switch to its remote version.

`git diff A...B` is equivalent to `git diff $(git merge-base A B) B` for a single merge base. `git diff A B` compares endpoint trees. `git log A..B` examines commits reachable from B but not A. Their meanings differ. A diff of two trees cannot guarantee a conflict-free final merge result or semantic equivalence.

Use `git diff --numstat` / `--name-status` and inspect per-file patches in manageable batches, including deletions and renames. For file paths, pass `--` followed by safely quoted paths. Do not omit files simply because they are huge; if the environment prevents full coverage, disclose precisely which parts were not inspected.

## Decision checks for adaptive analysis

1. **Normal feature/bugfix/hotfix:** Start with target merge base -> selected source (`target...source`) as the proposed change contribution. Cross-check commit list and endpoint trees.
2. **Release aggregating many features:** Compare release source against updated target; group by real functional differences. Use commit history for orientation, not as the sole source. Check that work already incorporated into target is not represented as new.
3. **Diverged branches:** Identify commits exclusive to both sides (`git log --left-right --oneline target...source`). A two-dot snapshot shows target-only differences too; never report these as changes introduced by the source. Clarify merge outcome uncertainty.
4. **Squash merge / cherry-pick / rebase:** Commit hashes may differ for identical or partially overlapping work. `git cherry -v` and patch-level comparison can help, but may not fully capture semantic equivalence. Do not double-count already-present functionality.
5. **Multiple merge bases / criss-cross:** Report ambiguity and inspect composite behavior rather than assuming a single merge base means a definitive change set.
6. **Shallow or incomplete history:** Check `git rev-parse --is-shallow-repository`. If no reliable merge base, do not silently substitute a snapshot diff and label it an MR diff; ask or qualify any partial summary.
7. **Conflict risk:** Branch divergence or touching the same files alone is not proof of a merge conflict. Report only verified conflicts; otherwise flag "merge outcome not verified" when relevant. Do not perform a mutating merge to check.
8. **No net diff:** If selected source introduces no meaningful changes, say this instead of drafting fictional content. A nonempty commit list can still have no net patch.
9. **Uncommitted content:** `git status --short` may indicate changes; they remain outside the source commit. State this exclusion in Git Analysis, not PR/MR Description.
10. **Binary / LFS / submodule changes:** Metadata may show changes without revealing contents. Inspect available metadata and flag unknown behavior; never infer payload details.
11. **Sensitive material:** Diff output can contain credentials or personal information. Avoid copying raw sensitive hunks or values into final text or logs. Prefer change-category summaries and separately note possible exposure without any secret values.

## Questions that must interrupt generation

- Fetch failed: "`git fetch <remote>` failed. Continue using existing local remote-tracking refs (may be stale), or stop?"
- Unpushed/ahead commits found: "The local branch has N commits not on the fetched remote branch. Summarize local HEAD (including them), or the remote source branch?"
- No reliable remote source: "I cannot verify push status. Use local HEAD, or specify the remote source ref?"
- Ambiguous target/remote: "Which target branch and remote should be used?"
- Unverified ticket IDs: "I found IDs X and Y but cannot verify their details. Which should be included, and what do they refer to?"
- Testing, **always**: "What testing did you perform, and what was each result? You can answer Not performed or Pending."

Ask related questions together where possible. Do not output a final copy-ready description while a required answer is outstanding.
