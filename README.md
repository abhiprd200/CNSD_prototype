# CNSD — Causal Neuro-Symbolic Diagnosis

> A framework for interpretable, causally-grounded fault and anomaly diagnosis across mechanical and biomedical signal domains.

---

## Overview

CNSD integrates four computational layers into a single bidirectional architecture:

1. **Neural layer** — WDCNN + S-JEPA dual-encoder backbone for signal representation
2. **Symbolic layer** — Rule engine mapping CNN activations to human-readable fault taxonomy
3. **Causal layer** — Backdoor-adjusted ATE estimation (Pearl's Rung 2) + DoWhy counterfactuals (Rung 3)
4. **Consensus layer** — Bidirectional feedback where causal risk history modulates neural confidence thresholds

The main novel contribution is **CCR-LoRA** (Causal Consistency Regularized LoRA) — a continual learning method that bounds ATE drift via a causal consistency penalty while allowing partial base network adaptation.

| Method | Old-Task Acc | New-Task Acc | ATE Drift |
|--------|-------------|-------------|-----------|
| Naive fine-tuning | 0.944 | 1.000 | 0.246 |
| EWC (λ=100) | 0.965 | 1.000 | 0.340 |
| Standard LoRA | 0.867 | 0.728 | 0.000 |
| Hard-freeze CML | 0.858 | 0.731 | 0.000 |
| **CCR-LoRA (λ=1.0)** | **0.960** | **0.992** | **0.002** |

---

## Domains & Datasets

| Dataset | Domain | Confounder | Causal Method | ATE | Placebo Ratio |
|---------|--------|-----------|--------------|-----|--------------|
| [CWRU](https://engineering.case.edu/bearingdatacenter) | Rotating machinery | Motor load (HP) | Backdoor OLS | −0.076 | 115× |
| [NASA CMAPSS FD001](https://www.nasa.gov/content/prognostics-center-of-excellence-data-set-repository) | Turbofan degradation | Operating conditions | Backdoor OLS | 0.204 | 101× |
| [MIT-BIH Arrhythmia](https://physionet.org/content/mitdb/) | Cardiac ECG (3-class) | RR interval | Backdoor OLS | 0.027 | 4.4× |
| [MFPT](https://www.mfpt.org/fault-data-sets/) | Bearing (harder benchmark) | Shaft RPM | Backdoor OLS | — | — |

All datasets are downloaded automatically at runtime. No raw data is stored in this repository.

---

## Key Results

- **CWRU cross-load F1: 0.9994** — including OR_021 (class 9), which IRM fails completely (recall=0.00)
- **CCR-LoRA ATE drift: 0.002** — 68× lower than EWC while maintaining competitive accuracy on both old and new tasks
- **Causal consistency across 3 domains** — backdoor identification significant (p<0.001) in all mechanical and biomedical domains
- **Proposition A** — bidirectional consensus improves reliability score (0.839 vs 0.827 forward-only, Spearman ρ=0.115, p<0.001)

---





## Causal Identification

Each domain uses backdoor adjustment with a domain-appropriate confounder. The identifying assumption — no unmeasured common cause between the treatment (feature norm) and outcome (fault) other than the named confounder — is validated empirically via placebo permutation test (1000 iterations). Results are reported with 95% bootstrap confidence intervals.

Causal analysis is run on training data only. Test data is never used to estimate ATEs.

---

## Continual Learning Protocol

Base model trained on CWRU classes 0–6 (Normal, Ball×3, IR×3) across loads 0–2. New task: OR faults (classes 7–9). N=100 samples per new class. EMA reference buffer computed on 500 base-class samples before adaptation begins.

CCR-LoRA freezes early convolutional layers and trains a LoRA adapter on the feature layer, with a causal consistency penalty that penalizes drift in feature norm distributions relative to the pre-adaptation reference.

---

## Prototype Notebook

This repo is entirely for the initial prototypes and archive notebooks. Some more unreleased notebooks will be uploaded soon.

---



## Citation

If you use this work, please cite:

```bibtex
@misc{cnsd2025,
  title={CNSD: Causal Neuro-Symbolic Diagnosis},
  author={[Author]},
  year={2026},
  note={Preprint}
}
```

---

## License

Apache 2.0
