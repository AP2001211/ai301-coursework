
# Procedure: how this tool grades a PR package

## Read order

Follow this order before assigning any check grades.

1. Determine whether the request is in live mode or eval mode using the definitions in `SKILL.md`.

2. In live mode, read `scope.md` first. Confirm that the repository is configured and the PR belongs to the permitted repository. Refuse to grade if the scope is missing, contains a placeholder, or does not match. Read `voice-guide.md` for later review of the outgoing PR text. In eval mode, ignore both files.

3. Read `rubric.md` and `references/evidence-guide.md`. Identify every check, its weight, pass condition, and required evidence. Refuse to grade if the rubric or procedure has no substantive authored content.

4. Read the original issue and its relevant discussion. In eval mode, use only the issue context included in the bundle. Record the reported problem, expected behavior, constraints, and any maintainer clarifications.

5. Read the implementation plan before the diff. Record:
   - Root cause and intended fix.
   - Files and components expected to change.
   - Explicit scope boundaries and exclusions.
   - Planned reproduction steps and tests.
   - Any documented deviations or omitted work.

6. Read the actual implementation diff. In live mode, use `git diff main...HEAD` from the repository working directory. Record all changed files, substantive hunks, test additions, and observable behavior changes. Do not substitute uncommitted working-tree changes for the committed branch diff.

7. Read the test evidence. Record before/after reproduction results, test commands, actual outcomes, failures, skipped checks, and limitations.

8. Read the PR title and description. Extract each implementation claim, stated limitation, testing claim, issue reference, and reviewer note.

9. Read the repository's PR template, contribution guidelines, and applicable AI-use requirements. In eval mode, use only the standards explicitly included in the bundle's repo-facts information.

Read the plan before the diff to establish the promised scope independently. Read the description after the diff so its claims can be verified rather than accepted at face value.

Do not assign final grades until these sources have been examined.

## Evidence gathering

Build an evidence record for each rubric check. Use the locations defined in `references/evidence-guide.md`.

### 1. Plan fidelity

- Extract the planned implementation and allowed scope from the plan.
- List every substantive file and behavior change from the diff.
- Match each change to a planned item or documented deviation.
- Identify planned work missing from the implementation.
- Check whether omissions or departures are explicitly disclosed.
- Record any unexplained scope expansion, reduction, or contradiction.

### 2. Description accuracy

- Extract the PR title, summary, changes, and outcome claims.
- Match each implementation claim to the corresponding diff hunk.
- Match each testing claim to recorded test evidence.
- Compare stated limitations and deviations with the plan and actual implementation.
- Record unsupported, exaggerated, or contradictory claims.

### 3. Observable test evidence

- Extract the reproduction steps and expected behavior from the issue and plan.
- Find the corresponding before/after observations in the test evidence.
- Identify any regression tests and their recorded results.
- Determine whether the observations support the claimed fix or honestly documented partial outcome.
- Record missing results, unsupported success claims, or evidence unrelated to the issue.

### 4. Repository checks

- Identify each required validation command from the repository's stated rules.
- Find its execution and outcome in the test evidence or PR description.
- Record whether it passed, failed, was skipped, or could not run.
- Compare the actual outcomes with the author's testing claims.
- Distinguish disclosed limitations from hidden or falsely reported failures.

### 5. Diff relevance

- Review the changed-file list and substantive hunks.
- Match each change to the issue, plan, or necessary supporting implementation.
- Identify unrelated refactors, cosmetic changes, or opportunistic cleanups.
- Record the exact files or hunks containing unrelated work.

### 6. Diff reviewability

- Inspect the diff for debugging statements, temporary instrumentation, commented-out code, accidental generated files, and unrelated formatting churn.
- Determine whether such material is necessary to the fix or is avoidable debris.
- Record concrete examples and their locations.
- Do not penalize legitimate tests, necessary supporting edits, or the number of commits alone.

### 7. Repository standards

- Extract mandatory requirements from the PR template, contributing guide, and repo-facts block.
- Check the PR title and description against those requirements.
- Verify required sections, issue references, and applicable submission disclosures.
- Accept equivalent wording when it fulfills the stated requirement.
- Record each missing or contradicted mandatory requirement.

### 8. AI-use disclosure

- Locate the repository's applicable AI-use policy.
- Inspect the PR description, especially Notes for Reviewers.
- Determine whether the disclosure meaningfully explains AI involvement or states that AI was not used.
- Record the relevant policy and disclosure text.
- If no disclosure requirement applies, record that the check is not applicable.

### 9. Communication clarity

- Read the title and description alongside the actual diff and evidence.
- Determine whether a reviewer can identify the purpose, implementation, validation, and limitations.
- Record any material ambiguity that makes the change difficult to understand.
- Ignore minor stylistic preferences unless they affect comprehension.

Do not invent missing evidence or treat an author's assertion as proof of an observable result.

## Check execution

Execute every rubric check in the exact order listed in `rubric.md`.

For each check:

1. Retrieve the evidence already collected for that check.
2. Compare the evidence against the check's written pass condition.
3. Assign exactly one grade:
   - `pass` when the evidence establishes the condition.
   - `fail` when the evidence establishes a violation.
   - `unclear` when necessary evidence is absent or insufficient.
4. Write one concise evidence line naming the decisive fact, observation, quotation, or missing artifact.
5. Preserve the check's exact name and weight from the rubric.

Do not create new checks, change their weights, or introduce unstated standards.

A check may be graded without rereading the entire package when its evidence record contains all facts needed to apply the pass condition. Revisit the source only if the record contains a contradiction or omits a necessary detail.

If evidence is genuinely missing, grade the check `unclear` unless the available evidence directly establishes a failure. Do not assume that a missing test passed or that an undocumented change was authorized.

For explicitly inapplicable requirements, assign `pass` and explain the inapplicability.

Continue through every check even if an earlier required check fails.

Apply the rubric's honest-outcome rules: a disclosed limitation is not automatically a failure, but disclosure cannot substitute for evidence supporting a claimed successful fix or excuse unrelated changes.

## Verdict assembly

1. Collect all check grades in rubric order.

2. Apply the verdict rule from `rubric.md`:
   - Return `accept` only when every required check passes.
   - Return `reject` when at least one required check fails or is unclear.
   - Preferred checks do not affect the verdict.

3. If the verdict is `reject`, identify the first required check graded `fail` or `unclear` in rubric order as the primary blocker.

4. Quote or identify the decisive evidence for that primary blocker in the human-readable summary. Also report every other failed or unclear check so the author can see all outstanding problems.

5. If the verdict is `accept`, identify the strongest evidence supporting readiness, especially plan fidelity, observable testing, and compliance with repository requirements.

6. In live mode, report any voice-guide violations separately. Do not change the verdict solely because of a voice preference unless a rubric check explicitly requires it.

7. Produce a concise per-check summary, followed by the exact fenced JSON schema from `SKILL.md`.

8. Populate the `item` field with the PR URL in live mode or the package identifier in eval mode. Include every rubric check exactly once, in rubric order, with its grade and one-line evidence.

9. Ensure the verdict is exactly `accept` or `reject`. The JSON must be valid, must be the final fenced block, and must have no text after it.

Do not silently change the rubric or procedure to make an individual package pass. Any improvements to the grading rules must be made in the corresponding skill files before rerunning an evaluation.
