# looped-recall

**When a pretrained small LM is folded into a looped (recursive) model, does factual recall break in attention or in the FFN?**

Looped language models reuse the same layers several times, adding depth without adding parameters. Prior work reports that looping helps reasoning but hurts knowledge-heavy tasks. This project asks *why*, at the component level.

## Research question

Looping a pretrained model lowers its factual recall. Is that because

- **the knowledge is gone** (the weights that stored it were overwritten), or
- **the knowledge is still there but no longer reachable** (the circuit that retrieves it was damaged)?

## Approach

1. **Probe set.** Collect facts the parent model actually answers correctly (from public datasets such as CounterFact, PopQA, LAMA). Ask each fact several ways: free generation, multiple choice with same-type distractors, one-shot, and with the answer given in context. The gap between generation and multiple choice separates *erased* from *demoted* knowledge.
2. **Component-wise folding.** Fold the middle layers of the same parent in four ways:
   - share attention only (FFN kept per layer, so stored knowledge is preserved by construction)
   - share FFN only
   - concatenate the FFNs into one wide FFN reused across loops with a loop-index gate
   - share everything
3. **Recovery and analysis.** Short recovery training without the probe facts, then trace where recall fails.
4. **Mechanism.** Per-loop low-rank adapters placed only on attention or on the FFN input, compared against layer-wise LoRA at equal parameter budget.

## Plan

A 10-week plan with weekly goals, deliverables, and done criteria is in [ROADMAP.md](ROADMAP.md) (Korean). Week 5 and week 8 are go/no-go points.

## Repository layout

- `ROADMAP.md` why this project exists and weekly goals
- `notes/` weekly reading and experiment notes

Code and data folders are added as the experiments start.

## Status

Week 1 in progress.

## License

MIT
