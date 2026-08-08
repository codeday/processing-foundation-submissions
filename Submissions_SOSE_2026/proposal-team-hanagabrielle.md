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

- **Link to the issue:** https://github.com/processing/p5.js/issues/9041

- Issue Title: Benchmark cleanup passes `p5.model` to `freeGeometry`

- Repository: p5.js

---

## Abstract* 📝

My proposal fixes a reproducible benchmark-cleanup defect in p5.js. During local testing, I found that `freeGeometry()` received the `myp5.model` function reference instead of the geometry instance created by `buildGeometry()`, emitting a Friendly Error System (FES) validation message while the benchmark continued. The scoped change passes the generated `shape` instance to `freeGeometry()` and is verified through the affected benchmark. This work corrects invalid cleanup usage without changing p5.js or FES behavior more broadly.

---

## Problem Statement* 📑

In `test/bench/cpu_transforms.bench.js`, the cleanup call passed `myp5.model`—a function reference—to `freeGeometry()` rather than the `shape` geometry instance. This invalid API usage emitted an FES validation message while the benchmark continued to report results. Passing `shape` instead removes the observed message in isolated WebGL and WebGPU benchmark runs. Contributors and maintainers need the benchmark to execute its intended cleanup call correctly while evaluating CPU-transform behavior.

---

## Proposed Solution* 💡

I will submit the verified one-line correction in `test/bench/cpu_transforms.bench.js`:

```js
myp5.freeGeometry(shape);
```

I will run the targeted CPU-transforms benchmark in its WebGL and WebGPU configurations before submitting the pull request. This narrow approach directly corrects the reported defect and follows the maintainer’s request to create Issue #9041 before opening a PR.

---

## Research on old issues and Maintainer Patterns* 🔭

Issue https://github.com/processing/p5.js/issues/8953 documents ongoing work on p5.strands transformation support across graphics backends. It is relevant background for the project’s graphics direction, but this proposal does not implement its assigned transformation-matrix work.

I also reviewed https://github.com/processing/p5.js/pull/8887, which modularized the Friendly Error System. My benchmark finding shows that invalid API usage can emit an FES validation message while a benchmark still passes. I will not assume that all FES messages should globally fail benchmarks; this proposal corrects the specific misuse tracked in https://github.com/processing/p5.js/issues/9041.

---

## Impact* 🛠️

- [X] **Bug Fix** — Corrects a verified invalid `freeGeometry()` call in `cpu_transforms.bench.js`.
- [X] **Testing** — Verifies the correction through the affected WebGL and WebGPU benchmark runs.

---

## Inclusivity and Accessibility 🤝

Not applicable — bug fix.

---

## Implementation Plan* ⏳

- **Week 5 (Completed)**: Reproduce the benchmark message, identify the incorrect argument, and prepare the one-line correction.
- **Week 6 (Completed)**: Discuss the finding with @davepagurek and create Issue #9041 as requested.
- **Week 7**: Create the focused pull request, run the targeted WebGL and WebGPU benchmark validation, and request review.
- **Week 8**: Respond to review feedback and document the result in the final technical presentation.

---

## Deliverables* 📦

- A pull request correcting the cleanup call in `test/bench/cpu_transforms.bench.js`.
- Targeted benchmark results for the WebGL and WebGPU configurations showing that the prior FES message no longer appears.

---

## Anything Else?

I discussed the finding with @davepagurek in the p5.js Discord before opening Issue #9041. The implementation is intentionally limited to the maintainer-confirmed correction.
