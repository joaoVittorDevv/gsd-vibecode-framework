---
name: deliver-integration
description: "Documents how DELIVER principles from Vibe Code Framework integrate into existing GSD execute-phase workflow. DELIVER is not a separate agent — it modifies existing workflows to add V.E.R.I.F.Y., S.E.C.U.R.E., S.H.I.E.L.D. as mandatory Layer Checks before commit."
version: "1.0.0"
owner: "gsd-vibecode-framework"
tags: ["deliver", "layer-checks", "execute-phase", "vibe-code", "quality-gates"]
created: "2026-04-13"
---

# DELIVER Integration — GSD Execute-Phase

## Overview

**DELIVER is not a new agent or workflow.** It is the integration of Vibe Code Framework Layer Checks (V.E.R.I.F.Y., S.E.C.U.R.E., S.H.I.E.L.D.) into existing GSD execution workflows. The execute-phase becomes the DELIVER orchestrator.

**Core principle: Commit ONLY. NO new branch creation per task.**

---

## DELIVER Phases Mapped to GSD Workflows

| DELIVER | GSD Workflow | Action |
|---------|-------------|--------|
| **D — Define** | Planning phase | Already done by gsd-plan-phase. No branch creation per task. |
| **E — Execute** | execute-phase | gsd-executor handles implementation from prompt. |
| **L — Layer Checks** | execute-phase | V.E.R.I.F.Y. → S.E.C.U.R.E. → S.H.I.E.L.D. called post-implementation. |
| **I — Improve** | execute-phase | R.E.F.A.C.T. called after Layer Checks pass. |
| **V — Validate** | execute-phase | C.L.E.A.R. called after R.E.F.A.C.T. |
| **E — Export Docs** | execute-phase | D.O.C.S. applied proportionally to complexity. |
| **R — Release** | execute-phase | **Commit ONLY.** No branch creation. No push. |

---

## Layer Checks — V.E.R.I.F.Y. + S.E.C.U.R.E. + S.H.I.E.L.D.

All three are **mandatory** after task implementation and **before** any commit.

### Layer 1: V.E.R.I.F.Y. — Functional Verification

**When:** After task implementation, before S.E.C.U.R.E.

**What it checks:**
- Does the code do what the prompt asked?
- Are all methods implemented with correct signatures?
- Are types correct (Python 3.10+ unions, Pydantic v2)?
- Are there tests for main behavior?
- Are edge cases handled (None, empty list, nonexistent ID)?
- Is logging adequate at critical points?
- Are imports correct without circular dependencies?

**If V.E.R.I.F.Y. fails:** Fix before proceeding to S.E.C.U.R.E.

### Layer 2: S.E.C.U.R.E. — Security Audit

**When:** After V.E.R.I.F.Y. passes.

**What it checks:**
- Inputs validated before use (especially user-supplied IDs, dicts)?
- No SQL/NoSQL injection ($where, eval in queries)?
- Sensitive data not logged (tokens, passwords, api_keys)?
- Endpoints protected with correct auth/authorization?
- Errors do not expose stack traces to clients?
- New dependencies verified for known vulnerabilities?

**If S.E.C.U.R.E. fails:** Fix before proceeding to S.H.I.E.L.D.

### Layer 3: S.H.I.E.L.D. — Transversal Security Hardening

**When:** Applied throughout, confirmed after S.E.C.U.R.E.

**What it checks:**
- Secure by Design: architecture secure by default?
- Hardening: no insecure default values?
- Injection Prevention: all external inputs sanitized?
- Encryption: data in transit and at rest protected?
- Least Privilege: minimal necessary permissions?
- Defence-in-Depth: multiple layers of protection applied?

**If S.H.I.E.L.D. fails:** Fix before R.E.F.A.C.T.

---

## Post-Layer Checks: R.E.F.A.C.T. + C.L.E.A.R.

### R.E.F.A.C.T. — Refactoring (Improve)

Applied after all Layer Checks pass.

1. **R — Recognise:** Identify anti-patterns (giant functions, mixed responsibilities, generic naming)
2. **E — Extract:** Extract components with clear responsibilities if needed
3. **F — Format:** Align naming and organization with existing codebase
4. **A — Address:** Cover any edge cases not yet addressed
5. **C — Confirm:** Verify tests still pass after refactoring
6. **T — Tune:** Resolve obvious bottlenecks (N+1, unnecessary loops)

**Rule:** Refactor only code in the current task scope — do not drift.

### C.L.E.A.R. — Code Review (Validate)

Applied after R.E.F.A.C.T.

1. **C — Context:** Does the code make sense in the module/architecture context?
2. **L — Layered Examination:** Review in layers: architectural > design > implementation
3. **E — Explicit Verification:** Check dangerous absences (missing validation, missing error handling)
4. **A — Alternative Consideration:** Is there a simpler or more robust approach?
5. **R — Refactoring Recommendations:** Record what can improve in future iterations

**If C.L.E.A.R. identifies critical problems:** Fix now and run R.E.F.A.C.T. again.

---

## Commit-Only Release

**NO new branch creation per task.**

1. All Layer Checks pass (V.E.R.I.F.Y. → S.E.C.U.R.E. → S.H.I.E.L.D.)
2. R.E.F.A.C.T. applied and verified
3. C.L.E.A.R. review passed
4. Commit on existing branch with atomic, descriptive message

**Commit format:**
```
<type>(<scope>): <short description>

<longer description if needed>

Task: TASK-X.X.X | Story: STORY-X.X | Epic: EPIC-X
```

**Types:** `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

---

## Flow Summary

```
Task Implementation
       |
       v
  V.E.R.I.F.Y. (functional)
       |
       v
  S.E.C.U.R.E. (security audit)
       |
       v
  S.H.I.E.L.D. (transversal hardening)
       |
       v
  R.E.F.A.C.T. (refactor)
       |
       v
  C.L.E.A.R. (review)
       |
       v
   COMMIT (on existing branch)
```

---

## Key Principles

1. **DELIVER is integrated, not a separate agent.** It modifies existing execute-phase behavior.
2. **Commit ONLY.** No new branches per task. Work continues on the existing feature branch.
3. **Layer Checks are sequential gates.** Skip none. Fix before proceeding.
4. **R.E.F.A.C.T. and C.L.E.A.R. run after Layer Checks**, not instead of them.
5. **D.O.C.S. is proportional.** Document complexity commensurate with task complexity.

---

## Relationship to Existing GSD Workflows

- **execute-phase.md:** Becomes the DELIVER orchestrator. It calls V.E.R.I.F.Y., S.E.C.U.R.E., S.H.I.E.L.D., R.E.F.A.C.T., C.L.E.A.R. as inline steps.
- **execute-plan.md:** Executes individual tasks. Each task commits only after all DELIVER gates pass.
- **verify-phase.md:** Validates the phase goal. Layer Checks ensure quality at task level.
- **plan-phase.md:** Defines tasks. DELIVER does not change planning.

---

## References

- V.E.R.I.F.Y. — `vibe-framework/skills/verify-generated-code/`
- S.E.C.U.R.E. — `vibe-framework/skills/secure-generated-code/`
- S.H.I.E.L.D. — `vibe-framework/skills/shield-security-layer/`
- R.E.F.A.C.T. — `vibe-framework/skills/refact-code/`
- C.L.E.A.R. — `vibe-framework/skills/clear-code-review/`
- D.O.C.S. — `vibe-framework/skills/docs-knowledge-preservation/`
- DELIVER-SKILL — `vibe-framework/skills/DELIVER-SKILL.md`
