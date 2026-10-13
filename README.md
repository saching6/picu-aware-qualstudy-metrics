# PICU-Aware qualitative study: LLM coding prompt and agreement metrics

Supporting materials for:

> Grover S, Mehta S, Sannino J, Keleman A, [...], Tsui F. *Supporting Shared Situation Awareness With Machine-Learning Risk Prediction in the Pediatric Intensive Care Unit: A Qualitative Study of PICU-Aware.* [Journal, year, DOI — to be added on publication]

The study evaluated PICU-Aware, a clinician-facing interface for the PICU Warning Index (P-WIN), a machine-learning model that predicts critical deterioration in the pediatric intensive care unit. Transcripts from 11 interviews and 6 focus groups were analyzed with a codebook thematic analysis. A large language model (LLM) pre-coded the 15 non-seed transcripts for human review. Its codes were validated against a blinded human reference standard on a 225-excerpt holdout.

This repository contains the prompt used for LLM pre-coding and the code that reproduces the validation results (Supplementary Table S5).

## Contents

| File | Description |
|---|---|
| `LLM_coding_prompt.md` | The complete prompt submitted to Microsoft Copilot Chat (GPT-5.6) on July 28, 2026: role, task, coding rules, 5 few-shot examples from the human-coded seed transcripts, and the JSON output schema (Supplementary File S1). The codebook supplied with the prompt is reproduced in Supplementary Table S1 of the paper. |
| `table_s5.ipynb` | Jupyter notebook that computes interrater reliability between 2 human coders and model agreement with the reference standard, by domain and pooled (Supplementary Table S5). |

## What the notebook computes

For each domain (A, situation awareness and team coordination, 8 codes; B, usability and workflow integration, 10 codes; C, explainability and trust, 11 codes) and pooled across domains:

- **Human coder 1 vs coder 2** (independent, pre-adjudication labels): observed agreement, Cohen's κ, and Gwet's AC1.
- **Model vs reference standard** (medium- and high-confidence model codes): precision, recall, F1-score, κ, and AC1, plus the error rates 1 − precision (model codes absent from the reference standard) and 1 − recall (reference codes the model missed).

Method details:

- Agreement is scored at the excerpt-by-code (present/absent) level, with each excerpt scored against the codes of each domain.
- Codes are assigned to their codebook domain regardless of the column in which a coder entered them.
- The administrative participant-background code is excluded.
- Blank reference cells are treated as true negatives.
- The pooled row is the micro-average of per-domain cells.
- 95% CIs are percentile bootstrap intervals: 3000 resamples over excerpts, with a fixed seed so results are reproducible.

The human and model values are not directly comparable. The reference standard was built from the 2 coders' reconciled labels, so the model is compared with a consensus while the coders are compared with each other.

## Running the notebook

1. Install Python 3.10 or later and the dependencies:

   ```bash
   pip install pandas openpyxl jupyter
   ```

2. Place the 5 input files in a folder named `data/` next to the notebook (or set `DATA_DIR` in the first code cell):

   | File | Contents |
   |---|---|
   | `coder1_codes.json`, `coder2_codes.json` | One record per excerpt: `excerpt_id` and one string field per domain listing the codes applied (comma- or semicolon-separated) |
   | `consensus_codes_holdout.json` | One record per excerpt: `excerpt_id`, `domainA_Consensus`, `domainB_Consensus`, `domainC_Consensus` |
   | `model_codes_holdout.json` | LLM output in the schema defined in `LLM_coding_prompt.md` (`domainA_codes`, `domainB_codes`, `domainC_codes`, each a list of `{code, confidence, ...}`) |
   | `Final_Codebook.xlsx` | Codebook with a `Codes` sheet mapping each code to its domain |

3. Run all cells. The notebook writes `table_s5.csv` and `table_s5_error_rates.csv`. It also checks that all 4 coding files cover the same excerpts and lists any unrecognized code labels.

Data availability: the input files are not included in this repository. Transcripts contain verbatim clinical discussion and are not publicly available; see the Data Availability Statement in the paper.

## Citation

If you use the prompt or code, please cite the paper above.

## License

[Code: MIT License. Prompt text: CC BY 4.0. Add a `LICENSE` file to match.]

## Contact

[Sachin Grover](mailto:grovers1@chop.edu), Department of Biomedical and Health Informatics, Children's Hospital of Philadelphia
