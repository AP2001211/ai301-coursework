# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->
## Diagnosis and grounding

**Where it lives:** In an eval package, read the Candidate plan's cause
or diagnosis against the Repro evidence block, especially its
environment, triggering steps, expected result, actual result, and any
observations that constrain the cause. Use Issue context when it adds
relevant behavior or constraints. In live mode, use the diagnosis in
`plan.md` and the student's posted reproduction comment on the issue
thread.

**What good looks like:** The stated cause explains the behavior that
was actually reproduced and does not contradict the reproduction.
The diagnosis does not need the reproduction to prove every internal
implementation detail, but claims presented as established must be
supported or consistent with the available evidence.

## Scope

**Where it lives:** In an eval package, read the Candidate plan's
Change statement, including what is in scope, what is explicitly out
of scope, and any files or code areas it names. Compare that work with
the reproduced problem. In live mode, use the scope, non-scope, and
files-to-touch portions of `plan.md`.

**What good looks like:** The proposed work is bounded to the change
needed to address the reproduced problem. Related files or checks may
be included when necessary to implement or verify that change, but
unrelated cleanup, redesigns, or drive-by changes are excluded.

## Executability

**Where it lives:** In an eval package, read the Candidate plan's
Change statement and implementation approach, including named files,
code areas, callbacks, functions, or components. Check those claims
against the Repo facts when relevant facts or conventions are given.
In live mode, use the files and approach in `plan.md` together with
the repository's actual structure and relevant documentation.

**What good looks like:** Another contributor can identify where to
start and what behavior or code path to change without having to
invent the core implementation strategy. Named locations and actions
must be consistent with the available repository evidence.

## Test plan

**Where it lives:** In an eval package, read the Candidate plan's Test
statement against the Repro evidence's triggering inputs, steps,
expected result, actual result, and observable artifacts. In live
mode, compare the test plan in `plan.md` with the student's posted
reproduction steps and evidence.

**What good looks like:** The verification exercises the reproduced
failure through the relevant path and names an observable post-fix
result that distinguishes fixed behavior from the original failure.
An automated test is not required unless the repository or issue
requires one; the verification must be decisive rather than merely
saying to test or confirm the fix.

## Honesty

**Where it lives:** In an eval package, read the Candidate plan for
risks, unknowns, assumptions, and factual claims, and compare material
claims with the Repro evidence, Thread highlights, and Repo facts.
When grading a completed build, also read its Deviations record. In
live mode, use the risks and unknowns in `plan.md`, the available issue
and repository evidence, and later the `## Deviations` section.

**What good looks like:** Facts supported by the package may be stated
directly; material facts that remain unresolved are identified as
assumptions or unknowns rather than asserted as certain. If the build
changes course, the deviation states what changed and why instead of
silently rewriting the original plan.

## Comms

**Where it lives:** In an eval package, read the Candidate plan
comment against Thread highlights and the Repo facts, especially
maintainer requests, issue constraints, contribution guidance,
templates, and any AI-use disclosure requirement. Compare the comment
with the Candidate plan so it does not promise materially different
work. In live mode, use the draft `comment.md`, the full GitHub issue
thread, and relevant repository contribution documentation.

**What good looks like:** The comment reflects the actual proposed
change, respects relevant maintainer requests and repository
conventions, and does not ignore or contradict material constraints.
It communicates the plan specifically enough to the issue rather than
using generic boilerplate, and follows any applicable contribution or
AI-disclosure requirements.
