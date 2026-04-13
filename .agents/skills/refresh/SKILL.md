---
name: refresh
description: Merge latest master into current branch and update PR title/description if stale.
disable-model-invocation: true
long_description: |
  Merge latest master into current branch and update PR title/description if stale.
---

# Refresh Branch

## Step 1: Merge Default Branch

```bash
DEFAULT_BRANCH=$(git remote show origin | awk '/HEAD branch/ {print $NF}')
if [ -z "$DEFAULT_BRANCH" ]; then
  DEFAULT_BRANCH=main
fi

git fetch origin "$DEFAULT_BRANCH"
git merge "origin/$DEFAULT_BRANCH"
```

If there are conflicts, resolve them. Prefer the current branch's intent. Ask the user if a conflict is ambiguous.

## Step 2: Update PR

Check if a PR exists (`gh pr view`). If so, compare the title and description against the actual branch content
(`git log origin/$DEFAULT_BRANCH..HEAD`, `git diff origin/$DEFAULT_BRANCH...HEAD --stat`).
Use the same shell variable from above when running the checks.
Update with `gh pr edit` if stale. Follow the PR format from the github skill.
