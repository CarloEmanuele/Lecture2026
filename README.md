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
│   └── anscombe.csv
└── Lecture1/
    ├── exercises_eda.ipynb
    └── solutions_eda.zip      (password protected)
```

## Getting started

You need Python 3 with `pandas`, `matplotlib` and Jupyter:

```bash
pip install pandas matplotlib jupyter
jupyter notebook
```

Open the notebooks from their own lecture folder. They load data with relative paths
(`../Datasets/...`), so the working directory must be the lecture folder.

### Solutions

The solutions are in password-protected `.zip` archives (AES-256). The password is given in
class after the exercises have been discussed. The built-in Windows/macOS extractors may not
support AES encryption, so use [7-Zip](https://www.7-zip.org/) (Windows),
[Keka](https://www.keka.io/) or The Unarchiver (macOS), or the command line:

```bash
7z x Lecture1/solutions_eda.zip -oLecture1
```

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
| [solutions_eda.zip](Lecture1/solutions_eda.zip) | A reference solution for each exercise, with a statistical explanation and references for each trap in Exercise 6 (password protected) |

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

## Author

Emanuele (Carlo) Villa — [carlo.villa@unimi.it](mailto:carlo.villa@unimi.it)
