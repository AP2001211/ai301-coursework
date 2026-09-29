# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | Repro report's environment record read against the issue context and its stated affected environment/version. Use the Environment section of `references/evidence-guide.md`. | Pass if the report identifies the relevant software/version and platform or other environment details needed to interpret the reproduction, and any meaningful difference from the issue's stated environment is explicitly called out. | required |
| steps-followable | Repro report's setup, inputs, commands, and actions from starting state through the attempted trigger. Use the Steps section of `references/evidence-guide.md`. | Pass if a stranger with the repository can follow the provided setup, input, and commands/actions from a defined starting state to the attempted trigger without guessing an essential step or value. | required |
| behavior-matched | Repro report's actual input and output/log/error artifacts read directly against the input and behavior described by the issue. Use the Behavior shown section of `references/evidence-guide.md`. | Pass if a claimed reproduction preserves the issue's relevant trigger conditions and the observed artifact demonstrates the same reported behavior or failure mode; a different error does not pass merely because the same component fails. For a cannot-reproduce result, pass if the report makes a concrete, relevant attempt to exercise the reported trigger, shows the observed result, and explicitly identifies any known difference or unconfirmed precondition that could explain why the behavior did not occur.| required |
| outcome-honest | Candidate claim comment and repro report's stated actual result/conclusion read against the artifacts produced by the reproduction. Use the Honesty section of `references/evidence-guide.md`. | Pass if the conclusion says only what the evidence establishes: a matching reproduction is described as reproduced, and a non-match or cannot-reproduce is reported as such. Fail if the report claims the issue was reproduced when its artifacts show a different behavior or do not establish the claim. | required |
| repo-conventions | Repo-facts contribution policy and bug-report requirements, plus the issue thread and candidate claim/repro comments. Use the Comms section of `references/evidence-guide.md`. | Pass if the candidate comments satisfy applicable repository requirements for issue/contribution communication, including any required AI-assistance disclosure, and do not conflict with maintainer instructions in the issue thread. If no relevant convention is stated, pass. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Accept if every required check passes. Reject if any required check fails. Unclear counts as fail for required checks. Preferred checks never change the verdict.
