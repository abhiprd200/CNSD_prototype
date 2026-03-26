# Causal-Neuro-Symbolic Diagnosis (CNSD)
CNSD is a five-layer bidirectional diagnostic framework for industrial fault detection. It goes beyond standard machine learning by not just predicting what fault occurred, but also explaining why, quantifying how much, reasoning about what if, and doing all of this with feedback flowing both forward and backward through the pipeline.

---

## Overview

**CNSD** is an independent research prototype that combines three powerful AI paradigms into a unified fault detection framework:

- 🧠 **Layer 1 — 1D CNN + S-JEPA** — A three-block convolutional neural network processes raw vibration signals directly — no manual feature engineering. Alongside it, a Signal JEPA encoder inspired by Yann LeCun's Joint Embedding Predictive Architecture learns physical state representations in a fully self-supervised manner with zero fault labels. Together they achieve 100% weighted F1 across 10 fault classes on CWRU, and 99.52% F1 via linear probe on JEPA embeddings alone.
- 🔣 **Layer 2 — Symbolic Rule Engine** — Maps every predicted fault class to a human-readable diagnosis: root cause, severity level (NONE/LOW/MEDIUM/HIGH), and a recommended maintenance action with a concrete timeframe. This transforms a raw class label into something an engineer can act on without any AI knowledge
- 📐 **Layer 3 — Causal Inference via Pearl's SCM (Rung 2)** — Using the backdoor adjustment criterion from Judea Pearl's Structural Causal Models, CNSD estimates the Average Treatment Effect of vibration energy on fault probability. ATE = 0.3409, validated by a placebo test with a ratio of 29.05× — meaning the real causal signal is 29 times stronger than random noise. Validated also on NASA CMAPSS turbofan data (ATE=0.0546, placebo=25.92×), proving domain-agnostic generalisation.
Most existing fault detection systems tell you **what** went wrong. CNSD tells you **why** it went wrong — and **what would have prevented it**.
- 📐 **Layer 3B — Counterfactual Analysis (Rung 3)** — Answers: "What would the fault risk have been if we had intervened earlier?" Three scenarios are computed — 25%, 50%, and 80% vibration reduction — quantifying exactly how much risk each intervention removes. This is Pearl's Rung 3. No prior industrial fault diagnosis system has demonstrated all three rungs simultaneously.
- 📐 **Layer 4 — Bidirectional Consensus** — All layers feed into a composite scoring mechanism. The feedback runs both ways — causal ATE adjusts CNN confidence thresholds dynamically, symbolic conflicts raise suspicion flags, and counterfactual risk escalates the final consensus. No layer operates in isolation.

---

## Architecture

```
Raw Sensor Data (CWRU Bearing / NASA CMAPSS)
          |
          v
[ Layer 1: 1D CNN + S-JEPA Encoder  ]  -->  F1 = 1.00  |  10 classes  |  JEPA probe = 99.52%
          |            ^  PATH B: Causal suspicion lowers CNN threshold dynamically
          v
[ Layer 2: Symbolic Rule Engine     ]  -->  Root cause  |  Severity  |  Maintenance action
          |            ^  PATH B: Symbolic conflict raises causal suspicion flag
          v
[ Layer 3: Causal Inference (SCM)   ]  -->  ATE = 0.3409  |  Placebo = 29.05x  |  Rung 2
          |            ^  PATH B: ATE dynamically adjusts CNN confidence threshold
          v
[ Layer 3B: Counterfactual          ]  -->  3 intervention scenarios  |  Pearl Rung 3
          |            ^  PATH B: CF risk multiplier escalates consensus score
          v
[ Layer 4: Bidirectional Consensus  ]  -->  Composite score  |  Forward + Backward feedback

```

---

## Motivation

Current neuro-symbolic systems for diagnosis lack **causal grounding**. They detect and classify, but cannot answer interventional questions and lack in the features like: explaining why, quantifying how much, reasoning about what if, and doing all of this with feedback flowing both forward and backward through the pipeline.


Pearl's do-calculus provides the formal framework to answer these questions. CNSD embeds this framework directly into the neuro-symbolic pipeline.

---

## Some valuable stats

| Metric | Value |
|---|---|
| RF baseline F1 (CWRU) | 94% |
| 1D CNN F1 (CWRU) | 100% |
| S-JEPA linear probe F1 | 99.52% (zero labels) |
| Causal ATE (CWRU CNN) | 0.3409 |
| Placebo ratio (CWRU) | 29.05x |
| Causal ATE (CMAPSS) | 0.0546 |
| Placebo ratio (CMAPSS) | 25.92x |
| RUL prediction RMSE | 41.35 cycles |
| Pearl Rungs | All 3 |
| Bidirectional paths | 2 |
| Labels used by JEPA | Zero |
| Datasets validated | 2 |


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

Currently prototyping on the **CWRU Bearing Fault Dataset** (Case Western Reserve University) — a standard benchmark for mechanical fault detection in rotating machinery and **NASA CMAPSS (Commercial Modular Aero-Propulsion System Simulation)** - Industrial benchmarks for fault detection of turbofan jet engines.

---


## Related Work

This work sits at the intersection of three research areas that have largely been studied separately:

- **Causal ML**: Pearl (2000), DoWhy (Sharma & Kiciman, 2020), DECI (Microsoft, 2021)
- **Neuro-Symbolic AI**: NS-CL (Mao et al., 2019), DeepProbLog, IBM NeSy systems
- **ML-based Fault Detection**: CWRU benchmarks, NASA CMAPSS, predictive maintenance literature

**Gap**: No unified framework applies Pearl's do-calculus *within* a neuro-symbolic pipeline specifically for fault diagnosis and applies JEPA to learn from it. CNSD addresses this gap.

---

## Author

**Abhimanyu Prasad** 
**abhiprd20@gmail.com**
High School Researcher | Independent AI Research
Research interest: Causal AI, Neuro-Symbolic Systems, Explainable Fault Detection, NLP and low - resource languages

*Research conducted independently.*

---

## Citation

Please cite:

```
  Author    = {Abhimanyu Prasad},
  Title     = {Causal Neuro-Symbolic Diagnosis},
  Year      = {2026}
  
---

## License

Apache 2.0 License — open for collaboration and academic use.

---

> *Built by a high schooler who believes AI should not just predict, but understand and learn from the committed mistakes.*
