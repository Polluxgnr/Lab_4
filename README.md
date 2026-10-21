# Lab 4: An LLM Extraction You Can Trust

- **Group Members:**
  - Pollux Gronier — Student ID: `B00822392`
  - Saty Viard Laroque — Student ID: `B00822312`
  - Fabrice Afopa — Student ID: `B00817356`
  - Eliott Beghin — Student ID: `B00824999`

---

## Overview

This repository contains the completed, fully executed notebook for **Lab 4: An LLM Extraction You Can Trust** (AI for Data Analysis and Management / Climate Finance).

The lab treats the LLM as a **measurement instrument** with measurable uncertainty, evaluating:
- **Part A:** Schema validation (`pydantic`), JSON parsing with regex recovery, and manual annotation nuances.
- **Part B:** Field-level accuracy against gold standard, Bayesian and Bootstrap confidence intervals, and downstream portfolio-level impacts (net error vs gross absolute error vs median intensity).
- **Part C:** Self-consistency voting ($T=0.7$, 5 samples), agreement as an uncertainty signal, systematic errors (confidently wrong blindspots), model calibration, and human review policies ($P0, P1, P2$) with economic trade-offs.
- **Part D:** Information retrieval (TF-IDF vs Dense Embeddings using SentenceTransformers), lexical mismatch, and retrieval bottleneck risks.
- **Part E:** Executive decision memo (~80 words) to the Head of ESG Data using calibrated IPCC uncertainty language.

---

## How to Run

### 1. Requirements

- Python 3.10+ (tested on Python 3.12)
- Dependencies:
  ```bash
  pip install numpy pandas scipy matplotlib scikit-learn pydantic sentence-transformers jupyter
  ```
  *Note on D2:* Box D2 downloads the embedding model `all-MiniLM-L6-v2` from Hugging Face on the first run. If offline or if the download is unavailable, the cell handles the exception gracefully, prints a note, and the rest of the notebook executes without interruption.

### 2. Data Files

The notebook expects the following JSON data files in either `./data/` or the root directory:
- `excerpts.json`
- `gold_labels.json`
- `recorded_responses.json`
- `aurelia_report_chunks.json`

### 3. Execution

Open Jupyter or VS Code:
```bash
jupyter notebook lab4.ipynb
```
To re-run all cells from the command line:
```bash
jupyter nbconvert --to notebook --execute --inplace lab4.ipynb
```

---

## Key Results Summary

| Metric / Experiment | Value |
|---|---|
| **Greedy Field Accuracy** | **86.7%** (104 / 120 fields) |
| **Greedy without E11 syntax crash** | **91.2%** (104 / 114 fields) |
| **Worst Greedy Field** | `scope1_tco2` (**75.0%**) |
| **Beta-Binomial 90% Credible Interval** | **[80.6%, 90.8%]** |
| **Bootstrap 90% Interval (over excerpts)** | **[77.5%, 93.3%]** |
| **P(greedy accuracy > 95%)** | **0.01%** |
| **Downstream Scope 1 Total (LLM vs Gold)** | **252.112 MtCO2e** vs **242.062 MtCO2e** |
| **Downstream Net Scope 1 Error** | **+10.050 MtCO2e** (+4.2%) |
| **Downstream Absolute Scope 1 Error** | **92.234 MtCO2e** (38.1% of true portfolio total) |
| **Median Intensity (LLM vs Gold)** | **1.895** vs **1.865** tCO2/t steel (1.845 vs 1.878 on 16 common) |
| **Majority Vote Accuracy (5 samples)** | **95.0%** (114 / 120 fields) |
| **Accuracy at 5/5 Agreement** | **97.8%** (91 / 93 fields; error rate 2.2%) |
| **Accuracy at 4/5 Agreement** | **93.3%** (14 / 15 fields; error rate 6.7%) |
| **Accuracy at $\le$3/5 Agreement** | **75.0%** (9 / 12 fields; error rate 25.0%) |
| **Cheapest Baseline Review Policy** | **Policy P1** (EUR 408 total cost: EUR 108 review + EUR 300 residual error cost) |
| **Extension Policy P3 (P1 + unit trigger)** | **EUR 148** (37 fields reviewed, 0 residual errors; in-sample optimistic) |
| **TF-IDF Retrieval hit@1 / hit@3** | **66.7%** (4 / 6 queries; Q1 and Q6 fail) |
| **Dense Embeddings hit@1 / hit@3** | **83.3%** (5 / 6 queries; only Q1 fails) |
