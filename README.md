# Lecture 10-2 · PSL migration demos

These examples use the ACME project plan and `Task.title` from Lecture 9.
The activity migration adapts Eclipse Epsilon’s “Migrate Project Plan” example.

## Set up the Epsilon Playground

Open https://eclipse.dev/epsilon/playground/?flock in three browser tabs.
This link opens the built-in Flock workspace, **not** the course example.
Use a different folder from the ZIP in each tab and replace all four editor contents:

| Playground pane | Paste this file |
| --- | --- |
| Original Metamodel | `original.emf` |
| Original Model | `acme.flexmi` |
| Evolved Metamodel | `evolved.emf` |
| Migration (Flock) | `copy-only.mig` for Demo 1’s first run; otherwise `migrate.mig` |

Run with the program pane’s Run button (or Ctrl+S / Cmd+S). Inspect the
Migrated Model diagram and the console. If the example needs to be reloaded,
paste the files again; each folder is independent. No snapshot or live share is needed.

## What to show

1. **Copy and rename** (slide 6): With `copy-only.mig`, the three Tasks
   survive and `start`, `duration`, and the Effort links are present.
   `Task.name` is unset (the diagram may omit it). Replace the program with
   `migrate.mig` and rerun. The task names become Analysis, Design,
   and Implementation. Analysis is still assigned to Alice.
2. **Evolve the schedule** (slide 10): Predict and then inspect the three Activities
   under ACME. Analysis is [1, 4), Design is [4, 10), and Implementation is
   [7, 10). The project still contains Alice and Bob. The four Efforts still point
   to migrated people; Implementation has Bob and Alice at 50% each.
3. **Remove placeholders** (slide 12): The input adds a synthetic Bob 0% Effort
   to Implementation. The guard removes only this Effort. Compare five original
   Efforts with four migrated Efforts; both people and all three schedule
   elements remain. For a before/after run, temporarily comment out the
   `delete Effort` line, run, then restore it and rerun.

If Playground is unavailable, the three demo slides include expected results.
The four runs were also checked with the local Eclipse Epsilon 2.8.0 engine.

## Bonus: Full Flock construct tour

Use `04-full-suite` in the same four Playground panes, or choose **4. Full
Flock construct tour** from the custom example menu once this revision is
published. The Playground uses the self-contained `migrate.mig`;
`helpers.eol` is only for the optional local import exercise.

ACME is moving its schedule to a new package, while preserving milestones and
owners and discarding temporary material. Watch the Console as well as the
migrated model diagram.

| Construct | What to look for |
| --- | --- |
| Named `pre` and `post` | The source count prints before migration; the result summary prints afterwards. |
| `@strict retype` | The exact `Task` becomes `Activity`; `Milestone` keeps its own type. Remove `@strict` and the milestone count drops to zero. |
| `retype package` | Same-named classes such as `Project` and `Milestone` move from `psl` to `schedule`. |
| `@cascade delete` and `when:` | The obsolete draft and its note are omitted. The current draft and its note survive. Remove just `@cascade` and both notes get equivalents. |
| `delete package` and mapping order | Within `psl`, the guard selects scratch memos for deletion before the fallback can convert its memo into an ordinary artifact. Comment out just the package deletion and the temporary import joins the published charter. |
| `migrate ... ignoring` | The source status `approved` is not copied; the evolved default is `review`. |
| `migrate` with a block guard and EOL operation | Tasks with nonnegative durations get an exclusive end. The rule also applies to the milestone subtype. |
| `equivalent()` and `::=` | The first reconnects the collection of activities; the second reconnects each lead to a migrated person. |

Expected console output:

```text
Original tasks (including milestones): 2
Status: review
Analysis: [1, 4), lead: Alice
Sign-off: [4, 4), lead: Bob
Milestones: 1
Draft notes: 1
Scratch memos: 0
Artifacts: Published charter
```

**Optional file-based import:** In a local copy of `migrate.mig`, remove the
inline `exclusiveEnd` operation and add `import "helpers.eol";` at the top.
Run that file with the local Epsilon engine and keep `helpers.eol` beside it.
The same migration result should be produced.
