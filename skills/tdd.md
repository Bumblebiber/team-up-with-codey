# tdd — Test-driven development

The red → green loop. This is what makes it produce tests worth keeping.

## What a good test is

A test verifies behaviour through a public interface, not through
implementation details. The code should be able to change entirely without the
test changing. A good test reads like a specification — "user can check out
with a valid cart" says exactly what capability exists — and survives a
refactor because it does not know the internal structure.

Expected values come from an independent source: a known-good literal, a worked
example, the spec. Never recompute the expectation the way the code computes
it.

## Seams

A seam is the public boundary you test at: where you can observe behaviour
without reaching inside. Tests live at seams.

Test only at seams the ticket or spec agrees on. You cannot test everything,
and agreeing the seams is how the effort lands on critical paths instead of on
every edge case. If the right seam is genuinely unclear, say which one you
chose and why in your result — do not invent a new public interface to make
testing easier.

## Anti-patterns

- **Implementation-coupled** — mocks internal collaborators, tests private
  methods, or asserts through a side channel such as querying the database
  instead of using the interface. The tell: it breaks on a refactor while
  behaviour is unchanged.
- **Tautological** — the assertion recomputes the expected value the way the
  code does, so it passes by construction and can never disagree with the code.
- **Horizontal slicing** — all the tests first, then all the implementation.
  Bulk tests verify imagined behaviour and commit you to a test structure
  before you understand the implementation. Work in vertical slices: one test,
  one implementation, repeat, each cycle informed by the last.

## Rules of the loop

- **Red before green.** The failing test first, then only enough code to pass
  it. No speculative features, no anticipating the next test.
- **One slice at a time.** One seam, one test, one minimal implementation.
- **Refactoring is not part of the loop.** It belongs to review, not to the
  red → green cycle.

## Never weaken a test to make it pass

If a test fails, the implementation is wrong until proven otherwise. Loosening
an assertion, widening a tolerance, or deleting a case to get green is how a
regression ships. If the test itself is wrong, say so explicitly in the result
and explain why — do not fix it quietly.
