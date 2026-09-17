# Embedding Models Measure in Peculiar Ways

![License: AGPLv3+](https://img.shields.io/badge/License-AGPLv3+-brightgreen.svg)

This repository contains the code for **Embedding Models Measure in Peculiar Ways**, a study of whether text embedding models reflect physical measurements of length, mass, volume and time. For example, we check whether "1 meter" is embedded closer to "105 centimeters" than to "15 kilometers". Across 24 embedding models, we find that embedding similarity follows physical relationships only weakly, and that it is more strongly linked to how numbers look as strings than to their numerical values.

Preprint: [LINK TO BE ADDED](https://TODO)

All experiments are in Jupyter notebooks. The main notebook writes its results to `produced_results/`, and its table section builds the LaTeX tables from those files.

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Setup](#setup)
- [Running the Experiments](#running-the-experiments)
- [Citation](#citation)
- [About Impresso](#about-impresso)
- [License](#license)

## Overview

For each embedding model, we embed short measurement phrases such as `"5 meters"` and compare them with cosine similarity. We then check whether the similarities follow the true physical distances. The analysis covers:

- **Measurement geometry**: similarity heatmaps for four quantities (meter, kilogram, liter, second) over five number ranges (local 0–10, medium 0–1,000, log 1–100,000, sign −100–100, and scientific notation), plus digits vs. number words (e.g. *5* vs. *five*).
- **Unit conversion**: similarity between equivalent expressions in different units (e.g. *1 meter* vs. *100 centimeters*).
- **Benchmark**: Kendall's τ and pairwise accuracy (PACC) for ranking all models on a single axis.
- **Recalibration**: a linear probe on the embedding difference, to test whether cosine similarity is simply miscalibrated.
- **Lexical vs. numerical similarity**: correlation of embedding similarity with character-level Levenshtein, tokenizer-level Levenshtein, and true numerical distance.

## Repository Structure

```
.
├── measurements_full_experimentation_output.ipynb   # all experiments, figures and tables (with outputs)
├── measurements_plot_front_page_figure.ipynb        # Figure 1 (ideal vs. model similarity, -15 to 15 meters)
├── requirements.txt
└── produced_results/                                # written by the main notebook
    ├── csv/                                         # per-model results
    ├── pdf/                                         # per-model heatmaps and conversion plots
    └── tex/                                         # LaTeX tables
```

The `produced_results/` folder is created automatically when the main notebook runs.

## Setup

Requires **Python 3.11**. A GPU is strongly recommended; we ran the notebooks on Colab with an A100. The larger models (e.g. `Qwen3-Embedding-8B`) need a GPU with enough memory.

```bash
pip install -r requirements.txt
```

The notebooks download models from the Hugging Face Hub and may prompt for a token via `login()`. Some models, such as `google/embeddinggemma-300m`, require accepting their license on the Hub first. Several models are loaded with `trust_remote_code=True`.

## Running the Experiments

### Main notebook (`measurements_full_experimentation_output.ipynb`)

The notebook is split into three parts ("Exams") and a table section. Run the definitions cell of Exam 1 first, since the other parts reuse its settings and output folders. Each part runs the models in batches (baseline models, then extended sets PT1–PT3). To test another model, add its Hugging Face name to one of the model lists.

| Notebook part | What it does | Paper |
| --- | --- | --- |
| Exam 1: Measurement geometry | Heatmaps, conversion curves, Kendall's τ and PACC per model | Sections 4–6, Figures 2–47, Tables 1–2 |
| Exam 2: Lexical vs. numerical similarity | Correlation with Levenshtein and numerical distance | Section 7.2, Table 4 |
| Exam 3: Is similarity just miscalibrated? | Ridge regression probe vs. cosine similarity, over 5 random item-level splits | Section 7.1, Table 3 |
| Tables | Builds the LaTeX tables from the saved CSV files | Tables 1–4 |

- **Output:** per-model CSV files in `produced_results/csv/`, plots in `produced_results/pdf/`, and LaTeX tables in `produced_results/tex/`.

This is the slow, GPU-heavy part.

### Front page figure (`measurements_plot_front_page_figure.ipynb`)

Produces Figure 1 of the paper for `all-mpnet-base-v2` and `Qwen3-Embedding-0.6B`.

- **Output:** `figure1.pdf` and `figure1.png` in the working directory.

## Citation

If you use this code, please cite:

```UPDATE
```

## Acknowledgements

This work has been supported by the Swiss National Science Foundation (grant no.\ CRSII5\_213585) and by the Luxembourg National Research Fund (grant no.\ 17498891).

## License

Released under the [GNU Affero General Public License v3 or later](https://github.com/impresso/impresso-pyindexation/blob/master/LICENSE).

---

