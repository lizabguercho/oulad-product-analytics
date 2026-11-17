# OULAD Product Analytics — Student Engagement & Retention

Product analytics project exploring **when and which students withdraw** from Open University online courses, using the [Open University Learning Analytics Dataset (OULAD)](https://analyse.kmi.open.ac.uk/open-dataset).

The goal is to identify withdrawal patterns across modules, presentations, and student groups, and to surface product opportunities that could improve retention (subject to further testing).

---

## Business problem

Online course providers lose a large share of enrollments to withdrawal. This project asks:

- How high is overall withdrawal, and where is it concentrated?
- Which modules and presentations show elevated risk?
- How do student characteristics (previous attempts, education, age, gender, region) relate to withdrawal?
- Where should retention support be prioritized — by **rate**, by **volume**, or both?

**Business value:** Findings can inform hypotheses for product changes (e.g. earlier support for high-risk groups or modules). Causal claims and intervention impact would require further testing.

---

## Dataset

**Source:** [OULAD](https://analyse.kmi.open.ac.uk/open-dataset) (Kuzilek, Hlosta & Zdrahal, 2017)

| Table | Role in this project |
|---|---|
| `studentInfo` | Enrollment outcomes (`final_result`), demographics |
| `courses` | Module / presentation metadata |
| `studentRegistration` | Registration / unregistration timing |
| `studentVle` / `vle` | Learning activity (planned for engagement analysis) |
| `assessments` / `studentAssessment` | Assessment structure and scores |

**Unit of analysis:** course enrollment (`id_student` + `code_module` + `code_presentation`), not unique students.

**Core metric:**

```text
withdrawal rate = enrollments with final_result = Withdrawn / total enrollments
```

---

## What’s in the analysis so far

Notebook: [`notebooks/01_data_exploration.ipynb`](notebooks/01_data_exploration.ipynb)

1. **Data loading & quality checks** — shape, missing values, composite-key uniqueness  
2. **Withdrawal by module / presentation / year** — rates and absolute counts  
3. **Demographics** — previous attempts, education, age, gender, region  
4. **Statistical checks** — e.g. confidence intervals and chi-square where relevant  
5. **Product implications** — prioritize by both risk rate and withdrawal volume  

### Selected findings (from exploration)

- Overall withdrawal rate: **~31.2%** (10,156 of 32,593 enrollments)
- **CCC** has the highest module withdrawal rate (~44.5%); **FFF** has the highest absolute withdrawals
- Withdrawal rates were generally higher in **2014** than in **2013**
- Students with **previous attempts** show a higher withdrawal rate, but **first-time students** account for most withdrawals by volume
- Lower educational attainment is associated with higher withdrawal rates; associations are correlational, not causal

---

## Tech stack

- Python 3.11+
- pandas, numpy, matplotlib, seaborn, statsmodels
- Jupyter notebooks
- Environment managed with [uv](https://github.com/astral-sh/uv)

Planned / next: deeper engagement metrics (activation, weekly retention), SQL, and Power BI dashboarding.

---

## Project structure

```text
oulad-product-analytics/
├── data/
│   ├── project_brief.md      # Business problem framing
│   ├── AGENTS.md             # Analysis learning rules
│   └── raw/                  # OULAD CSVs (not all committed — see below)
├── notebooks/
│   └── 01_data_exploration.ipynb
├── docs/                     # Metric notes / write-ups
├── pyproject.toml
├── uv.lock
└── README.md
```

---

## Setup

### 1. Clone and install

```bash
git clone <your-repo-url>
cd oulad-product-analytics
uv sync
```

### 2. Get the data

Download OULAD from the [official Open University page](https://analyse.kmi.open.ac.uk/open-dataset), unzip, and place the CSV files in `data/raw/`:

```text
data/raw/
  studentInfo.csv
  courses.csv
  studentRegistration.csv
  studentVle.csv
  vle.csv
  assessments.csv
  studentAssessment.csv
```

> Large files (`studentVle.csv`, the dataset zip) are gitignored so the repo stays pushable to GitHub.

### 3. Jupyter kernel (optional, for Cursor / VS Code)

```bash
uv run python -m ipykernel install --user --name=oulad-product-analytics --display-name="Python (oulad)"
```

Open `notebooks/01_data_exploration.ipynb` and select the **`.venv`** / **Python (oulad)** kernel.

### 4. Run the notebook end-to-end from the terminal

```bash
uv run jupyter nbconvert --to notebook --execute notebooks/01_data_exploration.ipynb --inplace
```

---

## Method notes

- Withdrawal is defined from `final_result == "Withdrawn"`.
- Comparisons report both **withdrawal rate** and **withdrawal count**.
- Statistical significance does **not** imply causation.
- Recommendations are framed as hypotheses for further investigation / testing.

---

## License & attribution

Analysis code in this repository is available for portfolio use.

The OULAD dataset is published by The Open University; please follow the [dataset terms](https://analyse.kmi.open.ac.uk/open-dataset) when using or redistributing the data.
