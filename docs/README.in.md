# How Can You Think That? Deliberation and the Learning of Opposing Arguments

Gaurav Sood, Robert C. Luskin, and James S. Fishkin

[Paper](ms/main.pdf) · [Research design](docs/design.md) · [Data](docs/data.md)

## Question and motivation

Does deliberation help people articulate reasons for policy views they oppose?
Learning what another side can argue is different from changing one's own
position. The study examines whether a Deliberative Poll on school policy in
Omagh, Northern Ireland, broadens the reasons people can give on both sides.

## Data and research design

The analysis combines a numeric survey, discussion-group roster, and coder
labels for open-ended answers. It compares participants with controls at the
follow-up survey and follows participants from the event to follow-up. Event
attendance was self-selected, and the open-ended outcome was not measured at
recruitment. The participant–control comparison therefore estimates an
association, not a causal effect of attendance.

## Key findings

At follow-up, participants give more coded reasons on average than controls,
but the interval for the adjusted difference includes zero. Participants' reason
counts decline between the event and follow-up, also with an interval spanning
zero. The data do not clearly establish a lasting participant advantage.

{{RESULTS_TABLE}}

Estimates are differences in the number of answers containing a coded reason;
intervals are 95% confidence intervals. Uncertainty uses CR2 small-sample
corrections, grouping participants by discussion group and treating controls as
singleton clusters. The noncontemporaneous event–control comparison is reported
in [tabs/main_estimates.csv](tabs/main_estimates.csv), alongside these comparisons.

## Reproduce

The project uses R and XeLaTeX. From the repository root:

```sh
git clone --branch v0.4.5 --depth 1 https://github.com/soodoku/dp_data.git ../dp_data
make restore
make check
```

The public dp_data release supplies all three analytical inputs. A downloaded
release archive works too; extract it to `../dp_data`, or set `DP_DATA_ROOT`
to its location. Builds verify the pinned file hashes before reading data.
See [`docs/data.md`](docs/data.md) for the source contracts and update procedure.

The individual targets are:

```sh
make analysis
make lint
make test
make paper
make ci-docker
```

The compiled paper and Supporting Information are in [`ms/main.pdf`](ms/main.pdf).
Analytical results are
stored together in the generated `data/derived/analysis_results.rds` object.
The pipeline renders publication tables directly with `knitr::kable`; the two
headline CSV files in `tabs/` provide machine-readable estimates.
Inline manuscript numbers come from [`tabs/numbers.tex`](tabs/numbers.tex).


Edit `docs/README.in.md` to change this README; `scripts/05_exhibits.R`
supplies the current result table.

## Files and pipeline

- `data/`: source manifest and ignored generated analysis data
- `scripts/00_config.R`: paths, data contracts, and analysis settings
- `scripts/00_utils.R`: reusable functions
- `scripts/01_prepare_survey.R` and `02_prepare_coding.R`: input preparation
- `scripts/03_build_analysis.R` and `04_estimate.R`: analysis data and estimates
- `scripts/05_exhibits.R`: manuscript exhibits, inline numbers, and this README
- `scripts/98_validate.R`: analytical validation
- `scripts/99_run_all.R`: run the stages in order
- `tests/`: data-contract and coding tests
- `tabs/`: headline CSV results and generated LaTeX table fragments
- `figs/`: generated publication figures
- `ms/`: manuscript, bibliography, and compiled paper
- `docs/`: research-design and data documentation

The source files used by the current pipeline and their SHA-256 hashes are
pinned in [`data/manifest.yaml`](data/manifest.yaml). Sample membership,
demographics, policy attitudes, and group identifiers are reconstructed from the
raw survey export and group roster; historical derived survey variables are not
used.

## Main measurement decisions

The main outcome counts answers that give at least one reason for a school-policy
view. Answers marked nonsense, missing, or vague do not count. For three questions,
code 3 means the answer calls people with that view prejudiced or ignorant. The
main count leaves out that code; a separate check includes it. Another valid code
in the same answer still counts. When coders disagree and no third coder settles
it, the main count is missing for that respondent and survey. We also report
results from each original coder separately.

Non-administered survey waves remain missing. They are never converted to zero.
Uncertainty estimates use CR2 small-sample corrections, clustering participants
by discussion group while treating controls as singleton clusters.

## Data responsibility

The public inputs contain a numeric survey, group roster, and coder labels.
Verbatim responses are excluded. Historical files containing response text remain
in the original repository history and the local source archive; they are not
required to reproduce the paper. See [`docs/data.md`](docs/data.md).

The MIT license covers code and documentation only. It does not relicense the
source data.
