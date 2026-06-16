# Data Sources

This file documents the main data sources used in this repository and the processed files included for reproducibility.

---

## IEDB Epitope Annotations

**Source:** Immune Epitope Database (IEDB)  
**Included file:** `data/t_cell_b_cell_MHC_II_epitopes.csv`  
**Processing notebook:** `notebooks/01_curate_allergenicity_data.ipynb`

IEDB records were exported from the IEDB web interface and used to derive allergy-associated epitope annotations for the residue-level benchmark. Protein sequences were retrieved using accession identifiers from the IEDB export and cached in `data/iedb_sequence_fetch_cache.csv`.

Please cite IEDB when using these annotations.

---

## UniProt-derived Negative Proteins

**Source:** UniProtKB / Swiss-Prot  
**Raw export file:** `data/uniprotkb_NOT_taxonomy_id_10239_NOT_tax_2026_04_08.tsv`  
**Processing notebook:** `notebooks/01_curate_allergenicity_data.ipynb`

The UniProt-derived negative set was exported manually from the UniProt web interface using the filters described in the paper and curation notebook. The filename indicates an export date of 2026-04-08.

Because UniProt is updated over time, repeating the same search later may return different entries. The processed negative files are included to support reproducibility.

Please cite UniProt when using these data.

---

## DeepAlgPro-derived Benchmark Data

The released DeepAlgPro train/test FASTA files were used as the starting point for the protein-level allergenicity benchmark. Exact sequence overlaps with the epitope-grounded residue-level benchmark were removed before training and evaluation.

Included files:
- `data/deepalgpro_all.train.fasta`
- `data/deepalgpro_all.test.fasta`
- `data/deepalgpro_train_cleaned.csv`
- `data/deepalgpro_test_cleaned.csv`

Please cite the original DeepAlgPro paper when using these benchmark files.

---

## NCBI Sequence Retrieval

Some protein sequences were retrieved through NCBI Entrez during data curation and cached for reproducibility.

Cache file:
- `data/negative_sequence_fetch_cache.csv`

Please acknowledge or cite NCBI and the original sequence records where applicable.

---

## ESM-2 Base Model

**Model:** `facebook/esm2_t6_8M_UR50D`

The ESM-2 base model is not stored in this repository. It is downloaded automatically from Hugging Face Hub on first use.

Please cite the original ESM-2 paper when using this model.

---

## Main Processed Files

| File | Description |
|------|-------------|
| `data/positives.csv` | Curated allergen proteins with epitope intervals |
| `data/positives_splitA.csv` | Positive training split |
| `data/positives_splitB.csv` | Positive test split used for residue-level evaluation |
| `data/negatives.csv` | Curated non-allergen proteins |
| `data/negatives_splitA.csv` | Negative training split |
| `data/negatives_splitB.csv` | Negative test split |
| `data/deepalgpro_train_cleaned.csv` | Cleaned DeepAlgPro-derived training data |
| `data/deepalgpro_test_cleaned.csv` | Cleaned DeepAlgPro-derived test data |
| `data/iedb_sequence_fetch_cache.csv` | Cached sequence retrievals for IEDB-derived positives |
| `data/negative_sequence_fetch_cache.csv` | Cached sequence retrievals for negatives |

---

## Citation

If you use this repository, please cite:

```bibtex
@inproceedings{yao2026residue,
  title     = {Residue-Level Attributions in Protein Language Models Do Not Recover Allergen Epitopes},
  author    = {Yao, Jianzhou and Song, Anxiong and Baerenfaller, Katja and Zhakparov, Damir},
  booktitle = {ICML 2026 Workshop on Mechanistic Interpretability},
  year      = {2026},
  note      = {Accepted at the ICML 2026 Workshop on Mechanistic Interpretability},
}
