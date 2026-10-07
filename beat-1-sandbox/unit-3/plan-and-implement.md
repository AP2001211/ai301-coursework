# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

AP2001211

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-6029684671

Thanks for letting me take a look at this. I reproduced the issue with a
context chunk containing `{"text": None}`. `FaithfulnessChecker.check()`
currently passes the result of `chunk.get("text", "")` directly into
`" ".join()`. Because the key exists, the lookup returns `None` rather
than the default empty string, and the join raises the reported
`TypeError`.

My plan is to keep the fix scoped to
`rag/evaluator/faithfulness_checker.py` by treating an explicit `None`
text value the same as missing text when the context is concatenated.
I also plan to remove the issue #60 `xfail` marker from the existing
`test_none_context_chunk_text` regression test so it becomes a normal
passing test.

I'll verify the change by rerunning the original reproduction, the
issue #60 regression test, and the faithfulness checker unit tests. I'm
not planning to change the scoring or claim-extraction behavior, the
separate issue #59 tests, or other components that access context
chunks.

---

## Your branch

**Branch**

`fix/60-none-context-text`

**Evidence**

Before the change, I ran the Unit 2 direct reproduction:

```bash
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```

It produced:

```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File ".../rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
```

I also ran the issue #60 regression test before the change:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv --runxfail
```

It failed at `rag/evaluator/faithfulness_checker.py:38` with:

```text
TypeError: sequence item 0: expected str instance, NoneType found
```

After building the change, I reran the same direct reproduction:

```bash
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```

It completed without the `TypeError` and produced:

```text
2026-10-06 22:40:54 [info] faithfulness_checked claims_count=1 score=0.0 supported_count=0
```

I then ran the issue #60 regression test normally after removing its `xfail` marker:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv
```

It passed:

```text
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text PASSED

1 passed in 0.36s
```

I also reran the full faithfulness checker unit test file:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py -vv
```

Result:

```text
19 passed, 3 xfailed in 0.39s
```

Finally, I ran the full unit test suite:

```bash
make test-unit
```

Result:

```text
376 passed, 52 xfailed, 3 warnings in 19.65s
```

## Eval iterations

**Run history**

20/20

**Package analysis**

`pkg-06`: my rubric decided `reject`, and the gold label was `reject` (`scope-creep`).

My rubric read this package as not ready because the `bounded_scope` check requires that the proposed work be "one bounded change needed to fix the reproduced problem and does not introduce unrelated work or unnecessary scope." The package proposed work beyond what was needed for the reproduced failure, so it failed a required check. Under my verdict rule, a failure on any required check results in `reject`.

**Check rationale**

One check in my submitted rubric is:

> `bounded_scope` | The plan's scope, non-scope, files to touch, and proposed changes. | Passes if the work is one bounded change needed to fix the reproduced problem and does not introduce unrelated work or unnecessary scope. | required

I wrote the check around the outcome — whether the proposed work is one bounded change — instead of requiring a particular plan format or number of sections. I also made it compare the scope, non-scope, files, and proposed changes rather than judging a scope statement in isolation. This lets the check catch scope creep even when a plan is well formatted and otherwise detailed.

**Trade-offs**

I kept every check required and treated `unclear` as a failure for required checks. The trade-off is that this rubric can reject a potentially workable plan when the submitted evidence is incomplete instead of giving the contributor the benefit of the doubt. I accepted that trade-off because the skill is deciding whether a plan is ready to post and build from; if there is not enough evidence to determine whether a required condition is satisfied, I do not consider the plan ready yet.

I did not loosen or revise any checks after the scored evaluation because the first full run reached 20/20 agreement, including matches in every failure category. Since no packages disagreed with the gold labels, I had no failing package that justified changing a check or running a targeted `--only` canary.