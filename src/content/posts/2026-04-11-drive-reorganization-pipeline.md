---
title: "20 Years of Backups, Reorganized by 156 AI Agents"
date: 2026-04-11
slug: "drive-reorganization-pipeline"
type: post
description: "522 GB of tangled backups, reorganized by 156 AI agents. Cut to 206 GB. Zero bytes written to the source drive."
categories: ["projects"]
---

## Overview

Consulting engagement. A 522 GB external SSD held roughly 283,000 files — 20 years of the client's consulting work tangled with personal data, grown unnavigable after a decade of "just back it up again" across multiple laptops and cloud sync folders. The client needed a deduplicated, categorized, human-browseable reorganization — without putting the only known copy of two decades of career records at risk.

I built a dedup-and-reorganization pipeline that collapsed 412 GB of hash candidates down to 96 GB of unique content, coordinated a 156-agent hierarchy to classify every canonical file into a new tree, and executed the resulting plan against a scratch workspace on a separate drive. The reorganized tree came in at roughly 206 GB — less than half the original footprint — and the client can browse two decades of their own work again. The source drive was never written to. Not one byte.

## A Copy-Only Workflow

The source is the only reliable copy of two decades of the client's career records. Every decision had to assume the reorganization could be wrong and the client would need the original intact.

Three drives:

- **`H:`** — the original. Read-only forever. Never written to, not once.
- **`I:`** — a fresh SanDisk SSD holding a byte-identical mirror of `H:` via `robocopy /MIR /COPY:DAT /DCOPY:T /R:1 /W:1 /MT:16 /XJ`. 283,058 files, 0 failures. Read-only during execution. Backup integrity was verified by recomputing blake3 on 500 randomly sampled files from the mirror — 500/500 matched the hashes recorded on the source.
- **`D:`** — an internal NVMe. The only writable target. Scratch workspace at `D:/reorg/`. Every mutation is `shutil.copy2` from `I:` to `D:`, never `os.rename` or `shutil.move`.

If the reorganization turns out wrong: delete the scratch directory and start over. Data loss is impossible by construction.

## Finding the Signal

Dedup ground truth comes from content hashing, not filename heuristics. The pipeline is a chain of single-purpose Python scripts, each reading the previous stage's output and writing an append-only checkpoint:

1. **Enumerate.** Walk the drive. 283,013 files across 56,638 directories.
2. **Prescan sizes.** Parallel `stat` with 16 workers, filtering junk (`Thumbs.db`, `.DS_Store`, Calibre metadata, `$RECYCLE.BIN`, AV signatures, Autodesk install templates). 246,155 real files survived the filter.
3. **Size-filter candidates.** Group by (filename, size). 54,156 groups containing 195,332 files.
4. **Hash the candidates.** `hash_candidates.py` parallelizes blake3 across 16 workers with JSONL checkpointing. Roughly 4 hours end-to-end — I/O-bound at ~32 MB/s on the USB SSD, not CPU-bound. Zero failures.
5. **Analyze.** `dir_similarity.py` builds recursive hash sets per directory and clusters them by hash-set equality and Jaccard neighborhood.

The numbers that fell out: 92.7% of hash candidates collapsed into fully-identical groups. 138,996 files — 304 GB — were redundant copies. Only 10,843 of the drive's 39,450 directories (27%) held real content. The other 28,607 were wrapper noise, the same data reached by longer paths through nested backup folders. The two biggest mirror trees alone, two full machine backups with Jaccard 0.995, accounted for roughly 132 GB each.

Of the 412 GB that entered the hashed pool, only ~96 GB was unique. Another ~110 GB bypassed the pool entirely — files with unique (name, size) tuples, filtered out before hashing.

## A 156-Agent Hierarchy

Deduplication answers "which bytes are redundant." It does not answer "where should this file live?" That required semantic categorization of ~66,000 canonical files — too much for a single session to hold in working memory, and not the kind of problem filename pattern-matching solves.

The answer was a two-tier agent hierarchy with a fresh synthesizer on top:

- **145 Sonnet workers** — each responsible for ~1,000 files from one leaf of the canonical tree. Classification, merge suggestions, category proposals, sensitive-content flags.
- **10 Opus bosses** — each synthesizing 14–15 worker reports covering a cohesive section of the tree. Consolidation, conflict resolution, escalation.
- **1 Opus synthesizer** — a separate session with an empty context window, aggregating the ten boss reports into the final classification rules and target structure.

Every agent ran at maximum reasoning budget. Sonnet handled the per-file classification where cost scales linearly with file count. Opus handled synthesis where the decisions were harder and the input counts were smaller. The main orchestrator was also Opus — its only job was dispatching.

`partition_tree.py` walked the canonical tree and produced 145 leaf work units grouped into the ten boss territories. `brief_builder.py` compiled a per-unit JSON brief for each worker containing the assigned subtree, per-subdirectory breakdowns, sampled files, and mirror-cluster annotations. Briefs plus a templated instruction file became the full input each worker would ever see.

## Files as the Communication Layer

This is the part I'd do the same way tomorrow.

The first orchestration attempt tried to put each worker's full instructions inline in the `Agent` tool call. The arithmetic killed it: 145 × ~10 KB of prompt text is 1.5 MB the main session would have to hold just to dispatch the swarm. That evaporates a 1M-token context budget before any real work happens.

The pivot was to treat files as the communication layer. Every agent reads exactly one input file and writes exactly one output file. Permissions enforce it.

- `generate_instructions.py` expanded `instructions_template.txt` into `instructions/worker_W<nnn>.txt` × 145, programmatically. Each file held the full instructions, the target tree, the classification rubric, and that worker's specific filesystem brief — everything the worker needed, and nothing else.
- Each worker's tool allowlist was scoped to one `Read` (its instruction file) and one `Write` (its report path at `reports/worker_W<nnn>.json`). That was the entire permission set.
- The inline `Agent` call from the orchestrator was a single sentence: *"Read your instructions at `/path/to/instructions/worker_W<nnn>.txt` and respond only with the word 'Done'."*
- The worker reads, reasons, writes its structured JSON report to disk, and returns "Done". The orchestrator never sees the prose reasoning. It only sees a one-word confirmation.

The bosses ran on the same pattern. `generate_boss_digests.py` pre-bundled each boss's 14–15 worker reports into a single `boss_digests/boss_B<NN>.json` — replacing 14 individual `Read` calls per boss with one. Boss instructions lived at `boss_instructions/boss_B<NN>.txt`. Read the digest, synthesize, write the consolidated report, return "Done". Same discipline.

The final synthesis ran in a fresh session with an empty context window. It didn't inherit a single token of worker or boss reasoning. It aggregated the ten boss reports through a small Python script rather than through LLM `Read` calls, because each boss report individually exceeded the `Read` tool's 10k-token ceiling.

This is principle of least privilege applied to LLM orchestration. Every agent can touch exactly what it needs and nothing else. No agent's context pollutes another's. The main orchestrator's context stays under control because the only state it carries is which worker finished. Deterministic Python aggregation replaces LLM synthesis wherever the inputs are structured — the model is only paid for the parts that need judgment. I'm curious whether a more direct inter-agent approach could collapse the file-passing ceremony without reintroducing the context-flooding problem that made files necessary in the first place. I haven't found one yet.

## What the Swarm Produced

All 145 worker reports returned JSON-valid with every required field present. All 10 boss reports returned `confidence_level: high`. The consolidated output:

- **80 classification rules**, ordered most-specific to least-specific, first-match wins
- **29 proposed new categories** where the bosses agreed the strawman tree was too narrow
- **11 smart merges** — multi-source directory merges with collision handling via `(target_path, hash)` merge keys
- **10 escalation groups** consolidating 120 individual sensitive-content flags from the workers

The target tree expanded the client's work archive from a handful of strawman categories to more than fifty named historical engagements the bosses surfaced independently from the corpus. Three bosses independently agreed to drop a 132 GB wrapper folder wholesale after recognizing it as a Jaccard-0.995 duplicate of another machine backup.

The encouraging signal was workers reasoning beyond their narrow assignments. Worker W042 was given a single media subtree and, on its own initiative, flagged two things outside its scope: a batch of buried financial records it recommended promoting to a dedicated personal category, and a cluster of sensitive files it set aside for direct owner review. That kind of unprompted judgment is what made running the swarm worth it instead of writing a deterministic rule engine.

## Three Failures That Became Patterns

**Subagent tools inherit. Permission mode doesn't.** An earlier phase of the project tried to parallelize a search with 96 agents that had `Bash` available. They all failed silently — subagents can't prompt a user for tool permission, and `Bash` requires prompting. The workaround — strip `Bash`, give them nothing but `Read`, feed them pre-generated text files — became the template for the entire worker pattern. The constraint that killed the first attempt shaped the architecture of the second.

**A filter in a dedup tool silently dropped 41,979 files.** An initial version of `partition_tree.py` had an `under_mirror` filter intended to skip duplicate wrapper paths. It conflated "wrapper chain" with "mirror" and quietly excluded real content. I caught it during validation, not during testing — the file counts at the top of the pipeline didn't match the counts at the bottom. The fix was to delete the filter and let canonical-path scoring `(depth, wrapper_score, length, alpha)` handle wrapper preference naturally. Silent content loss in a tool called "dedup" is the worst possible failure mode. Validation counts at every stage exist for exactly this.

**The `Read` tool has a 10k-token ceiling I didn't know about until I hit it.** The final synthesis step needed to read all ten boss reports to produce the classification rules. Every one exceeded the ceiling. The pivot was to write a small Python aggregator (`analyze_boss_reports.py`) that deserialized all ten reports, merged their structured fields, and emitted a single human-readable summary the synthesizer could work with. When inputs are structured, deterministic aggregation beats LLM synthesis. You only pay the model for the parts that need judgment.

## The Outcome

The pipeline ran to completion. `build_copy_plan.py` turned the 80 classification rules into a 66,098-file plan. `validate_copy_plan.py` confirmed zero collisions, zero coverage gaps, and no files falling outside their explicit buckets. `report_copy_plan.py` produced the human-readable audit. The client weighed in on the ten content-ownership calls the escalation list had surfaced — judgment calls only the owner could make — in a single afternoon. Then `execute_copy_plan.py` copied the canonical tree from the `I:` mirror into the `D:` scratch workspace. Three verification layers ran before anything left internal storage: `D:` hashes against their recorded blake3s, every baseline hash accounted for in `D:`, and `D:` compared against `H:` directly. All three passed.

The final tree came in at roughly 206 GB — a 61% reduction from the 522 GB starting point. The client now has a browsable drive with more than fifty named historical engagements spanning twenty years of consulting work, a cleanly separated personal archive, and an `Unsorted/` bucket that caught every file the rule engine couldn't confidently place so nothing was lost in the margins. A task this size — 283,000 files, 20 years of history, 66,000 distinct canonical files needing semantic placement — is not something a person has time to do by hand. With the leverage of a 156-agent pipeline and a file-based communication layer, it became a few hours of orchestration plus compute.

The reaction was the part I hadn't planned for. The client had written the drive off as lost territory — too big to sort by hand, too important to delete. Getting back a navigable version of twenty years of their own work, at less than half the size, with the original still byte-for-byte intact, read to them as something they hadn't thought possible. They told me more than once they didn't know AI could do this kind of work at this scale. The interesting thing from my side is how routine the tech made the task — not heroic, not clever, just a pipeline that understood where the leverage was.

## What I'd Do Next

I'd explore whether a more direct inter-agent messaging approach could shed the file-passing ceremony without reintroducing the context-flooding problem that made files necessary in the first place. I'd also package this orchestration pattern as a reusable Claude Code skill — the prompt template, the partitioning heuristic, the file-based invocation — because the same shape fits a lot of other "too big for one context" problems.
