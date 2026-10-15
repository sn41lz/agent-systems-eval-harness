# 05 · Agent Evaluation Harness

Reproducible trajectory evaluation with calibrated judge scoring, versioned benchmarks, and CI regression gates.

**Status: planning — implementation has not started.** This README describes intended scope and acceptance criteria. It does not establish working behavior, completed tests, security guarantees, or measured performance.

Week 5 of an eight-repository agent-systems portfolio.

## Planned scope

- Versioned task fixtures, tool trajectories, deterministic assertions, and scoring rubrics.
- LLM-as-a-Judge calibration against a held-out human-labeled set.
- Regression reports and GitHub Actions gates with documented thresholds and uncertainty.

## Planned setup and observability

The implementation is intended to provide a `docker-compose.yml` and `Makefile` so a reviewer can use `make demo` for a deterministic offline demonstration and `make test` for offline regression checks. **These commands and their supporting files are not implemented yet.** Live-model runs will be optional and separately documented with configuration and cost assumptions.

Every model call and tool execution is intended to emit an OpenTelemetry span. Planned aggregate reporting includes token usage, estimated USD per 1,000 requests, p50/p95 latency, and p50/p95 time to first token (TTFT) where streaming applies. Offline fixtures can verify instrumentation and behavior; live-provider performance requires separate measurements.

Implementation choices will remain inspectable, with explicit contracts and documented dependencies.

See [PRODUCTION_CHECKLIST.md](PRODUCTION_CHECKLIST.md) for shared acceptance criteria.

## Proposed architecture and tradeoffs

A runner records task inputs, agent trajectories, and artifacts in a versioned result schema. Deterministic graders check objective properties before an optional model judge applies a rubric. An aggregator reports quality and operational metrics, and CI evaluates declared regression thresholds.

Trajectory grading can diagnose failures but requires stable schemas and careful rubrics. Model judges add cost, variance, and bias. Strict CI gates can block useful changes when the benchmark is small or noisy, so thresholds need evidence and versioning.

## Roadmap and acceptance criteria

Complete the shared reproducibility and observability checklist, then implement and verify these project-specific criteria:

- [ ] Replay fixed trajectories and produce identical deterministic scores offline.
- [ ] Keep judge calibration data separate from evaluation data and report agreement and disagreement categories.
- [ ] Record rubric, judge model, agent configuration, dataset revision, and repetitions in each result.
- [ ] Demonstrate that a known quality regression fails CI while an unchanged deterministic run passes; document the initial gate policy before enabling it.

## Candidate failure modes

These are design risks to investigate, **not tested or observed findings**:

- A benchmark leaks into prompts or calibration data and inflates apparent quality.
- A judge rewards verbose or persuasive answers despite task failures.
- Flaky tasks, unstable external services, or small samples trigger misleading regression alerts.

The implementation will add reproduction cases, actual outcomes, and mitigations as evidence becomes available.

## Baseline versus optimized metrics

No benchmark has been run. The following comparison is planned:

- **Baseline:** A fixed agent revision graded with the initial deterministic checks and rubric.
- **Optimized:** A declared candidate revision or grading improvement evaluated on the identical held-out tasks.

| Metric | Baseline | Optimized |
| --- | --- | --- |
| Input/output/total tokens per request | Not yet measured | Not yet measured |
| Estimated USD per 1,000 requests | Not yet measured | Not yet measured |
| End-to-end latency p50 | Not yet measured | Not yet measured |
| End-to-end latency p95 | Not yet measured | Not yet measured |
| Streaming TTFT p50, where applicable | Not yet measured | Not yet measured |
| Streaming TTFT p95, where applicable | Not yet measured | Not yet measured |
| Project-specific quality and failure rate | Not yet measured | Not yet measured |

Future results must identify the workload revision, model/provider configuration, environment, concurrency, sample counts, and quality criteria. Cost estimates must disclose pricing date and assumptions. Report the complete workload, including errors and retries, and retain raw results so baseline and optimized runs can be reproduced. Do not substitute target values for measurements.

## Portfolio

- [01 · Raw Agent Tool Loop](https://github.com/sn41lz/agent-systems-01-tool-loop)
- [02 · Context Window Engine](https://github.com/sn41lz/agent-systems-02-context-window)
- [03 · Hierarchical Agent Memory](https://github.com/sn41lz/agent-systems-03-hierarchical-memory)
- [04 · Multi-Agent Orchestrator](https://github.com/sn41lz/agent-systems-04-multi-agent-orchestrator)
- [05 · Agent Evaluation Harness](https://github.com/sn41lz/agent-systems-05-eval-harness)
- [06 · Secure Code Sandbox](https://github.com/sn41lz/agent-systems-06-secure-sandbox)
- [07 · Durable Agent Runtime](https://github.com/sn41lz/agent-systems-07-durable-runtime)
- [08 · Agent Platform API and SDKs](https://github.com/sn41lz/agent-systems-08-api-sdk)
