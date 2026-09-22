# OR-GSEA

**Curated family-level gene sets for human olfactory receptors**

OR-GSEA provides ready-to-use Gene Set Enrichment Analysis (GSEA) gene sets for human olfactory receptor (OR) families. OR genes are grouped into individual families according to the Human Olfactory Data Explorer (HORDE).

Three versions of the gene set library are provided with different levels of pseudogene filtering.

## Gene set files

Ready-to-use GMT files are available in [`GMT_files/`](./GMT_files).

| File | Description |
| --- | --- |
| `HUMAN_OR_families_withPseudo_v01.gmt` | All OR genes classified by HORDE |
| `HUMAN_OR_families_noPseudo_v01.gmt` | OR genes excluding pseudogenes identified by a terminal `P` in the gene symbol |
| `HUMAN_OR_families_FunctionalOnly_v01.gmt` | OR genes excluding pseudogenes and genes with HORDE pseudogene probability > 0.3 |

The three files represent alternative filtering levels. They do not need to be used sequentially.

### Filtering

**withPseudo**

Includes all OR genes classified by HORDE without filtering.

**noPseudo**

Excludes pseudogenes identified by `P` as the final character of the gene symbol. Only the final character is evaluated to avoid misclassifying genes belonging to subfamily `P`.

**FunctionalOnly**

Additionally excludes OR genes with a HORDE-reported pseudogene probability score greater than 0.3.

---
## Python environment

All notebooks were developed and tested in **Google Colab**, which comes with 
Python 3 and most dependencies pre-installed. To run the notebooks:

1. Open the `.ipynb` file directly in Google Colab (upload it or open via 
   `File > Open notebook > GitHub` and paste the repository URL).
2. Install any missing packages by running the cells in the notebooks
3. No local Python installation is required.
---
## Gene set construction

The complete construction workflow is available in [`GMT_pipeline/`](./GMT_pipeline).

The directory contains the source OR annotation obtained from HORDE and three notebooks corresponding to the three GMT files:

* `HUMAN_OR_families_withPseudo_gmt.ipynb`
* `HUMAN_OR_families_noPseudo_gmt.ipynb`
* `HUMAN_OR_families_FunctionalOnly_gmt.ipynb`

Custom GMT Generation:

In addition to using the pre-built GMT files, users can adapt these Google Colab notebooks to build custom OR gene sets. By modifying the filtering logic, you can apply your own thresholds for HORDE pseudogene probability scores or implement custom criteria for pseudogene exclusion to fit your specific research context.

The HORDE dataset used for the current release was accessed in **August 2026**.

The workflow was performed in Python 3 using Google Colab.

---
## How to use OR-GSEA

The GMT files are compatible with standard GSEA software (https://www.gsea-msigdb.org/gsea/index.jsp) and can be used as a custom gene set database.

### Pre-ranked GSEA

Ready-to-use ranked files are available in [`RNK_files/`](./RNK_files):

* `data_rank.rnk` — pre-ranked mean log2 fold-change values for all detected genes.
* `data_rank_ORonly.rnk` — pre-ranked mean log2 fold-change values restricted to OR genes, including pseudogenes.

To run an analysis:

1. Download an OR-GSEA `.gmt` file.
2. Prepare or use a ranked `.rnk` file.
3. Select **GSEAPreranked** in GSEA.
4. Use the OR-GSEA `.gmt` file as the gene set database.
5. Run the analysis using parameters appropriate for your dataset.

The provided `.rnk` files are examples based on human olfactory epithelium transcriptomics data from Olender et al. (2016).

---
## Example ssGSEA analysis

An example single-sample GSEA (ssGSEA) analysis is provided in [`ssGSEA_example/`](./ssGSEA_example).

The directory contains:

* `OR_ssGSEA_example.ipynb` — Google Colab notebook containing the step-by-step workflow with detailed code annotations.
* `df_for_ssgsea.xlsx` — example transcriptomics dataset from Olender et al. (2016) used to demonstrate the workflow.

How to run ssGSEA:

1. **Open the notebook**: Launch `OR_ssGSEA_example.ipynb` in Google Colab, where each step of the analysis is explained via inline code comments.
2. **Input data formatting**: You can run the notebook directly with the provided `df_for_ssgsea.xlsx` or substitute it with your own dataset. Custom expression matrices must follow the same structure:
   * **Rows**: Unique gene identifiers (HGNC Gene Symbols).
   * **Columns**: Sample names (containing normalized expression values such as TPM, RPKM, or CPM).
3. **Execute the analysis**: The pipeline uses `gseapy.ssgsea()` to compute sample-specific normalized enrichment scores ($NES$) across the OR family gene sets.
---

## Data sources

**Human Olfactory Data Explorer (HORDE)**
https://genome.weizmann.ac.il/horde/app/webroot/

OR family classification and pseudogene information were obtained from HORDE.

**Olender et al. (2016)**
Transcriptomics data used for the example GSEA and ssGSEA analyses.

DOI: https://doi.org/10.1186/s12864-016-2960-3

---   
## Repository structure

```text
OR-GSEA/
│
├── GMT_files/
│   ├── HUMAN_OR_families_withPseudo_v01.gmt
│   ├── HUMAN_OR_families_noPseudo_v01.gmt
│   └── HUMAN_OR_families_FunctionalOnly_v01.gmt
│
├── GMT_pipeline/
│   ├── genes_fromHORDE.csv
│   ├── HUMAN_OR_families_withPseudo_gmt.ipynb
│   ├── HUMAN_OR_families_noPseudo_gmt.ipynb
│   └── HUMAN_OR_families_FunctionalOnly_gmt.ipynb
│
├── RNK_files/
│   ├── data_rank.rnk
│   └── data_rank_ORonly.rnk
│
└── ssGSEA_example/
    ├── OR_ssGSEA_example.ipynb
    └── df_for_ssgsea.xlsx
