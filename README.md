# Residue-Level Attributions in Protein Language Models Do Not Recover Allergen Epitopes

<p align="center">
  <img src="docs/assets/eth-logo-pos.png" alt="ETH Zurich" height="40" />
  &nbsp;&nbsp;
  <img src="docs/assets/SIAFlogo.svg" alt="Swiss Institute of Allergy and Asthma Research (SIAF)" height="40" />
  &nbsp;&nbsp;
  <img src="docs/assets/logo.svg" alt="Swiss Institute of Bioinformatics (SIB)" height="40" />
  &nbsp;&nbsp;
  <img src="docs/assets/ICML-logo.svg" alt="ICML 2026" height="40" />
</p>

<p align="center">
  <!-- Workshop -->
  <a href="https://mechinterpworkshop.com/"><img alt="ICML 2026" src="https://img.shields.io/badge/ICML%202026-Mechanistic%20Interpretability%20Workshop-blue.svg" /></a>
  <!-- Code license -->
  <a href="LICENSE"><img alt="Code: MIT" src="https://img.shields.io/badge/code-MIT-green.svg" /></a>
  <!-- Docs/figures license -->
  <a href="LICENSE-CC-BY-4.0.md"><img alt="Docs/Figures: CC BY 4.0" src="https://img.shields.io/badge/docs%20%2F%20figures-CC%20BY%204.0-lightgrey.svg" /></a>
  <!-- Python -->
  <a href="https://www.python.org/downloads/release/python-3135/"><img alt="Python 3.13.5" src="https://img.shields.io/badge/python-3.13.5-blue.svg" /></a>
  <!-- PyTorch -->
  <a href="https://pytorch.org/"><img alt="PyTorch 2.10" src="https://img.shields.io/badge/PyTorch-2.10.0-ee4c2c.svg?logo=pytorch&logoColor=white" /></a>
</p>

---

## Overview

Protein language models achieve strong allergenicity classification, but it remains unclear whether their residue-level explanations reflect immunologically meaningful mechanisms. This repository provides the code, data, checkpoints, and precomputed results for an epitope-grounded benchmark evaluating whether attribution scores align with experimentally annotated allergen epitopes.

We evaluate three main classifier families — frozen ESM-2, multi-task ESM-2, and retrained DeepPlantAllergy — with an additional exploratory top-layer-unfrozen MTL checkpoint included in supplementary analyses. Attribution alignment is measured using AUROC, AUPRC, and Precision@k against experimentally annotated allergy-associated epitopes from IEDB, including B-cell and MHC-II-associated T-cell epitope annotations after curation. We further test model faithfulness with masking experiments and characterize model sensitivity through in silico saturation mutagenesis.

---

## Key Findings

- Strong protein-level allergenicity classification does not imply residue-level alignment with experimentally annotated epitopes.
- Integrated Gradients can be demonstrably model-faithful under masking experiments while still failing to localize annotated epitopes.
- Multi-task epitope supervision allowed the auxiliary residue head to learn epitope-associated signals, but this did not translate into epitope-aligned classification-head attributions.
- Attribution methods tested are insufficient as a proxy for immunologically meaningful mechanisms in allergen protein language models.

---

## Authors and Affiliations

**Jianzhou Yao**¹³ · **Anxiong Song**¹ · **Katja Baerenfaller**¹² · **Damir Zhakparov**¹²

¹ Swiss Institute of Allergy and Asthma Research (SIAF), Davos, Switzerland
² Swiss Institute of Bioinformatics (SIB), Lausanne, Switzerland
³ ETH Zurich, Zurich, Switzerland

---

## Quick Start

**Requirements:** [`uv`](https://docs.astral.sh/uv/) and Python 3.13.5.

```bash
# 1. Clone the repository
git clone https://github.com/Jeffateth/XAllergen2.0-paper.git
cd XAllergen2.0-paper

# 2. Create the environment and install locked dependencies
make setup

# 3. Register the Jupyter kernel (for notebook execution)
make kernel

# 4. Verify the environment
make doctor
```

To reproduce all paper figures directly from included precomputed results
(no retraining required):

```bash
python replot_probe_figures.py
# or open notebooks/08_compare_all_model_probes.ipynb
```

---

## Repository Structure

```
XAllergen2.0-paper/
├── README.md                          This file
├── LICENSE                            MIT License (code)
├── LICENSE-CC-BY-4.0.md               CC BY 4.0 (docs, figures)
├── CITATION.cff                       Machine-readable citation
├── DATA_SOURCES.md                    Data provenance and licenses
├── pyproject.toml                     Python dependencies (pinned)
├── uv.lock                            Locked dependency graph
├── Makefile                           Environment setup shortcuts
├── .python-version                    Python version pin (3.13.5)
├── replot_probe_figures.py            Regenerate figures from saved CSVs
│
├── data/
│   ├── t_cell_b_cell_MHC_II_epitopes.csv       IEDB epitope export
│   ├── positives.csv / positives_splitA/B.csv  Allergen protein splits
│   ├── negatives.csv / negatives_splitA/B.csv  Non-allergen splits
│   ├── deepalgpro_*.csv / *.fasta              DeepAlgPro benchmark data
│   ├── iedb_sequence_fetch_cache.csv           Cached sequence fetches
│   └── ...                                     See DATA_SOURCES.md
│
├── models/
│   ├── README.md                      Checkpoint documentation
│   ├── baseline_frozen_esm2.pt        Frozen ESM-2 baseline
│   ├── mtl_frozen_esm2_epitope.pt     MTL frozen backbone
│   ├── mtl_top1_unfrozen_esm2_epitope.pt  MTL fine-tuned backbone
│   └── deep_plant_allergy_benchmark.pt    DeepPlantAllergy retrained
│
├── notebooks/
│   ├── 01_curate_allergenicity_data.ipynb     Data curation pipeline
│   ├── 02_data_exploration_deepalgpro.ipynb   Dataset EDA
│   ├── 03_baseline_model_esm2.ipynb           Baseline training (Colab/local)
│   ├── 03_deep_plant_allergy_benchmark.ipynb  DeepPlantAllergy benchmark
│   ├── 04_mtl_epitope_supervision.ipynb       MTL frozen training
│   ├── 05_mtl_top1_unfrozen_epitope_supervision.ipynb  MTL fine-tuned training
│   ├── 06_generate_probe_rows.ipynb           Residue attribution analysis
│   ├── 07_insilico_mutagenesis.ipynb          Saturation mutagenesis
│   ├── 08_compare_all_model_probes.ipynb      Paper figures and tables
│   └── 09_unfiltered_ig_masking_sensitivity.ipynb  Supplementary analysis
│
├── results/
│   ├── classification/                Protein-level classification metrics
│   ├── probing/
│   │   ├── rows/                     Per-protein residue attribution rows
│   │   └── summaries/                Bootstrap summaries and comparisons
│   ├── insilico_mutagenesis/         Mutagenesis tables and figures
│   ├── paper_figures/                Final paper figures (PDF + PNG)
│   └── paper_tables/                 Final paper tables (CSV + LaTeX)
│
└── src/xallergen/
    ├── baseline_notebook_utils.py    ESM-2 model, training, IG utilities
    ├── mtl_epitope_notebook_utils.py MTL model and probing utilities
    ├── deep_plant_allergy_utils.py   DeepPlantAllergy architecture
    ├── plotting_paper_figures.py     Figure rendering
    └── plotting_insilico_mutagenesis.py  Mutagenesis plots
```

---

## Reproducibility

### What can be reproduced directly

The following results can be reproduced **without GPU retraining**, using the
included model checkpoints and precomputed probe rows:

| Result | Source |
|--------|--------|
| All paper figures (PDF + PNG) | `results/paper_figures/` or notebook 08 |
| All paper tables (CSV + LaTeX) | `results/paper_tables/` or notebook 08 |
| Mutagenesis figures | `results/insilico_mutagenesis/` or notebooks 07–08 |
| Residue attribution scores | `results/probing/rows/` or notebook 06 |

### What requires GPU retraining

| Step | Notebook | GPU required | Runtime |
|------|----------|-------------|---------|
| Baseline model training | 03 | Yes (T4/A100, Google Colab) | ~30 min |
| MTL frozen training | 04 | Yes (T4/A100, Google Colab) | ~60 min |
| MTL unfrozen training | 05 | Yes (T4/A100, Google Colab) | ~90 min |
| DeepPlantAllergy benchmark | 03_deep_plant_allergy_benchmark.ipynb | Yes (T4/A100, Google Colab) | ~30 min |

---

## Data Sources

See [DATA_SOURCES.md](DATA_SOURCES.md) for full documentation of:
- IEDB epitope annotations (CC BY 4.0)
- UniProt negatives (CC BY 4.0)
- DeepAlgPro benchmark FASTA files
- ESM-2 base model (MIT License, auto-downloaded from Hugging Face)

---

## Running the Pipeline

### Environment

```bash
make setup    # creates .venv, installs locked dependencies via uv
make kernel   # registers Jupyter kernel "xallergen2"
make doctor   # verifies all imports
make clean    # removes .venv (safe to re-run make setup after)
```

### Notebook execution order

Run notebooks in the following order to reproduce the full pipeline from scratch:

```
01 → 02 → 03 → 03_deep_plant_allergy_benchmark → 04 → 05 → 06 → 07 → 08 → 09
```

**Notes:**
- Notebooks 03, 04, 05 support both local execution (`RUN_TARGET = "local"`)
  and Google Colab (`RUN_TARGET = "colab"`). Set the `RUN_TARGET` variable in
  the first cell before running.
- Notebooks 06–09 are designed for local execution.
- Notebook 01 requires [MMseqs2](https://github.com/soedinglab/MMseqs2) to be
  installed and available on `PATH` for sequence clustering. All downstream files
  are already included; nb01 only needs to be re-run if regenerating the dataset
  from scratch.
- Notebook 01 uses the NCBI Entrez API to fetch protein sequences. Set
  `ENTREZ_EMAIL` to a valid email address before running.

### Regenerate figures without retraining

```bash
# From precomputed probe rows and mutagenesis CSVs:
python replot_probe_figures.py

# Or open notebook 08 and run all cells (local, ~5 min).
```

### Random seed

All stochastic steps use `RANDOM_STATE = 42` (notebooks 03–07) and
`RANDOM_STATE = 13` (notebook 01, data splitting). These are set in each
notebook's configuration cell.

---

## Model Checkpoints

See [models/README.md](models/README.md) for detailed checkpoint documentation,
including which notebook regenerates each checkpoint and which figures depend on it.

The ESM-2 base model (`facebook/esm2_t6_8M_UR50D`) is downloaded automatically
from Hugging Face Hub on first use.

---

## Citation

If you use this code or data, please cite:

```bibtex
@inproceedings{yao2026residue,
  title     = {Residue-Level Attributions in Protein Language Models Do Not Recover Allergen Epitopes},
  author    = {Yao, Jianzhou and Song, Anxiong and Baerenfaller, Katja and Zhakparov, Damir},
  booktitle = {ICML 2026 Workshop on Mechanistic Interpretability},
  year      = {2026},
  note      = {Accepted at the ICML 2026 Workshop on Mechanistic Interpretability},
}
```

Please also cite the underlying resources:
- **IEDB:** Vita R et al., Nucleic Acids Research 47(D1):D339–D343, 2019
- **ESM-2:** Lin Z et al., Science 379(6637):1123–1130, 2023
- **MMseqs2:** Steinegger M & Söding J, Nature Biotechnology 35:1026–1028, 2017

---

## License

Code in this repository is released under the **MIT License** (see [LICENSE](LICENSE)).

Paper text, documentation, and generated figures are released under
**Creative Commons Attribution 4.0 International (CC BY 4.0)**
(see [LICENSE-CC-BY-4.0.md](LICENSE-CC-BY-4.0.md)), unless otherwise stated.

Datasets are redistributed or referenced according to their original source licenses
and terms; see [DATA_SOURCES.md](DATA_SOURCES.md).

---

## Notes

- `uv.lock` is the authoritative reproducibility artifact for the Python environment.
- Figures are saved in both PDF and PNG. The `results/` directory contains precomputed outputs; running the full pipeline will overwrite them.
