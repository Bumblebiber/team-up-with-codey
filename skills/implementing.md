# implementing — Turning one ticket into a change

## Before writing anything

Read the ticket and the spec excerpt together. Then find the code the ticket
names and read that too, including its callers. A small diff in the wrong place
is not a small change — it is a second bug.

If the repository documents how its code should be written (`CONTRIBUTING.md`,
`CODING_STANDARDS.md`, `AGENTS.md`, `CLAUDE.md`, a style section in the README),
read it before your first edit rather than after the review says so. Match the
surrounding code: its naming, its comment density, its idioms.

## Choosing what to write

Take the first option that actually works:

1. Does this need to exist at all? A speculative need is no need.
2. Is it already in this codebase — a helper, a type, a pattern a few files
   over? Reimplementing what exists is the most common waste.
3. Does the standard library do it?
4. Does a dependency the project already has do it? Never add a new one for
   what a few lines can do.
5. Only then: the smallest thing that works.

No abstraction nobody asked for: no interface with one implementation, no
factory for one product, no configuration for a value that never changes.

A bug ticket names a symptom. Fix the cause: check every caller of the function
you are about to touch, and put the guard where all of them route through.
Patching only the path the ticket names leaves every sibling caller broken.

## Tests

Use the `tdd` skill for the loop and for what makes a test worth keeping.

Non-trivial logic — a branch, a loop, a parser, anything touching money or
permissions — leaves behind one runnable check that fails if the logic breaks.
A one-line change does not need a test suite.

Run single test files as you go. Run the project's test action once at the end,
through the command action you were granted; you have no shell.

## Finishing

Commit in your clone, in pieces someone can read. Then write the result: what
changed, what it is tested by, the exact output of the final test run, and what
you are least sure about.

If you cut a corner deliberately — a naive scan, a global lock, a heuristic —
say so at the site in a comment naming the ceiling, and say so again in the
result. A known simplification is a decision; an unmarked one is a defect
waiting to be found by someone with less context than you.
