
# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Compare the diff's changed files, behavior, and scope against the plan's intended implementation, stated boundaries, and `## Deviations` notes. Compare both against the original issue. | Every substantive change is justified by the plan or an explicitly documented deviation. Any omitted planned work is disclosed. The implementation addresses the issue without silently expanding or reducing the promised scope. | required |
| Description accuracy | Compare the PR title, summary, change list, and stated outcomes against the actual diff, plan, and test evidence. | Every material implementation claim is supported by the diff or evidence. The description accurately identifies what changed, what remains incomplete, and any deviations. No claim contradicts the implementation. | required |
| Observable test evidence | Compare the plan's reproduction steps and test plan against the before/after observations and command outputs in the test evidence. | Evidence shows an observable result relevant to the issue, such as a before/after behavior difference, regression test result, or explicitly documented limitation. A claimed successful fix must have supporting observations. Commands alone, without meaningful results, do not establish success. | required |
| Repository checks | Compare the repository's required validation commands from the repo-facts block, PR template, or contributing guide against the recorded test evidence and PR testing claims. | Every applicable required check has a recorded execution and outcome, or an explicitly disclosed reason it could not be completed. Failures and unavailable checks are reported accurately rather than hidden or represented as passing. | required |
| Diff relevance | Compare all changed files and hunks against the issue, implementation plan, and documented deviations. | Every substantive change serves the issue or is a necessary supporting change. Unrelated refactors, opportunistic cleanups, or changes outside the issue's purpose are absent, even when disclosed. | required |
| Diff reviewability | Inspect the diff for debug statements, temporary instrumentation, commented-out code, generated artifacts, accidental formatting changes, and other review debris. | The diff contains no avoidable debris that obscures the intended implementation or makes its behavior difficult to review. Necessary supporting changes and legitimate test additions are allowed. | required |
| Repository standards | Compare the PR title and description against the repository's PR template, contribution rules, and explicit submission requirements in the repo-facts block. | All applicable mandatory repository requirements are satisfied, including required sections, issue references, and requested disclosures. Equivalent wording or organization is acceptable when it fulfills the actual requirement. | required |
| AI-use disclosure | Compare the PR description, particularly Notes for Reviewers, against the repository's stated AI-use policy. | When AI-use disclosure is required, the description explicitly and meaningfully explains how AI was used, or states that it was not used. Generic acknowledgments that do not satisfy the policy fail. If the repository has no applicable disclosure requirement, this check passes as not applicable. | required |
| Communication clarity | Read the PR title and description alongside the issue, diff, and test evidence. | A maintainer can identify the purpose, important changes, validation outcomes, and known limitations without having to infer the author's intent. Minor wording or formatting preferences do not cause failure. | preferred |

## Verdict rule

Grade every check independently as `pass`, `fail`, or `unclear`, using the evidence and pass condition in its row.

- `pass`: The available evidence establishes that the stated pass condition is satisfied.
- `fail`: The evidence establishes that the condition is violated, or a required claim is contradicted by the artifacts.
- `unclear`: The available evidence is insufficient to determine whether the condition is satisfied. Identify the missing or ambiguous evidence explicitly.

Return `accept` if and only if every `required` check receives `pass`.

Return `reject` if any `required` check receives `fail` or `unclear`.

A `preferred` check never changes the verdict, regardless of its grade.

Do not stop grading after the first failure. Execute every check and report its result.

Apply these interpretation rules consistently:

1. An explicitly documented deviation is not silent drift. Judge whether the implementation and description faithfully reflect the revised scope.
2. An honestly disclosed limitation, incomplete item, failed check, or unavailable test is not automatically a rejection. Determine whether the evidence supports the actual claims and whether the repository's explicit requirements have been met.
3. A claimed successful fix without observable supporting evidence fails the observable test evidence check, even if the description is polished.
4. A statement that tests passed does not substitute for recorded results when those results are required.
5. Unrelated changes are not excused merely because the author disclosed them.
6. Missing mandatory repository sections or disclosures fail the relevant standards check even when the code itself is correct.
7. Do not introduce unstated standards, require arbitrary quantities of tests, or reject an otherwise complete PR for cosmetic preferences.
8. In eval mode, use only the supplied package and its repo-facts block. Do not assume additional repository policies or external requirements.
9. When a requirement is explicitly inapplicable, grade it `pass` and state why in the evidence line.

The verdict must be exactly `accept` or `reject`. Do not return a numerical score or conditional acceptance.
