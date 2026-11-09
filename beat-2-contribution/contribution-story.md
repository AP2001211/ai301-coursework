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
**Pull Request:** Pending

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
- **Screenshots/logs:** pytest collected 11 tests: seven interpolation-method tests and four output-format tests. All 11 passed in 12.54 seconds. One `PytestConfigWarning` reported an unknown `timeout` configuration option.
- **My findings:** PR #7766 had already introduced pytest coverage for EPSG:4326 to EPSG:3857. The existing setup creates an `input_polar` raster, but I found no test using it. This suggests additional polar CRS coverage might be useful, but I did not reproduce an incorrect reprojection result. I asked the maintainer whether that coverage would be wanted before implementing it.

**Toolkit**

- **Tool:** repro-check, used to grade my claim and reproduction report against the issue, repository conventions, and actual execution evidence.
- **What it produced:** An accepted claim and a reproduction report that passed all five required checks, supported by 11 successful baseline tests.
- **Difficulties:** My first report was rejected because it lacked the complete Docker setup commands and quoted pytest evidence. The Docker image also initially lacked pytest and `ensurepip`.
- **What changed:** I built a reusable testing image, added reproducible commands and direct evidence to the report, corrected inaccurate terminology, and regraded until all five checks passed.
---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

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
