# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```

⏺ Live mode. Candidate is in codepath/pathreview-ai301-fa26-s1 — inside the
  scoped source. House rule (classmate claims don't block) noted; no claims exist
  here anyway.

  Issue #60 — "Faithfulness checker crashes when a context chunk has text: None"
  (labels: bug, good first issue, rag, tier-1; opened 2026-09-10 by Aburke225,
  COLLABORATOR)

  - maintainer-active — pass. Last default-branch commit 2026-09-16T21:42:18Z by
    Aburke225 (human), 4 days before today; the issue opener carries a
    COLLABORATOR association.
  - repo-in-use — pass. archived: false, no releases, but last push to any branch
    2026-09-16 (4 days ≤ 180).
  - newcomer-scope — pass. Concrete defect in one function with a reproduction
    snippet and a named failing test (test_none_context_chunk_text); no
    umbrella/tracking framing, no design debate (0 comments), no maintainer
    statement about core internals.
  - unclaimed — pass. assignees: [], 0 comments, no linked PRs in the timeline,
    and the repo has no PRs at all (open or closed).
  - contribution-policy — pass. docs/CONTRIBUTING.md and
    .github/PULL_REQUEST_TEMPLATE.md state no AI-contribution ban; the only
    conditions are green CI, tests, and removing the seeded bug's
    @pytest.mark.xfail marker.

  All five required checks pass → accept. Fit note: this is squarely your profile
  — a bounded Python bug in the RAG evaluator
  (rag/evaluator/faithfulness_checker.py) with a stated reproduction, testable
  expected behavior, and an existing xfail test to flip, so you touch real
  application code and its test suite rather than docs, with no frontend or infra
  work.

  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Last 
  default-branch commit 2026-09-16T21:42:18Z by human Aburke225 (4 days before 
  today); issue opened by Aburke225 with author_association COLLABORATOR."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Repo archived: false;
  no releases, but last push to any branch 2026-09-16T21:48:27Z, within 180 
  days."},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Body gives a 
  single concrete defect plus repro: \"chunk.get(\\\"text\\\", \\\"\\\")\" 
  returns None so \" \".join(...) raises TypeError, with failing test 
  test_none_context_chunk_text; no umbrella framing, no open design debate, no 
  core-internals statement."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], 
  comments: 0, no linked/cross-referenced PRs in the issue timeline, and the 
  repository has zero pull requests in any state."},
      {"name": "contribution-policy", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no 
  AI-generated-contribution ban; requirements are green CI, tests, and removing 
  the issue's xfail marker."}
    ],
    "verdict": "accept"
  }
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]
2/3
1/1
18/20
18/20
**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]
issue-01 — My final rubric decision was accept, and the gold label was accept. The issue passed the maintainer-active, repo-in-use, unclaimed, and contribution-policy checks. In my first run, newcomer-scope failed because I treated the issue's five related documentation changes as separate work and therefore as an umbrella issue. After inspecting the issue, I realized those changes all contributed to one stated outcome. I refined newcomer-scope so that multiple files, checklist items, or implementation steps do not by themselves make an issue an umbrella issue. With that rule, issue-01 passed newcomer-scope and the final verdict was accept.
**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]
newcomer-scope — "Pass if the issue asks for a concrete contribution rather than a usage/support question; is not explicitly identified as an umbrella or tracking issue whose sub-items are intended to be handled as separate contributions; has no unresolved design debate unless a maintainer has settled the direction; and has no maintainer statement that the fix requires changes to core internals. Multiple files, checklist items, or implementation steps that contribute to one stated outcome do not by themselves make the issue an umbrella issue."

I wrote the check this way because a good first issue should have a concrete, bounded outcome without requiring a newcomer to resolve an open design question or make changes to core internals. I added the final sentence after my first evaluation run because issue-01 showed that counting files or checklist items alone was too aggressive: one bounded contribution can legitimately require several related changes.
**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]
The broader newcomer-scope rule can accept issues that involve several files or steps, so it may let through some work that turns out to be larger than expected. I accepted that trade-off because the alternative caused a concrete false rejection: issue-01 initially failed when I treated its five related documentation changes as an umbrella issue. I re-ran issue-01 with --only after refining the check, and its result changed from reject to accept, matching the gold label.
---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]
1. Issue #60 fits my interests because it is a Python bug in the RAG part of the application, which is close to the AI/ML and backend work I have experience with. It is also small enough for a first contribution because the issue provides a reproduction and points to a specific function and test.

2. The verdict correctly identified that the issue is bounded, unclaimed, in an active repository, and has a clear way to test the fix. Beyond the rubric, I also considered what I would learn from the issue. I preferred #60 because it lets me understand and modify existing RAG application code while still having a clearly defined expected behavior.

3. I expect claiming it to be straightforward because there is currently no assignee, no discussion on the issue, and no linked pull request. The main uncertainty is that another student could claim it before I do, since this is a shared course repository.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
