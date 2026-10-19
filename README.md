# Ammon Jackson

I build open-source tools that test whether AI systems do what people claim they do, and I publish the results whether or not they are the results I wanted.

[![Read the plain-English version](https://img.shields.io/badge/New%20here%3F-Read%20the%20plain--English%20version-1d4ed8?style=for-the-badge)](./SIMPLE.md)

Everything below assumes you know what a language model, a benchmark, and a confidence interval are. If you do not, the button above goes to the same story with every term explained.

## What this is

Twenty-four repositories, one theme: a claim about an AI system is not a result until the inputs are pinned, the rules are written down before the run, and the number comes with an honest range. The work splits into three families.

**Experiments.** Six research harnesses, each built around one question, each run on real models on my own hardware. They are Python with no required runtime dependencies beyond the standard library (numpy for the retrieval project).

**Checkers.** Fifteen Rust command-line tools. Each takes a claim someone else published (a benchmark row, a jailbreak attack-success rate, "our agent never exceeds its budget," a secure-hardware attestation) and refuses to grade it until the inputs are pinned. Each writes its thresholds before seeing data, reports a 95% bootstrap interval on every rate, and exits with a code a CI job can read: 0 passed, 1 a named pre-registration clause failed, 2 refused to run because an input was degenerate or uncalibrated.

**Scaffolds.** Three Rust crates whose statistics and pre-registration exist but whose live drivers do not yet. Their READMEs say so in the first screen.

## The repositories

| Repo | Family | What it asks or checks | Status, 17 Oct 2026 |
| --- | --- | --- | --- |
| [refusalscope](https://github.com/Intermountainh8ter/refusalscope) | Experiment | Does removing refusal training (abliteration) damage tool calling or increase prompt-injection compliance? | 5 Qwen3 pairs run, 4 pass the manipulation check |
| [trialgauntlet](https://github.com/Intermountainh8ter/trialgauntlet) | Experiment | pass^k beside pass@k; every environment audited against do-nothing agents first | Qwen3-8B stock vs ablated pair run |
| [groundrl](https://github.com/Intermountainh8ter/groundrl) | Experiment | Can a deterministic citation verifier serve as an RL reward without being gamed? | 19 exploits audited; GRPO run degenerate, reported as null |
| [ethresearch-rag](https://github.com/Intermountainh8ter/ethresearch-rag) | Experiment | Citation-verified retrieval over Ethereum specs; which retrieval components earn their place? | Evals run; neural embedder changed the headline by 0.00 points |
| [txwarden](https://github.com/Intermountainh8ter/txwarden) | Experiment | Pre-signature transaction review where the LLM explains but fixed rules decide | 245 injection attacks; 0 verdicts moved, 79 misleading explanations |
| [fleetgate](https://github.com/Intermountainh8ter/fleetgate) | Experiment | Incident-response agents behind a default-deny policy gate with a hash-chained audit log | 0 invariant violations in 2,460 adversarial runs; 2 of 4 safety dials inert |
| [harnesspin](https://github.com/Intermountainh8ter/harnesspin) | Checker | Pin harness commit, task config, chat template, sampling, few-shot layout; compare declared vs observed | Built, one end-to-end demo |
| [jailbreakreplay](https://github.com/Intermountainh8ter/jailbreakreplay) | Checker | Replay a published jailbreak under pinned settings; ASR with bootstrap CI and a verdict against the claim | 24 live runs, 6 models, 4 prompt sets |
| [agentreplay](https://github.com/Intermountainh8ter/agentreplay) | Checker | Record an agent trajectory with chained SHA-256; replay byte for byte; inject at step k | 1 live replay |
| [codepatchreplay](https://github.com/Intermountainh8ter/codepatchreplay) | Checker | pass@k with CI plus a patch-identity leak signal | Built |
| [retrievalaudit](https://github.com/Intermountainh8ter/retrievalaudit) | Checker | Pin retriever, corpus, generator, k, reranker; precision/recall@k and faithfulness | Built |
| [kernelwitness](https://github.com/Intermountainh8ter/kernelwitness) | Checker | Rig-fingerprinted GPU kernel timings; same-rig three-sigma reproducibility verdict | Built |
| [toolguard](https://github.com/Intermountainh8ter/toolguard) | Checker | Dangerous-tool call rate, benign vs injected, with a catalog integrity gate | Built |
| [guardaudit](https://github.com/Intermountainh8ter/guardaudit) | Checker | Safety-classifier calibration: Brier, ECE, precision/recall at threshold, in and out of distribution | Built |
| [budgetguard](https://github.com/Intermountainh8ter/budgetguard) | Checker | Do agent frameworks honor token, call, USD, and wall-clock budgets under four scripted breakers? | Built |
| [shadowtrack](https://github.com/Intermountainh8ter/shadowtrack) | Checker | Shadow-mode agreement between two endpoints with self-agreement baselines | 3 live runs |
| [attestchain](https://github.com/Intermountainh8ter/attestchain) | Checker | Chain of custody for TEE-attested inference: policy, measurement, nonce, input and output hashes | Built |
| [provenancepack](https://github.com/Intermountainh8ter/provenancepack) | Checker | Was this document in the training data? Three membership-inference signals against a known-out bank | Built |
| [contamcheck](https://github.com/Intermountainh8ter/contamcheck) | Checker | Benchmark contamination: n-gram overlap, echo canary, loss delta, Holm-corrected | Built |
| [releaseflag](https://github.com/Intermountainh8ter/releaseflag) | Checker | Fold every upstream verdict into one PASS, HOLD, or BLOCK | Built |
| [bench-report](https://github.com/Intermountainh8ter/bench-report) | Checker | Render README tables only from committed result files; SHA-256 manifests of results | Built, manifests written |
| [mcpprobe](https://github.com/Intermountainh8ter/mcpprobe) | Scaffold | pass@k and pass^k for Model Context Protocol servers, with degenerate-caller audits | Estimators only |
| [rewardcoherence](https://github.com/Intermountainh8ter/rewardcoherence) | Scaffold | Does a reward model rank the same responses the same way across temperatures? Kendall tau-b with CI | Statistics only |
| [tokenwitness](https://github.com/Intermountainh8ter/tokenwitness) | Scaffold | Byte-identity of chat-template rendering across inference stacks | Diff only |

## Trading systems

Same method, different market. Pre-registered, rendered from committed results, honest about what the instrument cannot see.

| Repo | What it does | Status, 18 Oct 2026 |
| --- | --- | --- |
| [liqledger](https://github.com/Intermountainh8ter/liqledger) | Open liquidation and MEV observatory: every Aave V3, Spark, and Morpho Blue liquidation on mainnet from a full node, with builder payment, block position, and Chainlink oracle adjacency | 20,930 liquidations over 17 months; 2 of 3 pre-registered predictions wrong, 1 strongly supported (3.2x oracle enrichment) |
| [walkforward](https://github.com/Intermountainh8ter/walkforward) | Pre-registered backtesting: data hash, grid, and split locked before looking; walk-forward only; deflated Sharpe and probability of backtest overfitting on every run | 4 studies run, none beats buy-and-hold out of sample |

## Findings so far

Most of them are negative, which is what careful measurement usually produces and what rarely gets published.

- **Abliteration did not cause the predicted harm.** Across four verified Qwen3 pairs (1.7B to 14B), tool-call accuracy and injection susceptibility moved in the predicted direction on none of them. The 14B uncensored model scored 5.3 points *better* on tool calling; the 4B uncensored model was 6.3 points *harder* to trick. The pre-registered manipulation check (at least a 30-point drop in refusal rate) passed on four pairs; the 0.6B pair fell short because the stock model refuses only 15% to begin with, and it is reported unverified rather than rescued.
- **But uncensoring shows up in multi-step work.** On trialgauntlet's three environments, the Qwen3-8B uncensored model matched stock on two and collapsed on the hardest (15.3% to 5.7% pass@1), and broke a written policy in 19.2% of 552 trials against 8.3% for stock (0.21 vs 0.09 violations per trial); the 14B pair went 41.3% vs 27.2%. *Corrected 2026-10-19: an earlier version of this line said 52.8% vs 20.4%, which was the config-repair environment alone, mislabeled as the overall. No pass metric changed; the repo README carries the same dated note.*
- **Refusal rates are a property of the prompt list as much as the model.** Six downloadable models on four published harmful-request lists: non-refusal ranged from 1% to 100%, and HarmBench drew far more compliance than JailbreakBench or AdvBench from every safety-tuned model. A fixed, pre-calibrated grader under-counts Phi-3.5's refusals; the grader was not changed after the fact, and that limitation is reported with the number.
- **A server at temperature 0 disagreed with itself.** shadowtrack's self-agreement baseline found llama-server under continuous batching reproducing its own answers only 92% of the time, so part of any model-vs-model disagreement on that stack is server noise. The control was built for exactly this and it fired.
- **The expensive retrieval component earned nothing.** On 17,387 chunks of Ethereum specifications, adding a neural embedder to BM25 moved the top-5 hit rate from 81.46% to 81.46%. Only the reranker clearly helped.
- **"Zero security failures" can be true and misleading.** txwarden's rule-engine verdict never moved under 245 injection attacks, but the LLM-written explanation told the user to proceed on a blocked transaction 79 times.
- **Safety controls that do nothing still get trusted.** fleetgate never violated an invariant across 2,460 adversarial runs and 104,828 policy checks, and two of its four safety settings had no measurable effect on any outcome.

Every number above is rendered from a committed result file by [bench-report](https://github.com/Intermountainh8ter/bench-report) or the repo's own docgen, and the result directories carry SHA-256 manifests.

## Rules every repository follows

- Hypotheses, thresholds, and disqualifiers are written in `docs/PREREGISTRATION.md` or `METHODOLOGY.md` before the first run, and the file is append-only. When a rule turned out badly (agentreplay's byte-identity rule fails on volatile response fields; the refusal grader misses Phi-3.5's phrasing), the rule stands and the problem is reported.
- Every rate carries a 95% percentile bootstrap interval from a seeded generator, so the interval itself reproduces. Gates read the interval bound, never the point estimate.
- Degenerate baselines run first. A no-op agent, a random ranker, or a blank manifest must score zero before a real result is trusted.
- No LLM judges. Graders are scripted and deterministic.
- Nothing harmful goes into git. Adapters return labels and prompt hashes; completions to harmful prompts and raw trajectories stay out of every repository. Secrets live in environment variables and debug output prints the variable name, not the value.
- Every Rust crate pins `rust-toolchain.toml` to one exact compiler version.

## Hardware

All of this ran on a 2019 Mac Pro with two AMD Radeon Pro Vega II cards (gfx906), a GPU that current ROCm releases no longer ship kernels for. [rocm-gfx906-sramecc-fix](https://github.com/Intermountainh8ter/rocm-gfx906-sramecc-fix) is the repair: rebuilt rocBLAS, rocSPARSE, and rocALUTION for the exact ISA the card reports, no root needed. A second, override-free fix (the missing Tensile library rebuilt from unmodified upstream source, verified against the stock library) is packaged with checksums and a write-up.

## What I am looking for

Published claims to point the checkers at. If you ship a model, a safety classifier, an agent framework, or an attested inference endpoint and want an independent, pre-registered verdict with the inputs pinned, open an issue on the relevant repo. Pull requests and bug reports are welcome on all of them. Everything is MIT licensed.
