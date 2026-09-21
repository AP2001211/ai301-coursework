# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo-facts block: last 5 default-branch commits and maintainer first-response sample; issue Comments section for Owner, Member, or Collaborator activity. | Pass if at least one of the last 5 default-branch commits is human-authored within 90 days of the capture date, OR the maintainer first-response sample contains a response within 30 days, OR an Owner, Member, or Collaborator commented on this issue within 90 days. | required |
| repo-in-use | Repo-facts block: archived flag, latest release, and last push to any branch. | Pass if the repository is not archived AND either its latest release or its last push to any branch is within 180 days of the capture date. | required |
| newcomer-scope | Issue body and Comments section, including linked or mentioned prior PR attempts. | Pass if the issue asks for a concrete contribution rather than a usage/support question; is not explicitly identified as an umbrella or tracking issue whose sub-items are intended to be handled as separate contributions; has no unresolved design debate unless a maintainer has settled the direction; and has no maintainer statement that the fix requires changes to core internals. Multiple files, checklist items, or implementation steps that contribute to one stated outcome do not by themselves make the issue an umbrella issue. | required |
| unclaimed | Repo-facts block: this issue's assignees and linked PRs; Comments section for claims and PR mentions. | Pass if the issue has no assignee, no open linked or comment-mentioned PR implementing it, and no comment stating that someone is currently working on it. Closed unmerged PRs and comments that clearly say the contributor stopped working on it do not count as active claims. | required |
| contribution-policy | Repo-facts block: contribution policy, including CONTRIBUTING.md or .github contributor docs, dedicated AI policy files, and requirements surfaced by issue or PR templates. | Pass if the policy does not explicitly ban AI-generated or AI-assisted contributions. Conditions such as disclosure, personal understanding, testing, or human review pass. If no AI contribution policy is stated, pass. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. Treat unclear as fail for required checks. Preferred checks, if added later, never change the verdict and only rank accepted issues.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->
