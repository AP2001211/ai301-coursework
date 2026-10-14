
# Evidence guide: where evidence lives in a PR package

Use this guide to locate evidence for each rubric check. In eval mode, the supplied package is the complete evidence source. In live mode, use the named files, branch diff, issue thread, and repository documentation.

Do not infer missing facts or treat unsupported claims as verified evidence.

## Plan fidelity (harness category: silent-drift)

### Where it lives

**Eval mode:**
- Plan-context block: issue summary, intended implementation, scope boundaries, exclusions, and documented deviations.
- Candidate PR diff: changed files, modified behavior, added or removed functionality, and test changes.
- Candidate PR description: summary, changes, claimed outcomes, omissions, and deviation disclosures.
- Commit list: additional context about the changes represented in the package.

**Live mode:**
- `plan.md`: diagnosis, implementation approach, scope, exclusions, and `## Deviations`.
- `git diff main...HEAD`: committed implementation changes relative to the base branch.
- `pr_draft.md`: PR title, summary, changes, limitations, and reviewer notes.
- Original GitHub issue and discussion thread: expected behavior, constraints, and maintainer clarifications.

### What good looks like

Every substantive change in the diff falls within the plan's intended scope or is justified by an explicit deviation. Planned work that was omitted is acknowledged rather than silently ignored.

The PR description accurately represents the implemented behavior. A documented, justified change from the plan can be acceptable; an undisclosed expansion, omission, or contradictory success claim is silent drift.

### Warning signs

- A changed file or behavior has no relationship to the plan or documented deviations.
- The description claims functionality that the diff does not implement.
- Planned functionality is missing without an explanation.
- The author claims complete plan fidelity despite visible departures.
- A deviation is disclosed vaguely without identifying what changed.

**Related rubric checks:** Plan fidelity; Description accuracy.

## Test evidence (harness category: not-tested)

### Where it lives

**Eval mode:**
- Plan-context block: reproduction steps, expected behavior, planned regression tests, and validation strategy.
- Candidate PR test-evidence section: before/after observations, test commands, actual outputs, failures, and limitations.
- Candidate PR description: testing claims, checked boxes, and explanations of skipped or failed checks.
- Repo-facts block: required validation commands and testing expectations.

**Live mode:**
- `plan.md`: planned tests and expected behavior.
- Unit 2 reproduction report or supplied house reproduction package: original reproduction steps and observed failure.
- `test_evidence.md`: captured before/after results and validation command outputs.
- `pr_draft.md`: testing section and reported outcomes.
- `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`: required checks and testing instructions.

### What good looks like

The evidence identifies the relevant behavior, shows an observable result, and explains how that result compares with the expected outcome. A successful fix is supported by before/after reproduction evidence, a relevant regression test, or other concrete observations appropriate to the issue.

The repository's required checks have recorded outcomes. Failed, skipped, or unavailable checks are disclosed accurately rather than omitted or described as passing.

### Warning signs

- The description says "tests pass" without supporting results.
- Test commands are listed without any observable outcome.
- Evidence covers unrelated behavior instead of the reported issue.
- A required repository check is missing without explanation.
- The author claims the issue is fixed despite contradictory test output.
- A failing or skipped test is represented as successful.

A reported limitation does not automatically mean the PR must be rejected. Evaluate whether the evidence supports the actual claim and whether the repository's requirements have been satisfied.

**Related rubric checks:** Observable test evidence; Repository checks; Description accuracy.

## Diff quality (harness category: unreviewable)

### Where it lives

**Eval mode:**
- Candidate PR unified diff: changed files, hunks, additions, deletions, and surrounding context.
- Candidate PR commit list: commit subjects and the scope of included changes.
- Plan-context block: intended files, implementation boundaries, and documented deviations.
- Candidate PR description: explanations of supporting changes.

**Live mode:**
- `git diff main...HEAD`: complete committed diff.
- `git diff main...HEAD --stat`: changed-file summary.
- `git log main..HEAD --oneline`: commits introduced by the branch.
- `plan.md`: planned changes and documented deviations.
- `pr_draft.md`: explanation of implementation choices.

### What good looks like

The diff contains changes necessary to address the issue, including appropriate supporting tests and documentation. A reviewer can identify the fix without separating it from unrelated work or temporary artifacts.

The number of files, lines, or commits is not itself a failure. Judge whether the actual changes are relevant and reviewable.

### Warning signs

- Debug prints or temporary instrumentation remain in production code.
- Large commented-out blocks or dead code are included.
- Unrelated formatting changes obscure the functional fix.
- Opportunistic refactors or unrelated cleanups are bundled with the issue.
- Generated files or local artifacts are accidentally committed.
- A broad unrelated change is defended only by a description note.

Disclosure does not automatically justify unrelated changes. Necessary supporting edits are acceptable when their relationship to the fix is evident.

**Related rubric checks:** Diff relevance; Diff reviewability.

## Standards and comms (harness category: standards-wall)

### Where it lives

**Eval mode:**
- Repo-facts block: PR template sections, contribution requirements, stated policies, and AI-use disclosure rules.
- Candidate PR title and description: required content, issue references, testing disclosures, and reviewer notes.
- Included issue or maintainer context: explicit instructions relevant to the submission.

**Live mode:**
- `.github/PULL_REQUEST_TEMPLATE.md`: required PR sections and checkboxes.
- `docs/CONTRIBUTING.md`: contribution process, tests, conventions, and stated policies.
- `scope.md`: applicable repository and scope rules.
- Original issue thread: explicit maintainer directions.
- `pr_draft.md`: completed title and description, issue reference, testing details, and Notes for Reviewers.
- `voice-guide.md`: personal communication rules applied only in live mode.

### What good looks like

The PR fulfills the repository's explicit submission requirements. Mandatory template sections contain meaningful information, the issue is correctly referenced, and applicable policies are followed.

When AI-use disclosure is required, the description meaningfully states how AI was used or explicitly states that it was not used. Equivalent wording is acceptable when it satisfies the requirement.

### Warning signs

- Required PR template sections are missing or left as placeholders.
- The description omits a required issue reference.
- Mandatory testing or reviewer notes are ignored.
- The repository requires AI-use disclosure but none is provided.
- A generic statement about AI does not satisfy the stated disclosure policy.
- Explicit maintainer instructions are disregarded without explanation.
- Boilerplate is present without the information the template requests.

Do not invent repository requirements. In eval mode, use only the policies and template requirements included in the package. In live mode, verify them from the actual repository.

The description's factual consistency with the diff belongs under Plan fidelity, not merely communication quality.

**Related rubric checks:** Repository standards; AI-use disclosure; Communication clarity.
