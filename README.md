# Analysis of a Transfer-Learning Experiment

Reproducible statistical analysis of an experiment comparing transfer-learning
models against non-transfer baselines, written in R Markdown. The analysis
underlies our 4-page report.

## Research questions
- **RQ1** — Does transfer learning beat non-transfer baselines?
- **RQ2** — How do the *number* and *identity* of training datasets, and the
  *depth of refinement*, affect performance?

## Data
Each row is one run: a metric **score in [0,1]** for a `model` on a test set
(`TeD1`–`TeD7`), with eight 0/1 indicators (`TrD1`–`TrD8`) marking which training
datasets were used. Models are three baselines (`B1`–`B3`), a single-source
transfer model (`MS`), and five multi-source models (`MN`, `M1`–`M3`, `MF`).

## Methods
- The seven test sets differ in scale ~15×, so we **standardize the score within
  each test set** (z-score) and report effects in SD units, comparable across tasks.
- A **Type-II ANOVA** on a large preliminary model guides the analysis (the test
  set explains ~93% of the variance and is controlled for throughout).
- Effects are estimated with **linear models + estimated marginal means**
  (`emmeans`/`emtrends`), reported as **effect sizes with 95% CIs**.
- Conclusions are confirmed with **logit** and **quasi-binomial** models that
  respect the [0,1] bound.

## Key findings
- **RQ1:** Transfer learning beats the baselines on **6 of 7** test sets
  (~1.9 SD overall); the gain grows with refinement depth (`MS < MN < M1 < … < MF`).
- **RQ2:** More training datasets help with **diminishing returns** (~0.14 SD each);
  deeper refinement helps more (~0.28 SD per part); dataset *identity* is the
  stronger lever — `TrD8/TrD5/TrD7` help most, **`TrD1` hurts**, and a dataset's
  usefulness **depends on the target task** (strong TrD×TeD interaction).

## Reproduce
Requires **R ≥ 4.1** (uses the native `|>` pipe).

```r
install.packages(c("tidyverse", "car", "emmeans", "broom", "patchwork"))
```

Open `analysis.Rmd` in RStudio (with `data.csv` in the same folder) and click
**Knit**, or run:

```r
rmarkdown::render("analysis.Rmd")
```

The analysis is fully deterministic — no random seed needed; results are identical
on every run.
