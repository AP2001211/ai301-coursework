# Contribution: Adding coordinate reference system test coverage for r.proj

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Apoorva Vutukur
**GitHub Username:** AP2001211
**Issue:** https://github.com/OSGeo/grass/issues/2512
**Pull Request:** https://github.com/OSGeo/grass/pull/8165

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and refined an issue-scout for real GitHub contributions**

I adapted my Unit 1 issue-selection skill into an issue-scout with three Python-focused GitHub search queries and ranking filters based on assignment status, recent activity, labels, and comment counts. My first scan evaluated five issues, accepting two and rejecting three, but revealed that the ranking favored newly created repositories and sometimes selected multiple issues from the same project. I updated the strategy to retain only one candidate per repository in the initial grading shortlist, reducing repeated evaluations of repository-level risks. When I graded two issues from the course selection site, the scout rejected both because existing pull requests already addressed them, reinforcing why checking active claims matters more than relying on a "good first issue" label.

**Unit 6: Installed voice guard, refined issue-scout, and tested upstream posting**

In Unit 6, I installed a shared voice guide and a voice-guard hook for upstream GitHub comments. During my first attempt to post on GRASS #2512, the hook blocked the command because my draft did not specify when I would report back. I revised the sentence to promise an update within three days, and the next posting attempt passed.

My issue-scout also revealed limitations during selection. It initially accepted an issue reserved by maintainers, so I updated its `unclaimed` check and verified that the issue was subsequently rejected. I also discovered that GRASS #2512 had received substantial test coverage through a merged PR that wasn't linked to the issue. Rather than proposing duplicate work, I asked maintainers whether an additional polar CRS test would be useful.
I also learned that an open issue can have most of its requested work already implemented through an unrelated merged PR, so checking only linked PRs is insufficient.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched for open-source issues that involved Python, testing, or backend development and were small enough for me to investigate and reproduce locally. I used my issue-scout to evaluate repositories for maintainer activity, project activity, scope feasibility, existing claims, and contribution policies. Several issues that initially looked promising were already being addressed by other contributors. I eventually selected OSGeo/grass #2512, which requests basic unit tests for the `r.proj` raster reprojection tool. My scout accepted the issue across all five checks. I was interested in it because I have experience working with Python and automated testing, and it offered an opportunity to learn more about geospatial systems and coordinate reference systems.

Further investigation revealed a limitation in my scout. Although #2512 was still open, PR #7766 had already introduced pytest coverage for several `r.proj` behaviors without explicitly linking to the issue. Rather than assume that the original work was still needed, I examined the existing tests and identified a possible gap involving polar coordinate reference systems. I posted a clarification asking whether that additional coverage would be useful or whether the issue was effectively complete. This experience showed me that issue availability depends not only on assignments and open PRs but also on understanding what has already been implemented.

**Clarification comment:** https://github.com/OSGeo/grass/issues/2512#issuecomment-6732311600

**Toolkit**

- **Tool:** issue-scout, using my revised issue-selection rubric to evaluate candidate GitHub issues.
- **What it produced:** An accepted verdict for GRASS #2512, with evidence for all five required checks.
- **Difficulties:** The scout did not detect that a merged PR had already implemented much of the requested functionality because the PR was not linked to the issue.
- **What changed:** I investigated the existing tests before posting and changed my intended claim into a targeted clarification question. I would extend the scout to check for merged implementations that are not linked to the issue.


---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

I used macOS on an Apple Silicon (arm64) Mac with Docker Desktop. Because GRASS GIS was not installed locally, I downloaded the official `osgeo/grass-gis:main-ubuntu` Docker image and ran it using `linux/amd64` emulation.

The environment used GRASS GIS 8.6.0dev at revision `efe1c22249`, Python 3.14.4, and pytest 9.2.0. I created a custom Docker image, `grass-ai301-test`, with a Python virtual environment and the required testing dependencies.

My initial setup failed because the official image lacked pytest and the `ensurepip` component needed to create a virtual environment. I resolved this by installing `python3.14-venv` and adding pytest and NumPy to a custom Docker image.

**Steps to Reproduce**

1. Clone `OSGeo/grass` and use the repository at revision `efe1c22249`.
2. Build the custom testing image using the Dockerfile documented in my `repro-draft.md`.
3. Mount the repository into the container at `/workspace`, configure `PYTHONPATH` and `LD_LIBRARY_PATH` using `grass --config`, and run:

   ```bash
   python -m pytest raster/r.proj/tests/ -v
   ```

4. Inspect `raster/r.proj/tests/conftest.py`, `r_proj_methods_test.py`, and `r_proj_output_test.py` to identify the existing CRS coverage.

**Reproduction Evidence**

- **Commit showing reproduction:** No reproduction-specific commit yet; the investigation used upstream revision `efe1c22249`.
- **Screenshots/logs:** pytest collected 11 tests: seven interpolation-method tests and four output-format tests. All 11 passed in 12.54 seconds. One `PytestConfigWarning` reported an unknown `timeout` configuration option. This was my reproduction-stage run, saved in `ai301-unit6-starter/evidence/r-proj-baseline.txt`; the 11.88-second result under Solution Approach is a separate, later re-run.
- **My findings:** PR #7766 had already introduced pytest coverage for EPSG:4326 to EPSG:3857. The existing setup creates an `input_polar` raster, but I found no test using it. This suggests additional polar CRS coverage might be useful, but I did not reproduce an incorrect reprojection result. I asked the maintainer whether that coverage would be wanted before implementing it.

**Toolkit**

- **Tool:** repro-check, used to grade my claim and reproduction report against the issue, repository conventions, and actual execution evidence.
- **What it produced:** An accepted claim and a reproduction report that passed all five required checks, supported by 11 successful baseline tests.
- **Difficulties:** My first report was rejected because it lacked the complete Docker setup commands and quoted pytest evidence. The Docker image also initially lacked pytest and `ensurepip`.
- **What changed:** I built a reusable testing image, added reproducible commands and direct evidence to the report, corrected inaccurate terminology, and regraded until all five checks passed.
---

## Solution Approach

**Analysis**

This is a possible missing test scenario, not a defect in `r.proj`. On `main`, the tests in `raster/r.proj/tests/` cover EPSG:4326 to EPSG:3857 only, and `conftest.py` creates an `input_polar` raster that no test reprojects. I personally re-ran the baseline using the `grass-ai301-test` Docker image built from commit `efe1c22249`: 11 passed, 1 warning in 11.88s. The warning is a non-failing pytest configuration warning.

I then reviewed open draft PR #7807. It already adds an EPSG:3413 `session_pole` fixture and a test that reprojects `input_polar` serially and in parallel and checks that the outputs are bitwise identical. From reading the existing tests and #7807, I have not found an assertion on the bounds that `r.proj -p` reports for `input_polar` in EPSG:3413.

**Proposed Solution**

My proposed contribution is now limited to a possible `r.proj -p` polar bounds regression test, subject to maintainer approval. I am deliberately not adding a second EPSG:3413 fixture or repeating #7807's polar reprojection test, and I am not changing `r.proj` production code.

**Implementation Plan**

1. Run the existing `r.proj` test baseline and report the results on #2512. Done: 11 passed, 1 warning in 11.88s on `efe1c22249`.
2. Ask maintainers whether a polar `-p` bounds test would be useful, and whether it belongs in #7807 or a separate PR. Done: posted a follow-up on #2512.
3. Wait for maintainer clarification before writing any test. If they decline, stop here. Done: echoix recommended a focused new PR that can be merged independently (https://github.com/OSGeo/grass/issues/2512#issuecomment-6754067520).
4. If approved, reuse #7807's `session_pole` fixture, determine and independently verify reference bounds for `input_polar`, and add a single bounds test. Done, with one change: because the PR had to merge independently of draft #7807, I used a module-scoped EPSG:3413 fixture in `r_proj_output_test.py` instead of `session_pole`. Submitted as PR #8165.

Deviation: my original plan added its own EPSG:3413 fixture and polar reprojection test. After finding #7807, I narrowed it to the bounds assertion only, so as not to duplicate that work. #7807 also led me to keep my PR independent of it, so my test does not depend on its unmerged fixture.

**Follow-up comment:** https://github.com/OSGeo/grass/issues/2512#issuecomment-6753752297

**Toolkit**

- **Tool:** plan-check, used to grade my `plan.md` and draft plan comment against the issue thread, the repository, and related PRs.
- **What it produced:** An initial accept, then a reject when a re-run found that open draft PR #7807 already adds an EPSG:3413 fixture and polar test, then an accept after I narrowed the plan to a bounds assertion.
- **Difficulties:** My procedure only checked PRs linked from the issue. #7807 is not linked to #2512, so the strict procedure would have missed the overlap.
- **What changed:** I added a step to `procedure.md` that searches open PRs touching the same module and files, and revised the plan and comment to name #7807 and avoid duplicating it.

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-09**

Personally verified the `r.proj` baseline using the `grass-ai301-test` Docker image built from commit `efe1c22249`: 11 passed, 1 warning in 11.88s (non-failing pytest configuration warning). Reviewed open draft PR #7807 and found it already adds an EPSG:3413 `session_pole` fixture and a serial-versus-parallel polar reprojection test, so I narrowed my proposal to a possible `r.proj -p` polar bounds regression test. Posted a follow-up on #2512 asking whether that test would be useful and whether it belongs in #7807 or a separate PR. No implementation or pull request has been created; I am awaiting maintainer clarification.

**2026-11-09 (continued)**

echoix replied on #2512 that a focused new PR that can be merged independently might be easier to review for a first contribution. Since #7807 is an unmerged draft, I kept my test independent of it: I added a module-scoped EPSG:3413 fixture in `r_proj_output_test.py` (project `dst3413`, distinct from #7807's `dst_pole`) and left `conftest.py` unchanged. Before writing the test, I ran `r.proj -p` on `input_polar` in EPSG:3413: the bounds were finite and ordered, `-n` made no difference, and they matched `cs2cs` at the edge points `bordwalk_edge()` samples, about 2.2 km inside the exact extent. I then added `test_print_json_polar`, committed it as `r.proj: Add polar bounds regression test`, and rebased it onto upstream `0d8dd26551`. My first build of the rebased commit failed at CMake configure because turning FreeType off breaks the `g.mkfontcap` target; I re-enabled FreeType rather than changing GRASS code. I personally ran the full `raster/r.proj/tests/` suite on the GRASS build from the rebased commit (`efb3dced6b`, Ubuntu 26.04 arm64, PROJ 9.7.1): 12 passed, 1 warning in 2.58s. I pushed the branch to my fork and opened PR #8165 (GitHub records it as opened 2026-11-10 00:57 UTC). It is open and awaiting review.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** https://github.com/OSGeo/grass/pull/8165

**PR Description:** Adds `test_print_json_polar`, which checks the bounds `r.proj -p` reports for `input_polar` in EPSG:3413. The expected bounds were checked independently with `cs2cs` at the edge points `bordwalk_edge()` samples, so they reflect current sampling rather than the exact extent. A module-scoped fixture keeps the PR independent of draft #7807, whose polar test compares serial and parallel output instead of `-p` bounds. It references #2512 without closing it, states that it adds coverage rather than fixing a confirmed bug, lists the test and Ruff results, and discloses that I used Claude Code to navigate the codebase, investigate the issue, and help draft the test, which I reviewed and ran.

**Maintainer Feedback:**

- 2026-11-10: none yet (no reviews or comments when recorded).
- [Date]: [how you addressed it]

**Status:** Awaiting review.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
