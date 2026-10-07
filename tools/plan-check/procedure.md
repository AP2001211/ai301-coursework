# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->
## Read order

1. Read the repro evidence first. Record the reproduced behavior, the
   inputs and steps that trigger it, the observable failure, and any
   evidence that identifies or constrains the cause. Do not use the
   plan's diagnosis to reinterpret the reproduction.

2. Read the issue thread highlights. Record maintainer requests,
   constraints, clarifications, and any stated expectations for the
   proposed change.

3. Read the repo-facts block. Record the relevant files, code
   locations, test locations, and repository conventions that can be
   used to verify claims in the plan.

4. Read the candidate plan. Record its diagnosis, proposed scope and
   non-scope, files to touch, implementation approach, test plan,
   risks, unknowns, and assumptions.

5. Read the draft plan comment last. Record what it tells maintainers
   about the cause, scope, approach, verification, and any unresolved
   uncertainty. Compare it with the plan and the previously recorded
   thread constraints rather than treating it as independent evidence.

## Evidence gathering

1. For `diagnosis_matches_repro`, pair the plan's stated cause with
   the reproduced behavior and causal evidence recorded from the repro
   evidence. Record whether the reproduction supports, contradicts, or
   does not establish the claimed cause.

2. For `change_targets_cause`, pair the proposed implementation action
   with the plan's diagnosis and reproduced failure. Record what part
   of the diagnosed cause each proposed change is intended to alter.

3. For `bounded_scope`, collect the plan's proposed changes, non-scope,
   and files to touch. Compare them with the reproduced problem and
   record any proposed work that is not necessary to address that
   problem.

4. For `executable_approach`, collect each implementation action and
   file or code location it depends on. Compare those claims with the
   repo facts and conventions. Record whether the available evidence
   gives another contributor a concrete place and action from which to
   start.

5. For `test_proves_fix`, pair the test plan with the reproduction's
   triggering inputs, steps, code path, and observable failure. Record
   the planned post-fix observation and whether it would distinguish
   the fixed behavior from the reproduced failure.

6. For `uncertainty_is_honest`, collect the plan's risks, unknowns,
   assumptions, and factual implementation claims. Compare claims that
   affect the implementation with the repro evidence, thread
   highlights, and repo facts. Record unsupported material claims that
   are presented as certain.

7. For `thread_and_conventions`, collect relevant maintainer requests
   and issue constraints from the thread highlights and relevant
   conventions from the repo facts. Compare them with both the plan
   and draft comment. Record any material conflict or ignored
   constraint.

## Check execution

1. Grade the checks in this order:
   `diagnosis_matches_repro`, `change_targets_cause`,
   `bounded_scope`, `executable_approach`, `test_proves_fix`,
   `uncertainty_is_honest`, and `thread_and_conventions`.

2. For each check, use only the evidence gathered for that check and
   apply the pass condition in `rubric.md`. Do not substitute a
   different requirement because it seems preferable.

3. Grade a check `pass` when the gathered evidence establishes its
   pass condition. Grade it `fail` when the evidence establishes that
   the pass condition is violated.

4. Grade a check `unclear` when evidence required to decide the check
   is genuinely absent or insufficient and neither pass nor fail can
   be established. Do not use `unclear` merely because the plan uses
   different wording or organization than expected.

5. When a check fails or is unclear, record the specific conflicting
   or missing evidence. Do not fail a check solely because the plan
   lacks a particular heading, section length, format, or automated
   test unless that absence prevents the check's actual outcome from
   being established.

6. Reuse the gathered evidence across checks when the same fact is
   relevant. Re-read the source only when the recorded evidence is
   insufficient to apply a check's pass condition.

## Verdict assembly

1. After every check has a grade, apply the verdict rule from
   `rubric.md` exactly.

2. Return `accept` only when every required check is `pass`.

3. Return `reject` when any required check is `fail` or `unclear`.
   Treat `unclear` as a failure for required checks because the plan
   has not established that it is ready to post and build from.

4. Preferred checks, if any are added to the rubric, may be reported
   but never change the final verdict.

5. For each non-passing required check, quote or identify the smallest
   specific piece of package evidence that caused the `fail` or
   `unclear` grade and explain its conflict with the check's pass
   condition. If several required checks do not pass, report each one;
   do not choose a different verdict based on which failure seems most
   important.

6. Emit the final verdict using the skill's required binary value:
   `accept` for ready and `reject` for hold.

