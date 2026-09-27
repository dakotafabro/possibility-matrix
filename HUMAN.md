# Possibility Matrix - what changes for you

Once this package is installed, your agent answers judgment questions differently. Instead of an answer, you get a table of options, each with a premise. This page is for you, not the agent.

## What you'll see

A short situation, the facts the agent checked, and a table. Each row has a **Holds if** column. That column is where the work is.

## Where you'll see it

Test cases, bug fixes, approach decisions, user journeys, scoping, and triaging findings or review comments. The columns change; **Holds if** is always there.

- **Test cases:** each row is a test. A row you didn't expect is a test you were missing.
- **Bug fixes:** each row is a possible cause. Check the cause before you pick the fix.
- **Approaches:** each row is a design, with the conditions that make it the right call.
- **User journeys:** each row is a path through the flow. The paths you didn't picture are where users get stuck.

## How to read it

1. Skip to **Holds if**. For each row, ask: is this premise true? You usually can't tell yet, and that's fine.
2. Find the row whose premise you can check fastest. Ask for the check, or do it yourself.
3. Pick after the check. Keep your first pick, and notice what moved.
4. Read **What row is missing?** Write your own row if one is. That's the strongest thing you can do with a matrix.

## Why it helps

- **It turns "where do I even start?" into something you can act on.** You don't have to generate the answer space. You only have to check premises.
- **It shows you what you didn't know you didn't know.** A row you wouldn't have thought of is exactly that.
- **It keeps the agent in scope.** The columns define the decision. The agent can't wander into a redesign.
- **It shows the agent's mistakes too.** Every fact has to hold up a row, so a wrong fact gets corrected in the open.

## When to override it

- **The table feels complete.** It isn't, necessarily. The agent can only list what it can see. Ask what's missing.
- **The question feels wrong.** Look for the reframe row. If there isn't one, ask for it.
- **You asked for the agent's lean.** It will anchor you. Ask only after you've read every row, and know that your pick is no longer independent.
- **The question has one factual answer.** Tell the agent to just answer.

## What the agent learns about you

Your picks before and after checks show which premises you tend to miss. A good agent uses that to decide which facts to show first next time.

## How I think (edit this)

This section is yours. The agent reads it before building a matrix, so it can work alongside how you approach a problem instead of replacing it. Keep it short. Delete the prompts you don't need.

- Where I usually start on a new problem:
- What I tend to miss (premises, layers, edge cases):
- Columns I always want (for example: cost, reversibility, who it affects):
- How much context I want before the options (a little / a lot):
- When I want the agent's lean, if ever:
