# Residue-Level Attributions in Protein Language Models Do Not Recover Allergen Epitopes

<p align="center">
  <img src="docs/assets/SIAFlogo.png" alt="Swiss Institute of Allergy and Asthma Research" width="135" align="middle"/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="docs/assets/logo.svg" alt="Swiss Institute of Bioinformatics" width="165" align="middle"/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="docs/assets/eth-logo-pos.png" alt="ETH Zurich" width="230" align="middle"/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="docs/assets/ICML-logo.svg" alt="ICML" width="175" align="middle"/>
</p>

<p align="center">
  <a href="https://mechinterpworkshop.com/"><img alt="ICML 2026 MechInterp Workshop" src="https://img.shields.io/badge/Workshop-MechInterp%40ICML%202026-6aa84f?style=for-the-badge" /></a>
  <a href="https://doi.org/10.48550/arXiv.2606.22181"><img alt="arXiv" src="https://img.shields.io/badge/arXiv-2606.22181-b31b1b.svg?style=for-the-badge" /></a>
  <a href="https://openreview.net/forum?id=SQus9iV9sf"><img alt="OpenReview" src="https://img.shields.io/badge/OpenReview-forum-blue?style=for-the-badge" /></a>
  <a href="docs/assets/XAllergen_Poster_ICML.pdf"><img alt="Poster" src="https://img.shields.io/badge/Poster-PDF-orange?style=for-the-badge" /></a>
</p>

---

Official code release for the ICML 2026 Mechanistic Interpretability Workshop paper:

**Residue-Level Attributions in Protein Language Models Do Not Recover Allergen Epitopes**

**Authors:** Jianzhou Yao, Anxiong Song, Katja Baerenfaller, Damir Zhakparov  
**Affiliations:** Swiss Institute of Allergy and Asthma Research; Swiss Institute of Bioinformatics; ETH Zurich

---

## Overview

Deep allergenicity classifiers are increasingly used in safety screening of novel foods, and recent protein language models have substantially improved protein-level allergenicity prediction. However, whether their explanations capture biologically meaningful information remains unclear.

This repository provides the code, data, checkpoints, and precomputed results for an epitope-grounded residue-level benchmark for quantitatively evaluating attribution faithfulness in protein allergenicity models. Across frozen ESM-2, multi-task ESM-2, and DeepPlantAllergy, protein-level classification was robust, yet classification-head explanation signals did not significantly exceed random in residue-level alignment with annotated epitopes across AUROC, AUPRC, and Precision@k.

Integrated Gradients identified residues that were functionally important to the model, but did not overlap annotated epitopes. Saturation mutagenesis further suggested that classifiers may rely on physicochemical and compositional sequence features rather than epitope-specific mechanisms.

---

## Quick Start

**Requirements:** [`uv`](https://docs.astral.sh/uv/) and Python 3.13.5.

```bash
git clone https://github.com/Jeffateth/XAllergen2.0-paper.git
cd XAllergen2.0-paper

make setup
make kernel
make doctor
```

Reproduce paper figures from included precomputed results:

```bash
python replot_probe_figures.py
# or run notebooks/08_compare_all_model_probes.ipynb
```

---

## Repository Structure

```text
XAllergen2.0-paper/
├── data/                    Curated data, splits, epitope masks, benchmark files
├── models/                  Trained checkpoints
├── notebooks/               Reproducible analysis notebooks 01–09
├── results/                 Precomputed outputs, figures, and tables
├── src/xallergen/           Model, attribution, and plotting utilities
├── DATA_SOURCES.md
├── CITATION.cff
├── LICENSE
├── LICENSE-CC-BY-4.0.md
├── pyproject.toml
├── uv.lock
├── Makefile
└── replot_probe_figures.py
```

---

## Reproducibility

The included checkpoints and precomputed outputs allow figure regeneration without GPU retraining.

| Output | Source |
|--------|--------|
| Paper figures | `results/paper_figures/` or notebook 08 |
| Paper tables | `results/paper_tables/` or notebook 08 |
| Residue attribution rows | `results/probing/rows/` or notebook 06 |
| Mutagenesis results | `results/insilico_mutagenesis/` or notebooks 07–08 |

Training from scratch requires GPU execution for the model notebooks:

| Step | Notebook |
|------|----------|
| Frozen ESM-2 baseline | `03_baseline_model_esm2.ipynb` |
| DeepPlantAllergy benchmark | `03_deep_plant_allergy_benchmark.ipynb` |
| MTL ESM-2 | `04_mtl_epitope_supervision.ipynb` |
| Top-layer-unfrozen MTL ESM-2 | `05_mtl_top1_unfrozen_epitope_supervision.ipynb` |

Notebook execution order:

```text
01 → 02 → 03 → 03_deep_plant_allergy_benchmark → 04 → 05 → 06 → 07 → 08 → 09
```

Notes:
- Notebooks 03–05 support local and Google Colab execution via `RUN_TARGET`.
- Notebook 01 requires MMseqs2 on `PATH` if regenerating the dataset from scratch.
- Notebook 01 uses NCBI Entrez; set `ENTREZ_EMAIL` before running.
- Random states are set in the notebook configuration cells.

---

## Data and Checkpoints

Data provenance and redistribution notes are documented in [DATA_SOURCES.md](DATA_SOURCES.md).

Checkpoint documentation is provided in [models/README.md](models/README.md). The ESM-2 base model (`facebook/esm2_t6_8M_UR50D`) is downloaded automatically from Hugging Face Hub on first use.

---

## Citation

```bibtex
@inproceedings{yao2026residue,
  title     = {Residue-Level Attributions in Protein Language Models Do Not Recover Allergen Epitopes},
  author    = {Yao, Jianzhou and Song, Anxiong and Baerenfaller, Katja and Zhakparov, Damir},
  booktitle = {ICML 2026 Workshop on Mechanistic Interpretability},
  year      = {2026},
  note      = {Accepted at the ICML 2026 Workshop on Mechanistic Interpretability},
}
```

Please also cite the underlying resources used in the benchmark, including IEDB, ESM-2, MMseqs2, DeepAlgPro, DeepPlantAllergy, and UniProt where applicable.

---

## License

Code is released under the [MIT License](LICENSE).

Paper text, documentation, and generated figures are released under [CC BY 4.0](LICENSE-CC-BY-4.0.md), unless otherwise stated.

Datasets are redistributed or referenced according to their original source licenses and terms; see [DATA_SOURCES.md](DATA_SOURCES.md).

---

<p align="center" style="font-size: 0.85em; color: gray;">
Logos are used in accordance with the respective institutional and conference guidelines.
</p>
