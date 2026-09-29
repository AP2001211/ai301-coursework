# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
**Where it lives:** In eval packages, read the issue context for the affected version/platform and the repro report's environment record for the environment actually tested. In live mode, read the GitHub issue for the reported environment and the student's draft repro comment for the environment they used.

**What good looks like:** The reproduction identifies the software/version and platform or other environment details needed to interpret the result. If the tested environment differs meaningfully from the issue's stated environment, the report explicitly identifies that difference rather than implying an exact match.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
**Where it lives:** In eval packages, read the repro report's preparation/setup, test input, commands, and actions. In live mode, read the student's draft repro comment and any setup instructions it references from the repository's documentation.

**What good looks like:** A stranger with the repository can start from the stated setup and repeat the attempted reproduction through the bug trigger without guessing an essential command, input, value, file change, or action. The steps use the actual trigger being tested rather than a materially different substitute.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
**Where it lives:** Read the issue's described trigger and actual behavior alongside the repro report's exact test input and resulting output, logs, error text, stack trace, or other observable artifact. In live mode, compare the GitHub issue directly with the evidence included in the draft repro comment.

**What good looks like:** For a claimed reproduction, the test preserves the issue's relevant trigger conditions and the resulting artifact shows the same behavior or failure mode described by the issue. A different error is not equivalent merely because the same program or component fails. For a cannot-reproduce result, the faithful trigger is shown and the evidence demonstrates that the reported behavior did not occur.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
**Where it lives:** Compare the candidate's claim comment and the repro report's analysis, actual-result statement, and conclusion with the concrete artifacts produced by the reproduction.

**What good looks like:** The wording does not claim more than the evidence establishes. Matching evidence may be called reproduced; a different result is described as a mismatch; and a faithful attempt that does not trigger the bug is reported as cannot reproduce rather than being converted into a successful reproduction claim.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
**Where it lives:** In eval packages, read the repo-facts contribution policy and bug-report requirements, the issue thread for maintainer instructions, and the candidate claim and repro comments. In live mode, check the repository's CONTRIBUTING.md, issue/PR templates, dedicated AI or contribution-policy files when present, the issue thread, and the student's draft comments.

**What good looks like:** The comments comply with applicable repository-specific communication requirements, including required AI-assistance disclosure when stated, and do not contradict maintainer instructions. The claim identifies the specific issue and investigation the contributor intends to perform without promising an unverified fix or deadline. If the repository states no relevant convention, absence of one is not a failure.
