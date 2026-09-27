---
name: possibility-matrix
description: >
  When a question has more than one defensible answer, lay out the answer space
  as a matrix with a "Holds if" premise per row, instead of giving one answer or
  asking an open question. The human picks by checking which premise is true.
author: dakotafabro
version: "0.1.0"
tags:
  - decision
  - judgment
  - test-cases
  - bug-fixes
  - user-journeys
  - scaffolding
  - human-in-the-loop
---

# Possibility Matrix

## When to use it

- The human asks "what are the options?", "what are the possible answers?", or "where do I even start?"
- A judgment question where a bare "what do you think?" would stall, because the answer depends on premises the human can't see yet.
- Test design: each row is a case (input or state, expected outcome, what it proves).
- The human's first answer rests on a premise the code or data contradicts. Put the right premise in a row so they find it rather than being told.

Don't use it for questions with one deterministic answer. A matrix around a fact is noise.

## Use cases

The matrix was designed for any decision with more than one defensible answer. The columns change with the decision; **Holds if** and **Means for ...** are always there.

- **Test cases.** Columns: Case / Input or state / Expected / Proves. Each row is a test; **Holds if** is the assumption the test encodes, so a missing row is a missing test.
- **Bug fixes.** Columns: Root cause / Fix / Blast radius. Rows are competing explanations of the bug; **Holds if** is what you'd have to observe for that cause to be the real one. Pick the fix after the cause is checked, not before.
- **Approach considerations.** Columns: Approach / Cost / Risk / Reversibility. Rows are designs; **Holds if** is the condition under which each one is the right call (load, timeline, team familiarity).
- **User journeys.** Columns: Path / User state / What they see / Where it breaks. Rows are the routes a user can take through a flow (first run, returning, offline, interrupted, double tap); **Holds if** is the state that puts them on that path.
- **Labeling and triage.** Columns: Real? / Fix? / Why (for findings or review comments). Rows are readings of the finding; **Holds if** is what the code must show for that reading. This is where the matrix was first used.
- **Scoping and placement.** Columns: Where it lives / What it couples / What it costs later. Rows are candidate layers or modules.

Across all of them the move is the same: the human stops producing the answer space and starts checking premises.

## Shape

Every matrix has these parts, in this order.

1. **The situation.** The code or context, with lettered markers (A, B, C) on the lines that matter, and a short timeline of the sequence in question.
2. **Facts behind the options.** What is verified, with a file:line or source. Anything unverified is labeled "not checked".
3. **The matrix.** One row per option, each with an ID (`1a`, `1b`, ...). The first columns are the decision's dimensions (for example Race / Fix / Why, Approach / Cost / Risk, or Input / Expected / Proves). Then always:
   - **Holds if** - the premise that must be true for this row to be right.
   - **Means for ...** - the downstream consequence (for the design, the tests, the rule, the team).
4. **The human's prior answer as a row**, if there is one, with its premise stated plainly ("Holds if line C was emitted. The code shows it wasn't."). Don't mark it wrong. Let the premise do the work.
5. **An "unsure / check first" row**, naming the specific check that would settle it.
6. **A "reframe" row** when the question rests on an assumption: "the question assumes X; if X is false, the real question is Y."
7. **"What row is missing?"** as the last line before the blanks.
8. **Blanks:** Pick, Run check first?, Notes.

## Agent rules

- **Read the human's "How I think" section first.** If `HUMAN.md` has a filled-in "How I think (edit this)" section, use it: start where they start, show first the premises they tend to miss, include the columns they always want, and follow their preference on leans. Never overwrite that section.
- **Verify before you write the options.** Read the callers and callees first, so no "Holds if" needs correcting after the pick.
- **Correct yourself in the open.** If a check overturns a fact, write "Correction to the facts above" and name the rows it affects.
- **Neutral options.** No recommendation in the matrix. If asked for a lean, give it only after the human has seen every row, and record that a lean was given. A lean anchors the pick.
- **Stay inside the columns.** The columns are the scope of the decision. Don't widen it with redesigns or adjacent refactors. If the real issue is adjacent, use the reframe row.
- **Record before and after.** If the human runs a check and re-picks, keep both picks. The shift shows the premise moved, not just the answer.
- **Deliver as a rendered markdown file** when the matrix has a table. Tables don't render in a terminal.

## Signals of growth

- Picks a row, and the pick matches the verified premise.
- Challenges a row's premise, asks for a check, or writes a row that isn't there.
- Adds a new column, or reuses the shape unprompted for a different decision.

## Failure modes

- **False completeness.** The matrix is only as good as the agent's list. The "what row is missing?" line exists for this.
- **Wrong question.** The reframe row exists for this.
- **Anchoring.** Options frame thinking, and a lean frames it more. Keep leans out of the matrix.
- **Over-constraint.** Scope-bounding can hide an adjacent insight. The reframe row is the escape hatch.

## Human counterpart

This package changes how your agent answers judgment questions: you'll get options with premises instead of an answer. `HUMAN.md` explains what to look for, when to override, and how it can mislead you.

See `examples/double-tap.md` for a worked example.
