# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->
I am an early-career contributor working through an issue carefully before proposing changes. I communicate what I have actually verified, separate evidence from assumptions, and make my work easy for maintainers to check or reproduce.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->
### Rule: Say only what I verified

I distinguish between what I observed and what I currently suspect. I do not present an interpretation or likely cause as established fact unless my evidence demonstrates it.

- Wrong: "I confirmed that the decoder is causing the crash."
- Right: "I reproduced the crash and observed the stack trace reaching the decoder; I have not yet confirmed the underlying cause."

### Rule: Name the specific evidence

When reporting a reproduction result, I state the concrete behavior I observed instead of summarizing it with vague phrases such as "same issue" or "it doesn't work."

- Wrong: "I tested this and got the same problem."
- Right: "Running the command with the reported input produced `panic: not a string`, matching the failure described in the issue."

### Rule: Promise investigation, not a fix

When claiming an issue before reproducing it, I say what I plan to investigate and report back on. I do not promise that I will fix it, predict the solution, or give a completion date before I understand the bug.

- Wrong: "I'll fix this in `decoder_hcl.go` and have a PR ready tomorrow."
- Right: "I'd like to investigate this issue. I'll reproduce the reported behavior first and follow up here with the environment, steps, and result."

### Rule: Report mismatches directly

If my result differs from the issue, I say so instead of treating any failure in the same component as confirmation.

- Wrong: "The command failed, so I confirmed the reported bug."
- Right: "The command failed, but with a different error than the issue reports, so I have not reproduced the reported behavior yet."
### Rule: Separate the plan from certainty

When posting an implementation plan, I can state what I intend to
change, but I do not present unverified implementation details as
facts. If part of the approach still depends on something I need to
confirm while building, I name that uncertainty.

- Wrong: "The refresh callback is definitely the only code that needs to change."
- Right: "I plan to update the refresh callback; if implementation shows another refresh path is involved, I'll note that deviation before expanding the scope."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
- A claim that I reproduced a bug when the evidence shows a different failure.
- A promise that I will deliver a fix or PR by a particular date before investigating.
- A guessed root cause presented as confirmed.
- "Same as above" or another contributor's reproduction presented as my own evidence.
- Boilerplate that does not identify the specific issue or what I actually tested.
