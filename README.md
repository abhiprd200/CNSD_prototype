# CNSD_prototype
CNSD combines neural networks, symbolic reasoning, and Pearl's causal models into one fault detection pipeline — identifying not just what failed, but why, with counterfactual explanations.
# Causal-Neuro-Symbolic Diagnosis (CNSD)
### Integrating Pearl's Structural Causal Models into Neuro-Symbolic Fault Detection Pipelines

> *"Don't just detect the fault — understand why it happened."*

---

## Overview

**CNSD** is an independent research prototype that combines three powerful AI paradigms into a unified fault detection framework:

- 🧠 **Neural Networks** — learn patterns from raw sensor/signal data
- 🔣 **Symbolic Reasoning** — apply interpretable logic rules over learned representations
- 📐 **Pearl's Structural Causal Models (SCMs)** — perform causal attribution and counterfactual analysis

Most existing fault detection systems tell you **what** went wrong. CNSD tells you **why** it went wrong — and **what would have prevented it**.

---

## Architecture

```
Raw Sensor Data
      ↓
[ Neural Perception Layer ]   ← PyTorch autoencoder / classifier
      ↓
Symbolic Intermediate Variables  (e.g., vibration: HIGH, temp: ABNORMAL)
      ↓
[ Symbolic Reasoning Layer ]  ← Rule engine / pyDatalog
      ↓
Fault Label + Causal Attribution
      ↓
[ SCM Causal Layer ]          ← DoWhy / do-calculus
      ↓
Counterfactual Explanation    (e.g., "If temp had been normal → no fault")
```

---

## Motivation

Current neuro-symbolic systems for diagnosis lack **causal grounding**. They detect and classify, but cannot answer interventional questions like:

- *"What caused this fault?"*
- *"If we had changed X, would the fault have occurred?"*
- *"Which variable is the root cause vs a downstream symptom?"*

Pearl's do-calculus provides the formal framework to answer these questions. CNSD embeds this framework directly into the neuro-symbolic pipeline.

---

## Key Features

| Feature | Description |
|---|---|
| Causal Attribution | Identifies root cause, not just fault label |
| Counterfactual Reasoning | Answers "what-if" questions about system state |
| Symbolic Explainability | Human-readable logic rules at the reasoning layer |
| Modular Architecture | Each layer can be swapped independently |

---

## Tech Stack

| Tool | Role |
|---|---|
| `Python 3.10+` | Core language |
| `PyTorch` | Neural perception layer |
| `DoWhy` | Causal inference & do-calculus |
| `causalnex` | Causal DAG construction |
| `pyDatalog` | Symbolic reasoning layer |
| `pandas / numpy` | Data processing |

---

## Dataset

Currently prototyping on the **CWRU Bearing Fault Dataset** (Case Western Reserve University) — a standard benchmark for mechanical fault detection in rotating machinery.

---

## Project Status

> 🚧 **Active Prototype** — architecture designed, implementation in progress.

- [x] Problem formulation
- [x] Architecture design
- [x] Literature survey
- [ ] Neural layer implementation
- [ ] Symbolic layer implementation
- [ ] Causal layer (DoWhy integration)
- [ ] Experiments & evaluation
- [ ] Paper writeup (Overleaf)

---

## Related Work

This work sits at the intersection of three research areas that have largely been studied separately:

- **Causal ML**: Pearl (2000), DoWhy (Sharma & Kiciman, 2020), DECI (Microsoft, 2021)
- **Neuro-Symbolic AI**: NS-CL (Mao et al., 2019), DeepProbLog, IBM NeSy systems
- **ML-based Fault Detection**: CWRU benchmarks, NASA CMAPSS, predictive maintenance literature

**Gap**: No unified framework applies Pearl's do-calculus *within* a neuro-symbolic pipeline specifically for fault diagnosis. CNSD addresses this gap.

---

## Author

**Abhimanyu**
High School Researcher | Independent AI Research
Research interest: Causal AI, Neuro-Symbolic Systems, Explainable Fault Detection

*Research conducted independently. In correspondence with faculty at the Indian Institute of Science (IISc), Bangalore.*

---

## License

Apache 2.0 License — open for collaboration and academic use.

---

> *Built by a high schooler who believes AI should not just predict, but understand.*
