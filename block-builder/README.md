# Block Builder circuit

The factory for first-party BLKC blocks: describe a job, then generate and publish blocks.

## What it does

1. Operator describes the job and gives examples (e.g. "build an intent classifier for support tickets").
2. The builder asks its configured LLM for a candidate block (manifest + code).
3. Candidate pipeline is tried, in order: hand-rolled rules, then VADER, then spaCy, then SetFit — any one that fires.
4. The candidate is evaluated against a held-out eval set (`evals[]`): metrics, latencies, costs, baseline comparison.
5. A passing candidate is packaged as a new block (manifest v2 + entryPoint + README) in `blocks/`.
6. Nothing is hosted or committed until the `blkc check` + `validatePublishable` gates pass.

## Runbook

- **Describe the job** in the circuit `capabilities[0].config` (a free-form `job` string) and `jobExamples` (an array of example inputs+outputs).
- **LLM keys** are the operator's own. The circuit never stores secrets — it references the operator's environment.
- **Run the candidate** via the `blkc` CLI:

  ```
  blkc check block-builder        # validates the circuit + its kit
  blkc validatePublishable block   # manifest gate
  ```

- **Eval** the block to populate its `evals[]`:

  ```
  blkc block eval blocks/<new-block>/eval-set.json
  ```

- **Publish** when `evals[]` clear the gate. Mark the first-party set with `curation: "bcm"` in `seed.json`.

## Structure

```
block-builder/
  circuit.json    # v2 circuit manifest (this file)
  README.md       # this runbook
  kit/kit.json    # packages the block-builder block
```

## License

MIT — BLKC Core Team.
