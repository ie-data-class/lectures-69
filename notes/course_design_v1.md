# Industrial Data Analysis — Course Design (Second Half)

**Instructor:** Nirand P., CMU-NR  
**Format:** 7-week half-semester, independently assessed  
**Students:** 3rd-year Industrial Engineering undergraduates

---

## Context & Student Profile

This course is the second half of a full-semester offering. The first half (taught by Dr. Nattida Tachaboon) covered descriptive statistics, data visualization, Pivot Tables, regression, and optimization — all delivered through Microsoft Excel and Google Sheets.

Students entering this half bring:

- Foundational statistics: mean, variance, skewness, regression, R², residual analysis
- Excel-based data analysis fluency (Pivot Tables, Solver, Pareto charts)
- Introductory Python programming (syntax, basic scripting)
- Quality control concepts from a prior QC course
- **No prior exposure to DOE**

The key transition this half makes is from Excel-based, point-and-click analysis to Python-based, programmable analysis applied to realistic industrial datasets.

---

## Learning Philosophy: Two-Layer Approach

### The Problem This Addresses

In the current AI environment, students can generate correct-looking Python code without understanding what the code computes. Code submissions alone are no longer sufficient evidence of comprehension. At the same time, this is a data analytics course — not a pure statistics or QC course — so the goal is not to make students do everything by hand.

### The Solution: Small Data for Understanding, Real Data for Application

Every major concept is taught in two layers:

**Layer 1 — Hand calculation on small, clean datasets.**  
Before touching Python, students work through a representative example by hand on a dataset small enough to compute in a table (e.g., 10 subgroups of 5, or a 2² factorial with 2 replicates). This forces engagement with the algorithm itself. Students who understand the hand calculation understand what the software is doing. This is also the basis for in-class assessment, where AI cannot assist.

**Layer 2 — Python on real industrial data.**  
Once the concept is understood, Python is introduced as the tool that makes the same algorithm tractable on thousands of rows of messy sensor data. The emphasis is on _reading and interpreting_ Python output, not on syntax mastery. Students are not expected to write complex code from memory — they are expected to understand every number the code produces.

### Assessment Implications

| Evidence type        | Method                                     | What it proves                       |
| -------------------- | ------------------------------------------ | ------------------------------------ |
| In-class quiz / exam | Small dataset, no computer                 | Algorithmic understanding (AI-proof) |
| Assignments          | Interpret Python output, explain decisions | Analytical judgment                  |
| Mini project         | Full pipeline on a real dataset            | Applied competency                   |

Assignment questions shift from _"write code that does X"_ to _"here is the output — what does it tell you and what would you do next?"_ Interpretation questions test understanding far more robustly than code generation tasks.

---

## Weekly Schedule

### Module 1: Python for Industrial Data Processing (Weeks 1–2)

**Week 1 — Bridging from Excel to Python**

The opening week explicitly connects Python tools to what students already know from the first half. The goal is to reduce cognitive load by anchoring new syntax to familiar concepts.

| Excel concept      | Python equivalent            |
| ------------------ | ---------------------------- |
| Spreadsheet table  | `pandas` DataFrame           |
| Pivot Table        | `.groupby()` + `.agg()`      |
| Excel chart        | `matplotlib` / `seaborn`     |
| CORREL(), LINEST() | `scipy.stats`, `statsmodels` |

Class activity: reproduce a Pareto chart and a scatter plot from the first half — first in Excel (review), then in Python (new). Students see the same result via a different route.

**Week 2 — Data Cleaning and Preparation**

Core topics:

- Handling missing values: forward-fill, interpolation, and when each is appropriate
- Outlier detection: IQR method and Z-score (hand calculation on 10-point dataset first, then `scipy.stats` on full sensor log)
- Feature engineering: rolling windows, time-lagged variables in `pandas`

Hand-calculation anchor: Given 10 sensor readings, compute Q1, Q3, IQR, and the outlier fences by hand. Identify which points are flagged. Then apply `df.describe()` and IQR logic in Python to a 10,000-row dataset.

Python stack: `pandas`, `numpy`, `scipy.stats`, `matplotlib`, `seaborn`

---

### Module 2: Statistical Process Control (Weeks 3–4)

**Week 3 — Control Charts in Python**

Core topics:

- The logic of control limits: 3σ from process mean, not specification limits
- X̄-R and I-MR charts: construction, interpretation, run rules
- Automating chart generation from raw timestamped data

Hand-calculation anchor: 10 subgroups, n = 5. Students compute X̄ and R for each subgroup, look up A₂/D₃/D₄ constants, calculate UCL and LCL, and plot by hand. Identify any out-of-control signals. Then Python reproduces the same chart on a full production dataset.

Note: Since students had a prior QC course, the conceptual "why" of control charts can be reviewed quickly. The value-add here is Python automation and dealing with real, timestamped industrial data rather than textbook datasets.

**Week 4 — Advanced SPC and Process Capability**

Core topics:

- EWMA chart: weighted moving average logic, tuning the λ parameter
- Process capability: Cp, Cpk — what they mean and how they differ
- Connecting data quality (Module 1) to process monitoring: clean data is a prerequisite for meaningful control charts

Hand-calculation anchor: Given a small stable process dataset, compute Cp and Cpk by hand. Interpret: is the process capable? Is it centered?

Python emphasis: Automated Phase I / Phase II analysis on a multi-variable sensor dataset. Students flag when the process goes out of control and characterize which signal type (mean shift vs. variance increase) is occurring.

Python stack: `pandas`, `numpy`, `matplotlib`, `scipy.stats`

---

### Module 3: Design of Experiments (Weeks 5–6)

**Week 5 — ANOVA and Single-Factor Experiments**

This week requires the most time investment because hypothesis testing logic (F-statistic, p-value decision, post-hoc tests) was not formally taught in the first half. The regression covered earlier provides useful vocabulary (p-values, model fit) but not the full ANOVA framework.

Core topics:

- One-way ANOVA: variance decomposition into SS_between and SS_within
- The F-statistic: why we compare two variance estimates
- Tukey's HSD: when the F-test tells you something is different, but not what

Hand-calculation anchor: 3 machine settings, 4 replicates each (12 observations total). Build the ANOVA table by hand — SS, MS, F. Compare to F-critical. Then run `statsmodels` on the same data and match every number.

**Week 6 — Full Factorial Designs**

Core topics:

- 2² and 2³ factorial designs: the logic of systematically varying all factors
- Main effects and interaction effects: what an interaction means physically
- Effect plots and interaction plots: reading the geometry of the experiment
- Confounding and why it matters

Hand-calculation anchor: 2² factorial with 2 replicates (8 runs). Compute main effects and the interaction effect using the contrast method. Build the ANOVA table. Identify which effects are significant.

Python emphasis: Analyze a 2³ or 2⁴ factorial dataset from a real manufacturing context (e.g., injection molding or PCB assembly). Use `statsmodels.formula.api` to fit the model and `pyDOE2` to understand the design matrix. Students interpret the coefficient table and interaction plots — they do not write the model from scratch.

_Note: Response Surface Methodology (RSM) and Central Composite Designs are outside the scope of this course. Students who want to go further are pointed to Montgomery's Chapter 11 and the `dexpy` package._

Python stack: `pandas`, `numpy`, `statsmodels`, `scipy.stats`, `matplotlib`, `pyDOE2`

---

### Mini Project (Week 7)

Students work individually or in pairs on a provided multi-factor manufacturing dataset. The project has three required sections:

1. **Data preparation report.** Describe the dataset, identify and handle missing values and outliers, justify the approach. Show key distributions.

2. **Process monitoring.** Construct an appropriate control chart for the primary quality variable. Identify any out-of-control periods. Characterize what type of signal occurred.

3. **Factorial analysis.** A 2² or 2³ factorial experiment is embedded in the dataset. Fit the ANOVA model, interpret main effects and interactions, and state which factor settings minimize the defect metric.

Each section includes a short written narrative — not just code output. Students must explain what they found and what they would recommend to a process engineer. A brief hand-calculation verification (one key result computed manually) is required to demonstrate that students understand what their Python code produced.

---

## Assessment Structure

| Component                                | Weight | Format                                               |
| ---------------------------------------- | ------ | ---------------------------------------------------- |
| In-class quizzes (×2, after M1 and M2)   | 30%    | Closed-book, hand calculation on small datasets      |
| Module assignments (×2, after M2 and M3) | 30%    | Interpret Python output, answer analytical questions |
| Mini project                             | 30%    | Full analysis with written narrative                 |
| Attendance and participation             | 10%    | —                                                    |

---

## Recommended Resources

| Module   | Primary Reference                                                   | Python Reference                                                |
| -------- | ------------------------------------------------------------------- | --------------------------------------------------------------- |
| Module 1 | McKinney, _Python for Data Analysis_ (Ch. 7–10)                     | `pandas` documentation, `scipy.stats` preprocessing guide       |
| Module 2 | Montgomery, _Introduction to Statistical Quality Control_ (Ch. 5–7) | Custom `matplotlib` SPC scripts (provided)                      |
| Module 3 | Montgomery, _Design and Analysis of Experiments_ (Ch. 3–5)          | `statsmodels.formula.api` documentation, `pyDOE2` documentation |

---

## Design Decisions and Rationale

**Why drop RSM?** Seven weeks is insufficient to cover RSM with the depth it deserves after also teaching ANOVA and factorial designs from scratch to students with no DOE background. A rushed RSM unit produces superficial understanding. Students who encounter it in industry will have the factorial foundation needed to learn it then.

**Why emphasize output interpretation over code writing?** AI tools make code generation trivially easy but cannot reliably perform industrial reasoning: "given this interaction plot, which factor settings would you run and why?" Shifting assessment toward interpretation questions tests what actually matters for an industrial engineer's career and closes the AI-assisted-cheating gap.

**Why keep some hand calculations?** A student who can compute a control chart limit by hand and match it to Python output has demonstrated genuine understanding of the algorithm. This proof is impossible to fake with AI in a closed-book setting and anchors the Python output in real meaning rather than black-box trust.
