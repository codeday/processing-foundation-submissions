# Proposal Submission — SOSE 2026

---

## Team Details* 👥

**Mentor's Name:** Hana Gabrielle Bidon

**Team Members:**

| GitHub Username | Full Name            |
|-----------------|----------------------|
| @Danyccsf       | Dany Valverde Caldas |

---

## p5.js Issue* 📋

- **Related graphics context:** https://github.com/processing/p5.js/issues/8953

- Issue Title: Support transformation matrices in p5.strands

- Repository: https://github.com/processing/p5.js

**Proposal status**: This benchmark-reliability proposal is independent of #8953’s assigned transformation-matrix work. I am requesting maintainer approval for this direction; if approved, I will work with maintainers to create or link the appropriate p5.js issue before implementation.

---

## Abstract* 📝

My proposal focuses on improving the reliability of the p5.js benchmarking suite. During local testing, I identified a reproducible defect in benchmark cleanup that emitted Friendly Error System (FES) validation messages while the benchmark still passed. I propose a focused audit of related geometry cleanup code, a maintainer-approved safeguard for unexpected validation messages, and reproducible benchmark guidance. The goal is to improve the diagnostic quality and maintainability of benchmark runs.

---

## Problem Statement* 📑

The p5.js benchmarking suite can report FES validation messages during setup or cleanup without failing the benchmark. In `cpu_transforms.bench.js`, `freeGeometry()` received the `myp5.model` function reference rather than the generated geometry instance. This invalid API usage produced messages while the benchmark continued to report results. After changing the cleanup call to pass the generated `shape` object, isolated WebGL and WebGPU benchmark runs completed without the prior FES messages. This indicates a benchmark-harness defect that should be corrected and used to guide a focused review of related cleanup code.

---

## Proposed Solution* 💡

I propose to improve benchmark cleanup validation through the following work:

- Submit the verified correction to the `freeGeometry()` call in `cpu_transforms.bench.js`.
- Audit related benchmark files that create and release `p5.Geometry` objects for similar API or lifecycle misuse.
- Investigate a maintainer-approved benchmark-specific safeguard that clearly surfaces unexpected FES validation messages during benchmark runs.
- Document a reproducible benchmark command, expected environment, and criteria for a clean benchmark run.

The exact audit scope and safeguard implementation will be confirmed with the maintainers before implementation.

---

## Research on old issues and Maintainer Patterns* 🔭

Issue https://github.com/processing/p5.js/issues/8953 documents ongoing work on p5.strands transformation support across graphics backends. Although the proposal does not implement the assigned transformation-matrix work, reliable benchmarks can support broader graphics evaluation.

I also reviewed Modular FES https://github.com/processing/p5.js/pull/8887. This refactor moved FES into a dedicated module and introduced a shared `FES` interface for emitting messages, including `log`, `warn`, and `error`. Parameter validation now uses this shared interface to create user-facing diagnostics.  

My benchmark finding reveals an analogous unaddressed situation: invalid API usage can emit an FES validation message while a benchmark still passes. I will not assume that FES messages should globally fail benchmarks. Instead, I will work with maintainers to identify a narrow mechanism that detects unexpected validation output in benchmarks without creating false failures.

No existing p5.js issue has yet been identified for the benchmark-harness defect described in this proposal. If approved, I will work with maintainers to create or link the appropriate issue before beginning implementation.

## Impact* 🛠️

- [X] **Bug Fix** — Corrects a verified invalid `freeGeometry()` call in `cpu_transforms.bench.js`.
- [X] **Testing** — Makes benchmark validation problems easier to detect and investigate.
- [X] **Contributor Experience** — Provides clear, reproducible instructions for running the affected benchmark.
- [X] **Maintainability** — Reduces the chance that invalid geometry-cleanup usage persists unnoticed in related benchmark code.

---

## Inclusivity and Accessibility 🤝

This project has no direct end-user accessibility feature. Clearer diagnostics and reproducible benchmark workflows may indirectly reduce contributor friction by making maintenance work easier to understand and validate.

---

## Implementation Plan* ⏳

- **Week 5 (Completed)**: Finalize the proposal, preserve reproduction logs, prepare the one-line fix, and map related benchmark files.
- **Week 6**: Confirm scope with maintainers; decide whether a new issue is needed, and agree on the FES-safeguard approach.
- **Week 7**: Implement the approved fix, focused audit findings, and agreed safeguard, run targeted validation.
- **Week 8**: Submit or refine the PR based on review, add any approved documentation, and prepare the final technical presentation.
---

## Deliverables* 📦

- A pull request correcting the `cpu_transforms.bench.js` cleanup defect.
- Findings from an agreed, focused audit of related geometry benchmark cleanup.
- A maintainer-approved safeguard or reporting improvement for unexpected FES validation messages in benchmark runs.
- Reproducible benchmark-run instructions in the maintainer-approved documentation location.

---

