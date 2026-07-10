---
name: verify
description: Use after making a non-trivial change to product behaviour, before you call it done - drive the affected path end-to-end and observe the real result, not just a green typecheck or a passing unit test. Do not use for changes with no runtime surface (docs, comments, pure test-only edits) or for pure questions.
---

# verify: green tests are evidence, not proof

A clean typecheck proves the code compiles. A passing suite proves the cases someone already thought of still hold. Neither proves *this* change does what was asked — that gap is exactly where "done" turns out to be wrong. Verification is exercising the real path a user or caller takes and watching it behave.

## When the gate applies

Fires after any change to product behaviour, before declaring it done. Skip it for changes with no runtime surface — docs, comments, pure test-only edits — and for pure questions. A change to product *source* always has a runtime surface; find it.

## The loop

1. **Name the observable first.** One sentence: "If this works, then `<observable behaviour>`." Cannot name one? You do not understand the change well enough to verify — or to have made it.
2. **Drive the real path.** Run the command, hit the endpoint, load the screen, call the function with representative inputs. The thing itself, not a mock of it.
3. **Observe — do not assume.** Read the actual output, state, or render. "Should work" is not an observation.
4. **Compare to step 1.** Match → done. Mismatch → not done; fix and re-run the loop.

## What "drive the real path" means

- **CLI:** run it with real args; read stdout *and* the exit code.
- **HTTP:** send the request; inspect status *and* body.
- **UI:** load the view and look at what rendered (capture a screenshot if you cannot see it directly).
- **Function/library:** call it from a scratch harness with representative inputs.
- **Data write:** read the record back after writing it.

## Tests are part of this, not a substitute for it

Add or keep the test that would have caught this bug. But a passing suite stands in for driving the path *only* when a test already exercises this exact path with this exact assertion — and usually it does not yet. Write that test, then still run the real thing once.

## The honest failure mode

The tempting shortcut: typecheck passes, tests pass, ship. That catches regressions in what was already covered and is blind to the distance between "what I built" and "what was asked". One real run closes that distance for the price of a minute.
