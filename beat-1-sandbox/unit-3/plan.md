# Plan for Issue #60

## Diagnosis

`FaithfulnessChecker.check()` constructs `context_text` with:

`" ".join([chunk.get("text", "") for chunk in context_chunks])`

The default value passed to `dict.get()` only applies when the `text`
key is missing. When a context chunk contains `{"text": None}`, the
lookup returns `None`, so the list passed to `" ".join()` contains a
`NoneType` value. `join()` requires strings and raises:

`TypeError: sequence item 0: expected str instance, NoneType found`

This matches both the direct reproduction and the existing regression
test for issue #60.

## Scope

### In scope

- Make `FaithfulnessChecker.check()` handle a context chunk whose
  `text` value is `None` without raising `TypeError`.
- Keep the existing behavior for chunks whose `text` value is a
  string or whose `text` key is missing.
- Enable the existing issue #60 regression test to verify the fixed
  behavior.

### Out of scope

- Changes to claim extraction or faithfulness scoring behavior.
- Changes to `RelevanceScorer`, `HybridRetriever`, or
  `ReviewGenerator`, even though those components also access a
  chunk's `text` field.
- Changes related to the separate issue #59 xfail tests.
- Broader validation or normalization of context chunk schemas.

## Files to touch

- `rag/evaluator/faithfulness_checker.py`
  - Normalize a `None` context `text` value to an empty string before
    the context strings are joined.

- `tests/unit/test_faithfulness_checker.py`
  - Remove the issue #60 `xfail` marker from
    `test_none_context_chunk_text` so the existing regression test
    becomes a normal passing test after the fix.

## Implementation approach

Update the context-text construction in
`FaithfulnessChecker.check()` so that both a missing `text` key and an
explicit `None` value contribute an empty string to `context_text`.
Keep the change local to the context concatenation rather than
changing downstream scoring behavior.

Then remove the strict `xfail` marker associated with issue #60 from
`test_none_context_chunk_text`. The assertions already require the
method to complete normally and return a float between `0.0` and
`1.0`, so the existing test can serve as the regression test without
changing its expected behavior.

## Test plan

First rerun the Unit 2 direct reproduction:

```bash
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```

Before the fix, this raises:
```text
TypeError: sequence item 0: expected str instance, NoneType found
```

After the fix, it should complete without raising an exception and
return a faithfulness score.
Run the issue #60 regression test normally, without --runxfail:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv
```

The test should pass and confirm that the result is a float between
0.0 and 1.0.

Also rerun the existing faithfulness checker unit tests to check that
the change does not regress normal string-valued or missing-text
behavior:
```bash
python -m pytest tests/unit/test_faithfulness_checker.py -vv
```

## Risks and unknowns
The intended behavior established by the existing issue #60 test is
that an explicit None text value should not crash the checker and
should still produce a valid score. The planned change treats None
the same as absent text for context concatenation.

This plan does not assume that other non-string text values should
be normalized. Handling arbitrary context value types would broaden
the behavior beyond the reproduced issue and is therefore left out of
scope.

## Deviations

No implementation deviations were needed. The fix stayed within the
planned two files and used the planned approach: normalize an explicit
`None` context text value before joining the context, and remove the
issue #60 `xfail` marker from the existing regression test.
