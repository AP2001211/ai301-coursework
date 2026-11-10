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

**Unit 7: Installed and validated the ship-gate on my GRASS GIS contribution branch.**

I configured the ship-gate manifest to run the `r.proj` pytest suite in Docker and GRASS's full pre-commit checks before evaluating my branch with `pr-precheck`. My first real run was rejected because my local `main` was outdated, making a single-file contribution appear to contain 17 changed files; I corrected the local base reference and updated inconsistent test evidence. The final ship-gate run returned **PASS**, with all 12 tests passing, the full pre-commit suite passing, and all nine `pr-precheck` criteria accepted. This showed me that a release gate must validate both the contribution and the accuracy of its comparison baseline.

**Unit 8: Configured a PR monitor and managed maintainer feedback on my GRASS GIS contribution.**

I installed the PR monitor and wrote a playbook defining reviewer-response rules, a seven-day follow-up threshold based on five recently merged GRASS PRs, nudge etiquette, and walk-away criteria. The monitor detected the changes-requested review on PR #8165 but drafted a recommendation to wait because it could not retrieve the inline review comments. I investigated those comments directly, addressed the SPDX header, excessive explanation, and fixture-placement feedback, then verified the changes with 12 passing tests and the full pre-commit suite. Instead of posting the monitor's draft, I wrote and posted my own evidence-based response after pushing commit `391d070502`, demonstrating why human review of AI-generated recommendations matters.

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

GRASS GIS issue #2512 concerns the reliability and testing of `r.proj`, particularly its behavior during raster reprojection. While investigating the issue, I found that existing tests covered several interpolation methods and output flags, but did not validate the bounds reported by `r.proj -p` for a north-polar coordinate reference system. This left an important projection scenario without dedicated regression coverage.

**Expected Behavior**

When `r.proj -p` is used with a raster spanning high northern latitudes and a destination project using EPSG:3413, it should report finite, correctly ordered projected bounds. The JSON output should also contain the expected source raster dimensions. A regression test should verify these properties consistently.

**Current Behavior**

My investigation did not establish a bug in the existing bounds calculation. Running `r.proj -p` on the existing `input_polar` raster produced finite, correctly ordered bounds. Independent `cs2cs` calculations confirmed the reported values at the edge points sampled by `bordwalk_edge()`. However, those sampled bounds differ from the mathematically exact extent by approximately 2.2 km, which is an important limitation of the current sampling approach. The missing piece was an automated test preserving this behavior.

**Affected Components**

The contribution focuses on `raster/r.proj/tests/r_proj_output_test.py`, which tests the output flags of `r.proj`, and `raster/r.proj/tests/conftest.py`, which defines shared GRASS test fixtures. The relevant behavior is the projected bounds output from `r.proj -p`, including its JSON format and edge-sampling logic. No production implementation was changed.

**Toolkit**

- **Tool:** Claude Code, GRASS GIS source code, Docker, `r.proj`, `cs2cs`, and GitHub.
- **What it produced:** An understanding of the existing polar raster fixture, the current bounds calculation, gaps in test coverage, and the relationship between issue #2512 and draft PR #7807.
- **Difficulties:** The issue had received substantial work through other PRs, and the calculated polar bounds were not identical to the mathematically exact extent. I needed to distinguish missing regression coverage from an actual defect.
- **What changed:** I narrowed the contribution to a focused polar bounds test, independently verified its reference values, and avoided claiming that the new test fixed a confirmed bug.

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

I first established a baseline on GRASS commit `efe1c22249` using the `grass-ai301-test` Docker environment. All 11 existing `raster/r.proj/tests/` tests passed, confirming that this was a missing coverage scenario rather than a reproduced failure.

Before implementing the test, I recreated the existing `input_polar` raster and ran `r.proj -p` in an EPSG:3413 destination. I compared its reported bounds with independent `cs2cs` projections of the edge points sampled by `bordwalk_edge()`. The values matched within 1e-6 m. I also verified that the bounds were finite and ordered and that using `-n` did not change the result.

After adding `test_print_json_polar`, the suite increased from 11 to 12 tests. The new test checks the raster dimensions, finite and ordered bounds, and expected north, south, east, and west values using `pytest.approx`. These values represent the current edge-sampling behavior, not the mathematically exact geographic extent.

I personally reran the full suite using GRASS built from the rebased commit `efb3dced6b` in an Ubuntu 26.04 arm64 Docker container with PROJ 9.7.1. The result was 12 passed, 1 non-failing warning in 2.58 seconds. A separate amd64 Docker run with PROJ 9.9.0 also passed all 12 tests. Ruff 0.17.3 lint and formatting checks passed on the changed file.

The remaining verification is GRASS's pull-request CI on other platforms. The pull request is open and awaiting review.

**Toolkit**

- **Tool:** Docker, pytest, cs2cs, Ruff, and Claude Code.
- **What it produced:** A reproducible baseline, independently checked polar bounds, a passing regression test, and lint/format verification.
- **Difficulties:** The initial Docker baseline used an older commit, the first rebased CMake build failed with FreeType disabled, and the expected bounds reflect sampled edges rather than the exact geographic extent.
- **What changed:** I rebased onto upstream, rebuilt GRASS with FreeType enabled, and verified all 12 tests using the updated build.

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-09**

Personally verified the `r.proj` baseline using the `grass-ai301-test` Docker image built from commit `efe1c22249`: 11 passed, 1 warning in 11.88s (non-failing pytest configuration warning). Reviewed open draft PR #7807 and found it already adds an EPSG:3413 `session_pole` fixture and a serial-versus-parallel polar reprojection test, so I narrowed my proposal to a possible `r.proj -p` polar bounds regression test. Posted a follow-up on #2512 asking whether that test would be useful and whether it belongs in #7807 or a separate PR. No implementation or pull request has been created; I am awaiting maintainer clarification.

**2026-11-09 (continued)**

echoix replied on #2512 that a focused new PR that can be merged independently might be easier to review for a first contribution. Since #7807 is an unmerged draft, I kept my test independent of it: I added a module-scoped EPSG:3413 fixture in `r_proj_output_test.py` (project `dst3413`, distinct from #7807's `dst_pole`) and left `conftest.py` unchanged. Before writing the test, I ran `r.proj -p` on `input_polar` in EPSG:3413: the bounds were finite and ordered, `-n` made no difference, and they matched `cs2cs` at the edge points `bordwalk_edge()` samples, about 2.2 km inside the exact extent. I then added `test_print_json_polar`, committed it as `r.proj: Add polar bounds regression test`, and rebased it onto upstream `0d8dd26551`. My first build of the rebased commit failed at CMake configure because turning FreeType off breaks the `g.mkfontcap` target; I re-enabled FreeType rather than changing GRASS code. I personally ran the full `raster/r.proj/tests/` suite on the GRASS build from the rebased commit (`efb3dced6b`, Ubuntu 26.04 arm64, PROJ 9.7.1): 12 passed, 1 warning in 2.58s. I pushed the branch to my fork and opened PR #8165 (GitHub records it as opened 2026-11-10 00:57 UTC). It is open and awaiting review.

**Toolkit**

- **Tool:** Claude Code, Docker, Git, pytest, Ruff, and cs2cs.
- **What it produced:** Claude Code helped me investigate the GRASS codebase, develop the polar bounds regression test, and review the implementation. Docker provided reproducible environments, pytest verified the tests, Ruff checked code quality, and cs2cs independently validated the reference bounds.
- **Difficulties:** I initially planned overlapping test coverage with PR #7807. The expected bounds also reflected sampled edge points rather than the exact geographic extent. After rebasing, my first GRASS build failed because FreeType was disabled.
- **What changed:** I narrowed the implementation to one independent test, documented the edge-sampling limitation, enabled FreeType to complete the updated build, and personally verified all 12 tests before submitting PR #8165.

**2026-11-09 — Unit 7: Ship-gate validation**

Installed the Unit 7 ship-gate and configured `ship-manifest.md` with the GRASS `r.proj` pytest suite and repository-wide pre-commit checks. The first full run rejected my contribution because my local `main` was outdated, causing `pr-precheck` to compare 17 changed files instead of the one file in PR #8165. I updated the local base reference, corrected the manifest's Docker platform and Python executable, and synchronized the PR draft and test evidence. The final ship-gate run passed both repository checks and all nine `pr-precheck` criteria.

**Toolkit**

- **Tool:** Unit 7 ship-gate, `pr-precheck`, Docker, pytest, pre-commit, and Git.
- **What it produced:** A ship manifest defining required checks, a recorded gate run against `test/rproj-polar-bounds`, and a final `ship-gate: PASS`.
- **Difficulties:** The initial comparison included unrelated upstream commits because local `main` was outdated. The manifest also needed adjustments for the ARM64 Docker image and `python3`, while the PR draft and test evidence initially disagreed about pre-commit results.
- **What changed:** I corrected the comparison baseline, updated the manifest and evidence, and reran the gate until both repository checks and all nine rubric criteria passed. The GRASS PR remained unchanged.

---

## Pull Request

**PR Link:** https://github.com/OSGeo/grass/pull/8165

**PR Description:** Adds `test_print_json_polar`, which checks the bounds reported by `r.proj -p` for `input_polar` in EPSG:3413. The expected values were independently verified using `cs2cs` at the edge points sampled by `bordwalk_edge()`, so they represent the current sampling behavior rather than the exact geographic extent. Following maintainer feedback, I moved the EPSG:3413 session-scoped fixture into `conftest.py`, added an SPDX header, removed the hardcoded test count, and shortened the explanatory comments. The PR remains independently mergeable without relying on draft PR #7807, which tests serial-versus-parallel reprojection instead. It references issue #2512, adds regression coverage rather than fixing a confirmed bug, documents testing results, and discloses my use of Claude Code for investigation and implementation assistance.


**Maintainer Feedback:**

- **2026-11-10 — echoix:** Pointed out the missing SPDX license header, recommended removing the hardcoded test count from the module docstring, and suggested shortening the lengthy explanation above the polar regression test.
- **2026-11-10 — wenzeslaus:** Requested changes to the placement of the new EPSG:3413 fixture so that it follows the existing test organization.
- **2026-11-10 — My response:** Added the SPDX header, removed the test count, shortened the explanatory comments, and moved `session_3413` into `conftest.py` alongside the existing session fixture. Verified the revisions with 12 passing tests and the full pre-commit suite, then pushed commit `391d070502`. I also posted a reply acknowledging both reviewers and explaining the changes: https://github.com/OSGeo/grass/pull/8165#issuecomment-6773938539.


**Status:** Iterating — reviewer-requested changes have been implemented and pushed in commit `391d070502`. Awaiting another review.

**Toolkit**

- **Tool:** Claude Code, Git, GitHub, and the GRASS GIS contribution guidelines.
- **What it produced:** A focused pull request (#8165) containing a polar bounds regression test, a description of the changes, reproducible testing evidence, and an AI-assistance disclosure.
- **Difficulties:** I had to ensure my PR did not duplicate draft PR #7807, follow the repository's commit-message conventions, and correct broken links in the PR description after submission. Maintainer feedback also required revisions to the test's documentation and fixture placement.
- **What changed:** I submitted the regression test as an independent PR, corrected the description links, and documented the testing and AI-assisted development process. After review, I added the SPDX header, removed the hardcoded test count, shortened the comments, and moved the fixture into `conftest.py`. I verified the changes and pushed commit `391d070502`, then responded to both reviewers.
---

## Learnings and Reflections

**Technical Skills Gained**

This contribution helped me understand how GRASS GIS handles raster reprojection, particularly the difference between geographic extents and projected bounds calculated through edge sampling. I learned to investigate `r.proj -p`, work with EPSG:3413, independently verify coordinates using `cs2cs`, and write pytest regression tests using GRASS's shared fixtures and session management.

I also gained practical experience building and testing a large C/Python codebase with Docker, diagnosing build configuration problems, running repository-wide pre-commit checks, and managing Git branches against an upstream repository. Building the AI-assisted workflow taught me how to use issue-scout, repro-check, plan-check, pr-precheck, ship-gate, and PR-monitor while independently verifying their conclusions.

**Challenges Overcome**

One of the biggest challenges was identifying a useful contribution without duplicating existing work. Issue #2512 had already received substantial attention through other pull requests, including draft PR #7807. After investigating the existing tests and discussing the scope with a maintainer, I narrowed my contribution to an independently mergeable polar bounds regression test.

I also encountered several technical and workflow problems. The first rebased Docker build failed because FreeType was disabled, and the ship-gate initially rejected my branch because an outdated local `main` made a one-file change appear to modify 17 files. I resolved these problems by correcting the build configuration and aligning the local comparison base with upstream.

Maintainer feedback led to another round of improvements: I added an SPDX header, removed a hardcoded test count, shortened explanatory comments, and moved the EPSG:3413 fixture into `conftest.py`. The updated code passed all 12 tests and the full pre-commit suite. The PR monitor also exposed a limitation: it detected a changes-requested review but could not retrieve the inline comments, so I investigated them directly before responding.

**What I'd Do Differently Next Time**

I would investigate the complete history of an issue, including unlinked merged and draft PRs, before committing to an implementation approach. I would also establish the correct upstream comparison base earlier and verify that every Docker command targets the intended image architecture and Python executable.

For the implementation itself, I would inspect repository conventions for fixture placement, licensing headers, and concise code comments before writing the first version. I would also keep test evidence and PR descriptions synchronized as verification progresses.

Most importantly, I would continue using AI tools for investigation, planning, and review, but treat their outputs as recommendations rather than authoritative conclusions. This project showed me that even a tool reporting a failure can be wrong because of its environment, and a tool recommending that I wait can lack important review context. My responsibility is to verify the evidence and make the final decision.

---

## Resources Used

- **[GRASS GIS Issue #2512](https://github.com/OSGeo/grass/issues/2512):** Helped me understand the original `r.proj` concerns, existing work, and maintainer guidance about making an independently mergeable contribution.
- **[GRASS GIS Draft PR #7807](https://github.com/OSGeo/grass/pull/7807):** Helped me identify overlapping polar reprojection tests and design a separate bounds regression test.
- **[GRASS GIS PR #8165](https://github.com/OSGeo/grass/pull/8165):** Provided the actual review feedback from echoix and wenzeslaus, which guided my revisions to the fixture placement, documentation, and SPDX header.
- **GRASS GIS `AGENTS.md` and `CONTRIBUTING.md`:** Guided test execution, repository conventions, AI-assistance disclosure, and communication with maintainers.
- **GRASS GIS test files (`conftest.py` and `r_proj_output_test.py`):** Provided existing fixtures and test conventions for implementing the regression test.
- **PROJ `cs2cs` utility:** Allowed me to independently verify the projected reference bounds rather than relying only on `r.proj` output.
- **CodePath AI301 Units 5–8 materials:** Provided the issue-scout, PR precheck, ship-gate, and PR-monitor workflows used to select, validate, submit, and monitor my contribution.
