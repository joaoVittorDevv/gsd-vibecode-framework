<purpose>
Create a pull request from completed phase/milestone work, generate a rich PR body from planning artifacts, optionally run code review, and prepare for merge. Closes the plan → execute → verify → ship loop.
</purpose>

<required_reading>
Read all files referenced by the invoking prompt's execution_context before starting.
</required_reading>

<process>

<step name="initialize">
Parse arguments and load project state:

```bash
INIT=$(node "$HOME/.claude/get-shit-done/bin/gsd-tools.cjs" init phase-op "${PHASE_ARG}")
if [[ "$INIT" == @file:* ]]; then INIT=$(cat "${INIT#@file:}"); fi
```

Parse from init JSON: `phase_found`, `phase_dir`, `phase_number`, `phase_name`, `padded_phase`, `commit_docs`.

Also load config for branching strategy:
```bash
CONFIG=$(node "$HOME/.claude/get-shit-done/bin/gsd-tools.cjs" state load)
```

Extract: `branching_strategy`, `branch_name`.

Detect base branch for PRs and merges:
```bash
BASE_BRANCH=$(node "$HOME/.claude/get-shit-done/bin/gsd-tools.cjs" config-get git.base_branch 2>/dev/null || echo "")
if [ -z "$BASE_BRANCH" ] || [ "$BASE_BRANCH" = "null" ]; then
  BASE_BRANCH=$(git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's|^refs/remotes/origin/||')
  BASE_BRANCH="${BASE_BRANCH:-main}"
fi
```
</step>

<step name="preflight_checks">
Verify the work is ready to ship:

1. **Verification passed?**
   ```bash
   VERIFICATION=$(cat ${PHASE_DIR}/*-VERIFICATION.md 2>/dev/null)
   ```
   Check for `status: passed` or `status: human_needed` (with human approval).
   If no VERIFICATION.md or status is `gaps_found`: warn and ask user to confirm.

2. **Clean working tree?**
   ```bash
   git status --short
   ```
   If uncommitted changes exist: ask user to commit or stash first.

3. **On correct branch?**
   ```bash
   CURRENT_BRANCH=$(git branch --show-current)
   ```
   If on `${BASE_BRANCH}`: warn — should be on a feature branch.
   If branching_strategy is `none`: offer to create a branch now.

4. **Commit Metadata Preparation**
   Identify the semantic type (feat, fix, refactor, etc) and scope (feature name) from the phase context.
</step>

<step name="build_semantic_commit">
Based on the phase objective and summary, generate a semantic commit message inside your mind following strictly this pattern:
`[<nome-branch-base>] <padrao-semantico>(<funcionalidade>): <descricao-do-commit>`

Example targeted to main branch:
`[main] feat(login): Ajuste simples no login`

Determine the variables:
- nome-branch-base: It's `${BASE_BRANCH}`
- padrao-semantico: feat, fix, chore, refactor, etc.
- funcionalidade: the core feature name.
- descricao-do-commit: A short, concise summary.

Use AskUserQuestion to confirm this commit message with the user before executing.
```
AskUserQuestion:
  question: "Confirm semantic commit message?"
  options:
    - label: "Approve commit message"
      description: "Approve the generated semantic commit message"
    - label: "Suggest another"
      description: "I will provide my own description"
```
If user provides their own, update the commit message.
</step>

<step name="squash_merge_and_commit">
Perform the squash merge and commit with the approved message:

```bash
# Swich to base branch
git checkout ${BASE_BRANCH}

# Squash merge the phase branch
git merge --squash ${CURRENT_BRANCH}

# Commit
git commit -m "COMPUTED_COMMIT_MESSAGE"
```

Report: "Merged and committed to `${BASE_BRANCH}`"
</step>

<step name="push_branch">
(Optional) Push the updated base branch to remote:

```bash
git push origin ${BASE_BRANCH} 2>&1
```
</step>

<step name="track_shipping">
Update STATE.md to reflect the shipping action:

```bash
node "$HOME/.claude/get-shit-done/bin/gsd-tools.cjs" state update "Last Activity" "$(date +%Y-%m-%d)"
node "$HOME/.claude/get-shit-done/bin/gsd-tools.cjs" state update "Status" "Phase ${PHASE_NUMBER} shipped and merged to ${BASE_BRANCH}"
```

If `commit_docs` is true:
```bash
node "$HOME/.claude/get-shit-done/bin/gsd-tools.cjs" commit "docs(${padded_phase}): ship phase ${PHASE_NUMBER} merged to ${BASE_BRANCH}" --files .planning/STATE.md
```
</step>

<step name="report">
```
───────────────────────────────────────────────────────────────

## ✓ Phase {X}: {Name} — Shipped

Merged Branch: {branch} → ${BASE_BRANCH}
Semantic Commit: [${BASE_BRANCH}] {type}({scope}): {desc}
Verification: ✓ Passed
Requirements: {N} REQ-IDs addressed

Next steps:
- /gsd-complete-milestone (if last phase in milestone)
- /gsd-progress (to see what's next)

───────────────────────────────────────────────────────────────
```
</step>
</process>

<offer_next>
After shipping:

- /gsd-complete-milestone — if all phases in milestone are done
- /gsd-progress — see overall project state
- /gsd-execute-phase {next} — continue to next phase
</offer_next>

<success_criteria>
- [ ] Preflight checks passed (verification, clean tree, branch)
- [ ] Semantic commit message formulated and approved by user
- [ ] Squash merge to base branch executed with semantic commit
- [ ] STATE.md updated with shipping status
- [ ] Base branch optionally pushed to remote
</success_criteria>
