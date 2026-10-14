
---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Determine whether exactly one candidate pull request is ready to submit.

Treat a PR package as the proposed title and description, implementation diff, commits, test evidence, and implementation plan, evaluated against the original issue and repository requirements.

Check whether the implementation follows the plan, the tests support the claims, the changes are reviewable, and the submission satisfies the repository's stated standards.

Do not implement fixes, rewrite the PR, or grade multiple packages in one run. Report readiness based on evidence rather than confidence or presentation quality.

## Inputs and modes

Determine the mode before gathering evidence.

**Live mode**

Grade the student's own PR using:

1. `plan.md`, including its original scope, test plan, and any `## Deviations` section.
2. The committed branch diff relative to `main`, obtained with `git diff main...HEAD` from the repository working directory.
3. The draft PR title and description from `pr_draft.md`.
4. Test results, reproduction output, and before/after comparisons from `test_evidence.md`.
5. The original issue URL, including the issue description and discussion thread.
6. The repository's `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`, including applicable contribution and AI-disclosure requirements.

Inspect commit history when necessary to understand the submitted changes. Do not treat uncommitted working-tree changes as part of the submitted diff.

For a house-chain submission, use the supplied house plan, reproduction package, and working branch instead of the student's corresponding artifacts. Apply the same rubric and verdict rule.

**Eval mode**

Read only the supplied practice package bundle. Treat its contents as the complete evidence universe.

Do not fetch GitHub pages, inspect local repositories, consult external standards, or fill gaps using assumptions.

Run every rubric check and apply the full verdict rule, even when an early required check fails.

Do not reject a package merely because its evidence is represented as a frozen snapshot rather than live files.

## The scope seam (live mode only)

Before reading or grading any live PR evidence, read `scope.md`.

Identify the permitted repository from its `Repo:` line and apply the repository rules recorded in that file.

If the repository value is missing or still contains a placeholder, stop without grading and instruct the student to complete the `Repo:` line.

If the PR belongs to a repository outside the configured scope, refuse to grade it. Never guess or silently substitute another repository.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

Read `voice-guide.md` and evaluate the outgoing PR title and description against its communication rules.

Report each violated rule with the relevant wording and a concise explanation in the human-readable summary.

Do not allow voice preferences alone to change the final verdict unless a specific rubric check makes the violation relevant.

In eval mode, ignore `voice-guide.md` entirely. Apply only the universal communication and reviewability standards defined in the rubric.

## Component reads

Read and use the skill components as follows:

1. Read `rubric.md` to identify the named checks, required evidence, pass conditions, weights, and final verdict rule.
2. Read `references/evidence-guide.md` to locate each evidence family in the live artifacts or evaluation package.
3. Read `procedure.md` and execute its instructions in the stated order, including evidence collection, check execution, and verdict assembly.
4. In live mode, also apply the scope and voice rules described above.

If `rubric.md` or `procedure.md` contains no substantive authored instructions, refuse to grade. Template comments and placeholder headings do not count as content.

If a procedure step is missing or ambiguous, identify and report that gap. Do not invent an alternative procedure or silently bypass it.

Do not create additional grading checks at runtime. Grade only the checks defined in `rubric.md`.

## Verdict and output

Return exactly one binary verdict:

- `accept`: the PR meets the rubric's readiness requirements and can be submitted.
- `reject`: the PR does not meet those requirements and should be held for correction.

Apply the verdict rule in `rubric.md` exactly. Never invent a third verdict, numerical score, or conditional acceptance.

Provide a concise human-readable summary identifying each check, its grade, and the evidence supporting that grade. For rejected submissions, identify the specific blockers without rewriting the submitted work.

End every completed grading response with the following fenced JSON block. Preserve its schema, field order, and field names. Replace placeholders with actual values and include every rubric check exactly once.

Nothing may follow the JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

**Evidence first.** Support every check with a concrete fact, file reference, diff observation, command result, or quotation. Never use statements such as "looks good" as evidence.

**Grade the artifact, not its polish.** A concise but complete PR may pass. A polished description cannot compensate for missing tests, undocumented changes, or unsupported claims.

**The rubric decides.** Apply each check's written pass condition and weight consistently. Do not override the rubric based on intuition or a preferred outcome.

**The procedure decides how.** Follow `procedure.md` as written. Report procedural gaps rather than inventing missing steps during execution.

**Compare claims against evidence.** Verify that the PR description matches the actual diff, the diff matches the plan or documented deviations, and the test evidence demonstrates the claimed behavior.

**Treat uncertainty explicitly.** Use `unclear` when the available evidence cannot establish whether a condition passes. Apply the rubric's stated handling of `unclear`; if the verdict rule is silent, treat unverifiable claims as failing rather than assuming success.

**Do not conceal failures.** Report failing commands, missing evidence, unrelated edits, and unmet repository requirements. Distinguish an honestly disclosed limitation from an undisclosed contradiction.

**Remain within scope.** Evaluate submission readiness only. Do not implement code changes, manufacture evidence, or assume tests passed without recorded results.
