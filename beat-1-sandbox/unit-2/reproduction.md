# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

AP2001211

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5898059673

Hi, I'd like to investigate #60. I'll reproduce the reported `FaithfulnessChecker.check()` TypeError with a context chunk containing `{"text": None}` on my local environment and also run the related `test_none_context_chunk_text` test. I'll report back here with my environment, exact steps, and observed result before working on a fix.

Disclosure: I used AI assistance to help draft this comment; I'll run and verify the reproduction myself.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5898596893

## Reproduction of #60

I reproduced the reported `TypeError` when `FaithfulnessChecker.check()` receives a context chunk containing `{"text": None}`.

### Environment

- OS: macOS (Darwin 25.6.0, arm64)
- Python: 3.12.8
- pytest: 9.1.1
- Repository: my fork of `codepath/pathreview-ai301-fa26-s1`
- Branch: `main`
- Commit: `f89c06fc3ff292df2a04a39ac51319d32a76b779`
- Environment: local `.venv` with the project installed using `python -m pip install -e ".[dev]"`

### Steps and observed behavior

From the repository root with the virtual environment activated, I ran the example from the issue:

```bash
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```

This produced:

```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File ".../rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
```

I also ran the regression test named in the issue with --runxfail so that pytest would execute it as a normal test:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv --runxfail
```

The test failed at rag/evaluator/faithfulness_checker.py:38 with the same error:

```text
TypeError: sequence item 0: expected str instance, NoneType found
```

As a control in the same environment, I supplied a string value instead of None:

```bash
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print(FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python well'}]))"
```

This completed successfully and returned:
1.0

#### Expected vs. actual
- Expected: FaithfulnessChecker.check() handles a context chunk whose text value is None without raising an exception and returns a faithfulness score.
- Actual: with {"text": None}, check() raises TypeError: sequence item 0: expected str instance, NoneType found while constructing context_text.
- Reproduction verdict: Confirmed. The direct example and the named regression test both reproduce the reported failure in my environment.

Disclosure: I used AI assistance to help organize this report, but I ran the reproduction and control commands myself and verified the outputs above.

## Eval iterations

**Run history**

Initial setup run (`--limit 3`): 3/3 agreement.

Initial full evaluation: 18/20 agreement. The disagreements were `pkg-09` and `pkg-20`.

Targeted run on `pkg-09` and `pkg-20`: `pkg-09` still disagreed with the gold label, while `pkg-20` matched. This isolated the remaining problem to how my `behavior-matched` check handled an evidenced cannot-reproduce result.

After revising `behavior-matched`, I ran a targeted canary on `pkg-09`, `pkg-02`, and `pkg-20`: 3/3 agreement. I included `pkg-02` as a wrong-target canary and `pkg-20` as the disclosure-convention canary to check that the revision had not made those cases too permissive.

Final full evaluation: 20/20 agreement. All categories matched: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, and wrong-target 4/4.

**Package analysis**

I used `pkg-09` to diagnose the main disagreement. The gold label was `accept`, while my initial rubric decided `reject`. My `behavior-matched` check was too strict about an unconfirmed precondition when the candidate was reporting a cannot-reproduce result. The package made a concrete attempt to reproduce the reported behavior, showed the observed result, and explicitly acknowledged that its padding might not have reached the asymmetric command-line limit. It did not falsely claim that it had reproduced the issue.

My initial check nevertheless treated the unconfirmed precondition as enough to fail `behavior-matched`. The targeted rerun reproduced that disagreement. I revised the check to distinguish between a claimed reproduction, where the relevant trigger and failure mode must match, and an honestly evidenced cannot-reproduce result, where the report must show a concrete relevant attempt, the observed result, and any known difference or unconfirmed precondition. With that revision, `pkg-09` was accepted and matched the gold label.

**Check rationale**

The final `behavior-matched` check in my rubric reads:

> Pass if a claimed reproduction preserves the issue's relevant trigger conditions and the observed artifact demonstrates the same reported behavior or failure mode; a different error does not pass merely because the same component fails. For a cannot-reproduce result, pass if the report makes a concrete, relevant attempt to exercise the reported trigger, shows the observed result, and explicitlyidentifies any known difference or unconfirmed precondition that could explain why the behavior did not occur.

I kept the first part strict because a reproduction should not pass merely because the same component produces some error; the trigger conditions and observed behavior need to correspond to the issue. I added the second part after `pkg-09` showed that the earlier version incorrectly rejected an honest, evidenced cannot-reproduce result when a precondition could not be fully confirmed. The revised wording preserves the wrong-target protection while allowing a cannot-reproduce result when the attempt and its limitations are clearly evidenced.

**Trade-offs**

Allowing an evidenced cannot-reproduce result to pass without proving every precondition reduces false rejections like `pkg-09`, but it creates a risk that a shallow or incomplete attempt could be accepted. I limited that trade-off by requiring a concrete and relevant attempt, an observed result, and explicit identification of known differences or unconfirmed preconditions.

I also re-ran `pkg-02` as a wrong-target canary and `pkg-20` as a repository-conventions/disclosure canary after the change. Both still received their expected `reject` verdicts, while `pkg-09` changed to the expected `accept`. The final full run then reached 20/20 agreement.