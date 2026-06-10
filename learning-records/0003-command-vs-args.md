# command vs args — and when a shell is needed

Completed Lesson 002 exercises. All three pods ran. Surfaced and corrected one
real misconception plus one requirements-reading slip.

## What was missed initially
- **Exercise 2 (`web`)**: omitted `resources.limits` (128Mi/200m). Pod was
  `Running` and `1/1` Ready anyway, so the gap was invisible to `kubectl get
  pods`. Corrected — limits now applied. Lesson: a Running pod ≠ a correct pod;
  always diff the manifest against the requirements, not the pod status.
- **Mental model bug**: believed you must "go into a shell first, then run
  sleep." Wrong — `sleep` is a binary and runs directly with no shell.

## The corrected understanding (verified via quiz, both correct)
- `command:` overrides the image ENTRYPOINT (the executable); `args:` overrides
  CMD (the arguments). Container runs `command + args`. No shell unless you add one.
- A shell (`sh -c`) is only needed for shell syntax: `$VAR` expansion, `&&`/`||`/`;`,
  pipes, redirects (`>`/`>>`), globs.
  - `sleep 3600` → no shell. `echo $GREETING && sleep 3600` → needs `sh -c`.
- `sh -c` form puts the ENTIRE shell line as ONE string in `args`.
- `args` alone relies on the image having no ENTRYPOINT (fine for busybox/alpine).
  `command` is the bulletproof form for arbitrary images.

## Implications for next sessions
- Solid on single-container Pod spec, command/args, env, resources. Ready for
  Lesson 003 — multi-container Pods (init containers, sidecars).
- Reinforce the "Running ≠ correct" habit when grading own work.
- Reference card created: [[command-vs-args]] (reference/command-vs-args.html).
