# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_matches_repro | The plan's stated cause read against the reproduced behavior, observations, and evidence in the repro evidence. | Passes if the proposed cause is supported by the reproduced evidence and does not contradict what the reproduction shows. | required |
| change_targets_cause | The plan's implementation approach read against its diagnosis and the reproduced failure. | Passes if the proposed change addresses the diagnosed cause rather than only masking or working around the observed symptom. | required |
| bounded_scope | The plan's scope, non-scope, files to touch, and proposed changes. | Passes if the work is one bounded change needed to fix the reproduced problem and does not introduce unrelated work or unnecessary scope. | required |
| executable_approach | The plan's files-to-touch and implementation approach read with the repo facts and stated conventions. | Passes if another contributor could begin implementing the change from the plan and the proposed locations and actions are consistent with the available repo evidence. | required |
| test_proves_fix | The plan's test plan read against the repro evidence's inputs, steps, and observable failure. | Passes if the planned verification exercises the reproduced failure through the relevant code path and states an observable post-fix result that would distinguish fixed from still broken. | required |
| uncertainty_is_honest | The plan's risks, unknowns, assumptions, and implementation claims read against the available evidence. | Passes if unresolved facts that could affect the implementation are identified as unknowns or assumptions rather than presented as established facts. | required |
| thread_and_conventions | The draft plan comment read against the issue thread highlights and the plan read against the repo-facts block and stated repository conventions. | Passes if the proposed work respects relevant maintainer requests, issue constraints, and repository conventions, with no material conflict left unexplained. | required |

## Verdict rule

Accept only if every required check passes. A fail on any required
check results in reject. Treat unclear as a fail for required checks
because a plan is not ready to post or build from when the evidence is
insufficient to determine whether a required condition is satisfied.
Preferred checks, if any are added later, do not change the verdict.



<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
