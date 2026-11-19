# Lectures 2026

Course materials: Jupyter notebooks with exercises and solutions, plus the datasets they use.
New lectures will be added as the course goes on.

## Repository structure

```
Lecture2026/
├── Datasets/
│   ├── penguins.csv
│   ├── titanic.csv
│   ├── icecream_sharks.csv
│   ├── berkeley_admissions.csv
│   ├── galton_heights.csv
│   ├── anscombe.csv
│   ├── e18_mouse_brain_counts.npz
│   ├── e18_mouse_brain_genes.csv
│   ├── visiumhd_mouse_brain_cortex_2um.npz
│   └── visiumhd_mouse_brain_gene_means.csv
├── Lecture1/
│   ├── exercises_eda.ipynb
│   └── solutions_eda.ipynb
├── Lecture2/
│   ├── exercises_signal_noise.ipynb
│   ├── solutions_signal_noise.ipynb
│   └── PLAN.md
├── Lecture3/
│   ├── exercises_sampling.ipynb
│   ├── solutions_sampling.ipynb
│   └── PLAN.md
└── environment.yml
```

## Getting started

You need Python 3 with `numpy`, `pandas`, `scipy`, `matplotlib` and Jupyter. The recommended way is a
conda (or mamba) environment, created from [environment.yml](environment.yml):

```bash
# with conda (Miniconda / Anaconda / Miniforge)
conda env create -f environment.yml
conda activate lectures2026

# or, with mamba (same commands, faster)
mamba env create -f environment.yml
mamba activate lectures2026
```

To update an existing environment after `environment.yml` changes:

```bash
conda env update -f environment.yml --prune      # or: mamba env update -f environment.yml --prune
```

If you prefer to install the packages by hand in an environment you already have:

```bash
conda install -c conda-forge numpy pandas scipy matplotlib jupyter ipykernel
```

### Running the notebooks in VS Code

1. Install the **Python** and **Jupyter** extensions (Microsoft).
2. Open the repository folder (`File → Open Folder…`), then open a notebook.
3. Click **Select Kernel** (top right) → **Python Environments…** → choose `lectures2026`.
   If it does not appear, run `Python: Select Interpreter` from the command palette (`Ctrl+Shift+P`), pick
   `lectures2026`, and reload the window.

VS Code runs a notebook with its own folder as the working directory, which is what the notebooks expect.

### Running the notebooks with Jupyter

```bash
conda activate lectures2026
jupyter notebook
```

Open the notebooks from their own lecture folder. They load data with relative paths
(`../Datasets/...`), so the working directory must be the lecture folder.

## Lectures

### Lecture 1 — EDA and basic programming by decomposition

An introduction to **exploratory data analysis (EDA)** with pandas, built around one way of
working: **break the problem down before writing code**. Each question is split into a tree of
smaller sub-problems (*functional decomposition*, *top-down design*, *stepwise refinement*,
Wirth 1971) until every leaf is small enough to become one function. A "root" function then
calls the leaves in order.

| Notebook | Content |
|---|---|
| [exercises_eda.ipynb](Lecture1/exercises_eda.ipynb) | Questions, decomposition trees and function stubs (`# TODO`) to complete |
| [solutions_eda.ipynb](Lecture1/solutions_eda.ipynb) | A reference solution for each exercise, with a statistical explanation and references for each trap in Exercise 6 |

Exercises:

1. **Is the dataset ready for analysis?** (penguins): shape, column types, missing values, duplicates.
2. **Descriptive statistics by group** (penguins): count, mean, median and standard deviation of body mass by species and sex.
3. **Look before you conclude** (penguins): bill length vs. bill depth, overall and within each species (Simpson's paradox).
4. **Who survived?** (titanic): survival rate by sex, by class and by sex × class, with a grouped bar chart.
5. **A small complete pipeline** (titanic): students draw the tree themselves and build `clean → add_features → summarize`, dealing with missing ages, age groups and family size.
6. **Statistical traps**: four short cases where the data seem to say one thing and say another.
   - 6a, *Do ice creams cause shark attacks?* Correlation ≠ causation: a confounder, stratification and partial correlation.
   - 6b, *Did Berkeley discriminate against women?* Simpson's paradox on aggregated (count) data.
   - 6c, *The cover jinx.* Regression to the mean, with Galton's heights and a simulation of athletes.
   - 6d, *Can you trust summary statistics?* Anscombe's quartet.

Each exercise ends with a discussion question. These have no single right answer and are meant
to be talked through in class.

### Lecture 2 — Signal and noise: from single genes to signatures

Why a single marker gene was enough with bulk data, and why it is not any more with single-cell and spatial
data (Visium HD): few molecules per cell or bin mean that **zero is the most likely reading** even for an
expressed gene, so information must be pooled across genes (signatures), space (bins, smoothing) or cells
(clusters, pseudobulk). Teaching notes: [Lecture2/PLAN.md](Lecture2/PLAN.md).

| Notebook | Content |
|---|---|
| [exercises_signal_noise.ipynb](Lecture2/exercises_signal_noise.ipynb) | Questions, decomposition trees and function stubs |
| [solutions_signal_noise.ipynb](Lecture2/solutions_signal_noise.ipynb) | Reference solutions with comments and references |

Exercises:

1. **How many molecules do you need to see a gene?** (simulation): detection 1 − e^(−λ) and CV ≈ 1/√λ, from bulk to a 2 µm bin.
2. **One marker or a signature?** (simulation + E18 cells): separability vs number of markers and depth; how often *Pax6* is zero in progenitor-like cells.
3. **Zeros in real data: dropout or sampling?** (E18 cells): observed zeros vs Poisson / negative binomial.
4. **Visium HD: resolution against signal**: aggregating 2 µm bins to 4–32 µm; single gene vs signature maps; split-half reliability by count splitting.
5. **Populations come from single-cell data** (Visium HD): label transfer with atlas signatures, with and without smoothing.
6. **Analysis traps**: 6a depth and normalisation, 6b double dipping, 6c pseudoreplication, 6d spillover in spatial data.

### Lecture 3 — Sampling in time, space and populations

Every dataset is a sample at nested levels (time point → organoid/batch → section → cell or bin → molecules).
At which level are we replicating, and at which level do we want to generalise?
Teaching notes: [Lecture3/PLAN.md](Lecture3/PLAN.md).

| Notebook | Content |
|---|---|
| [exercises_sampling.ipynb](Lecture3/exercises_sampling.ipynb) | Questions, decomposition trees and function stubs |
| [solutions_sampling.ipynb](Lecture3/solutions_sampling.ipynb) | Reference solutions with comments and references |

Exercises:

1. **The peak that wasn't there** (simulation): transient expression and sparse time courses.
2. **How old is this organoid?** (simulation): calendar age vs transcriptomic age against a fetal reference.
3. **Pseudotime from a snapshot** (simulation): PC1 vs kNN-graph pseudotime, with and without the intermediate states.
4. **A slice of an organoid** (simulation): how many ventricular-like rosettes a 2D section shows.
5. **Neighbouring bins are not independent** (simulation + Visium HD): false positives with bins as units; Moran's I.
6. **How many cells do I need?**: sampling rare populations.
7. **Population traps**: 7a compositional data, 7b cells vs organoids, 7c batch confounded with condition, 7d dissociation bias.

## Datasets

### `penguins.csv` — Palmer Penguins

344 penguins from three species, observed on three islands of the Palmer Archipelago
(Antarctica) between 2007 and 2009. The data were collected by Dr. Kristen Gorman and the
Palmer Station LTER, and published in the `palmerpenguins` package (Horst, Hill & Gorman, 2020).

| Column | Description |
|---|---|
| `species` | Adelie (152), Gentoo (124), Chinstrap (68) |
| `island` | Biscoe, Dream, Torgersen |
| `bill_length_mm` | Bill (culmen) length, mm |
| `bill_depth_mm` | Bill (culmen) depth, mm |
| `flipper_length_mm` | Flipper length, mm |
| `body_mass_g` | Body mass, g |
| `sex` | `male` / `female` |
| `year` | Year of observation (2007–2009) |

Missing values are coded as `NA`: 2 rows have no measurements at all, and 11 rows have no `sex`.

### `titanic.csv` — Titanic passengers

891 passengers of the RMS Titanic (the training set of the well-known Kaggle competition, in the
version shipped with `seaborn`). Several columns repeat the same information in a different form.
Exercise 4 asks about this on purpose.

| Column | Description |
|---|---|
| `survived` | 1 = survived, 0 = died (342 survivors) |
| `pclass` | Ticket class: 1, 2, 3 |
| `sex` | `male` / `female` |
| `age` | Age in years (177 missing, about 20%) |
| `sibsp` | Number of siblings/spouses aboard |
| `parch` | Number of parents/children aboard |
| `fare` | Ticket fare |
| `embarked` | Port of embarkation: S, C, Q (2 missing) |
| `class` | Same as `pclass`, as text (First, Second, Third) |
| `who` | `man`, `woman`, `child` |
| `adult_male` | True for adult men |
| `deck` | Cabin deck, A–G (688 missing) |
| `embark_town` | Same as `embarked`, as text (Southampton, Cherbourg, Queenstown) |
| `alive` | Same as `survived`, as text (`yes` / `no`) |
| `alone` | True if travelling with no family (`sibsp + parch == 0`) |

### `icecream_sharks.csv` — Ice creams and shark attacks (synthetic)

261 weeks (2020–2024) for a fictional seaside town. The data are **synthetic**: temperature drives both
ice-cream sales and the number of swimmers, and only swimmers affect shark attacks. Used in Exercise 6a.

| Column | Description |
|---|---|
| `week` | Week start date (Monday) |
| `mean_temperature_c` | Mean temperature, °C |
| `ice_creams_sold` | Ice creams sold in the week |
| `estimated_swimmers` | Estimated number of swimmers |
| `shark_attacks` | Number of shark attacks |

### `berkeley_admissions.csv` — UC Berkeley graduate admissions, 1973

Applications to the six largest departments of UC Berkeley in fall 1973 (4526 applicants), from
Bickel, Hammel & O'Connell (1975). The data are **aggregated**: each row is a count, not a person.
Used in Exercise 6b.

| Column | Description |
|---|---|
| `outcome` | `admitted` / `rejected` |
| `gender` | `M` / `F` |
| `department` | A–F (anonymized) |
| `n` | Number of applicants in that cell |

### `galton_heights.csv` — Galton's parent and child heights, 1886

928 parent–child pairs from Francis Galton's study of hereditary stature, converted from inches to cm.
Female heights were scaled by 1.08 by Galton, and the values are rounded into classes. Used in Exercise 6c.

| Column | Description |
|---|---|
| `midparent_height_cm` | Mean height of the two parents, cm |
| `child_height_cm` | Height of the adult child, cm |

### `anscombe.csv` — Anscombe's quartet

Four datasets of 11 (x, y) points each, with nearly identical summary statistics and very different
shapes (Anscombe, 1973). Used in Exercise 6d.

| Column | Description |
|---|---|
| `dataset` | `I`, `II`, `III`, `IV` |
| `x`, `y` | Coordinates of the point |

### `e18_mouse_brain_*` — 1,301 cells from an E18 mouse brain

10x Genomics public dataset *1k Brain Cells from an E18 Mouse* (Chromium v3): cells from the cortex,
hippocampus and ventricular zone of an embryonic day 18 mouse. Genes detected in fewer than 10 cells were
removed. Used in Lecture 2.

| File | Content |
|---|---|
| `e18_mouse_brain_counts.npz` | Sparse UMI count matrix, cells × genes (load with `scipy.sparse.load_npz`) |
| `e18_mouse_brain_genes.csv` | Gene symbols, in the column order of the matrix |

### `visiumhd_mouse_brain_*` — Visium HD, adult mouse brain

10x Genomics public dataset *Visium HD Mouse Brain* (fresh frozen, coronal section). Used in Lectures 2 and 3.

| File | Content |
|---|---|
| `visiumhd_mouse_brain_cortex_2um.npz` | A 1.7 × 1.7 mm crop (striatum → white matter → cortex → pia) at the native 2 µm bin size, 67 marker genes. Arrays: `data`, `indices`, `indptr`, `shape` (sparse CSR matrix bins × genes), `row`, `col` (bin position in the crop), `total_counts` (UMIs per bin over **all** genes), `genes`. Load with `np.load` |
| `visiumhd_mouse_brain_gene_means.csv` | For every gene, the mean count per bin over the whole tissue at 2, 8 and 16 µm |

## Author

Emanuele (Carlo) Villa — [carlo.villa@unimi.it](mailto:carlo.villa@unimi.it)
