# Final Year Project

Comparison of from-scratch machine learning algorithms across benchmark datasets, with an extended strict-evaluation Bitcoin reporting pipeline.

## What This Repository Is

This project implements and compares three supervised learning algorithms built largely from scratch:

- `KNNClassifier`
- `DecisionTreeClassifier`
- `LinearSVM_Pegasos` plus a One-vs-Rest multiclass wrapper

The repository contains:

- reusable loaders and preprocessing utilities
- cross-validation and metric code
- grid-search experiment runners
- final evaluation scripts comparing custom models against `scikit-learn`
- Bitcoin-specific strict temporal evaluation and report-generation scripts
- saved result CSVs, prediction exports, and dissertation-ready figures

## Important Working Directory Note

Most runnable scripts use paths like `data/raw/...` and `results/...` relative to `src/`.

That means the safest way to run the project is:

1. Open a terminal in the repository root.
2. Create/activate the environment.
3. Install dependencies from `requirements.txt`.
4. Change into `src/`.
5. Run scripts from there.

Example:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
cd src
```

## Dependencies

The project expects Python 3.10+ and the packages listed in `requirements.txt`.

Main libraries used:

- `numpy`
- `pandas`
- `scipy`
- `scikit-learn`
- `matplotlib`
- `seaborn`
- `plotly`
- `pytest`

## Repository Navigation

Authoritative project code, datasets, and outputs live under `src/`.

### Top level

- `README.md`: this guide
- `Diary.md`: implementation diary / development log
- `requirements.txt`: Python dependencies
- `docs/`: lightweight top-level notes
- `results/`: lightweight top-level placeholder output folders

### Inside `src/`

- `algorithms/`: from-scratch model implementations
- `evaluation/pipeline.py`: data loading helpers, preprocessing, train/test split, CV, metrics
- `data_utils.py`: dataset-specific loaders
- `grid_search.py`: reusable grid-search utility
- `bitcoin_report_utils.py`: strict Bitcoin tuning/evaluation/report helpers
- `scripts/experiments/`: main experiment entrypoints
- `scripts/reporting/`: result summarisation, plotting, evidence-pack generation
- `scripts/legacy/`: older one-off visualisation scripts kept for reference
- `tests/`: runnable test and benchmark scripts
- `data/raw/`: benchmark datasets used by the experiments
- `results/`: authoritative generated outputs used by the dissertation/report
- `docs/`: repo-layout and reporting notes

## Core Files And What They Do

### Model implementations

- `src/algorithms/knn.py`: custom k-NN classifier
- `src/algorithms/decision_tree.py`: custom decision tree using Gini/entropy-based splitting
- `src/algorithms/svm_linear.py`: custom linear SVM trained with a Pegasos-style optimisation routine
- `src/algorithms/svm_multiclass.py`: One-vs-Rest wrapper for multiclass SVM runs

### Shared utilities

- `src/evaluation/pipeline.py`: CSV/ARFF loading, imputation, scaling, label encoding, train/test split, stratified CV, grouped CV, metrics
- `src/data_utils.py`: loaders for Iris, Bank Marketing, Breast Cancer, and Bitcoin Heist
- `src/grid_search.py`: generic grid-search runner that records accuracy, precision, recall, F1, ROC AUC, and average precision where available
- `src/bitcoin_report_utils.py`: strict Bitcoin workflow helpers for temporal splitting, grouped CV, threshold selection, model construction, and scoring

### Main experiment runners

- `src/scripts/experiments/run_knn_grid.py`: KNN grid search on Iris and Bank datasets
- `src/scripts/experiments/run_decision_grid.py`: Decision Tree grid search on Iris and Bank datasets
- `src/scripts/experiments/run_svm_grid.py`: SVM grid search on Iris and Bank datasets
- `src/scripts/experiments/run_breast_cancer_grid.py`: grid searches on Breast Cancer
- `src/scripts/experiments/run_bitcoin_grid.py`: earlier Bitcoin grid search path
- `src/scripts/experiments/run_full_pipeline.py`: runs the main benchmark grid searches, summarises best params, and performs final evaluation
- `src/scripts/experiments/summarise_best_models.py`: extracts best rows from grid-search CSVs
- `src/scripts/experiments/final_evaluate_all_models.py`: compares from-scratch models against `scikit-learn` on final splits

### Bitcoin strict report pipeline

- `src/scripts/experiments/run_bitcoin_grid_report.py`: report-focused strict Bitcoin tuning with grouped CV across years
- `src/scripts/experiments/run_bitcoin_final_report_eval.py`: strict holdout evaluation using the tuned Bitcoin configs
- `src/scripts/reporting/analyse_bitcoin_results.py`: creates Bitcoin summary CSV/Markdown from report outputs
- `src/scripts/reporting/plot_report_figures.py`: creates dissertation-ready Bitcoin figures
- `src/scripts/reporting/build_chapter5_results_pack.py`: builds a larger Chapter 5 evidence pack and figure/table bundle

### Final prediction export and comparison

- `src/scripts/reporting/export_final_predictions.py`: exports paired custom vs sklearn predictions
- `src/scripts/reporting/analyse_final_predictions.py`: disagreement analysis, McNemar tests, and supporting plots/tables

### Tests

- `src/tests/test_knn.py`
- `src/tests/test_decision_tree.py`
- `src/tests/test_svm_crossval.py`
- `src/tests/test_svm_on_iris_binary.py`
- `src/tests/test_comparison.py`

## Recommended Setup

From the repository root:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
cd src
```

Optional quick check:

```powershell
pytest tests -q
```

Note: some legacy test scripts are easiest to run through `pytest` from inside `src/`, because direct `python path\to\script.py` execution does not always set up imports consistently.

## Execution Order

There are two main ways to use the repo, depending on what you want to reproduce.

### 1. Main benchmark pipeline

Use this if you want the general KNN / Decision Tree / SVM comparison across the standard datasets.

Run from `src/`:

```powershell
python .\scripts\experiments\run_full_pipeline.py
```

What it does:

1. Runs `run_knn_grid.py`
2. Runs `run_decision_grid.py`
3. Runs `run_svm_grid.py`
4. Runs `run_bitcoin_grid.py`
5. Runs `run_breast_cancer_grid.py`
6. Runs `summarise_best_models.py`
7. Runs `final_evaluate_all_models.py`

Main outputs:

- `results/knn_grid_*.csv`
- `results/decision_tree_grid_*.csv`
- `results/svm_grid_*.csv`
- `results/breast_cancer_grid_*.csv`
- `results/best_hyperparams_summary.csv`
- `results/final_evaluation_all_models_strict.csv`

### 2. Strict Bitcoin report pipeline

Use this if you want the final dissertation-style Bitcoin results with a more defensible temporal split.

Run from `src/` in this order:

```powershell
python .\scripts\experiments\run_bitcoin_grid_report.py
python .\scripts\experiments\run_bitcoin_final_report_eval.py
python .\scripts\reporting\analyse_bitcoin_results.py
python .\scripts\reporting\plot_report_figures.py
python .\scripts\reporting\build_chapter5_results_pack.py
```

What each step does:

1. `run_bitcoin_grid_report.py`
   Tunes Decision Tree and SVM on pre-holdout years only using grouped CV across years.
2. `run_bitcoin_final_report_eval.py`
   Applies the selected configs to a strict unseen holdout year and records metrics/predictions.
3. `analyse_bitcoin_results.py`
   Produces compact summary tables and Markdown interpretation.
4. `plot_report_figures.py`
   Produces the main static Bitcoin figures.
5. `build_chapter5_results_pack.py`
   Produces the broader evidence-pack tables, figures, and source-of-truth notes.

Main outputs:

- `results/bitcoin_report_grid_decisiontree.csv`
- `results/bitcoin_report_grid_svm.csv`
- `results/bitcoin_report_best_configs.csv`
- `results/bitcoin_report_final_metrics.csv`
- `results/bitcoin_report_holdout_predictions.csv`
- `results/bitcoin_report_summary.csv`
- `results/bitcoin_report_summary.md`
- `results/figures/*.png`
- `results/report_chapter5_evidence_pack.md`


```

## Fast / Smoke-Test Runs

Some scripts support bounded runs for quicker checks.

From `src/`:

```powershell
$env:FAST_PIPELINE="1"
python .\scripts\experiments\run_full_pipeline.py
```

Or for the strict Bitcoin pipeline:

```powershell
python .\scripts\experiments\run_bitcoin_grid_report.py --fast --max-configs 4
python .\scripts\experiments\run_bitcoin_final_report_eval.py --fast
```

## Authoritative Result Files

If you only need the final numbers/files to cite:

- Non-Bitcoin final comparison: `src/results/final_evaluation_all_models_strict.csv`
- Best benchmark hyperparameters: `src/results/best_hyperparams_summary.csv`
- Strict Bitcoin tuning outputs: `src/results/bitcoin_report_grid_decisiontree.csv`, `src/results/bitcoin_report_grid_svm.csv`
- Strict Bitcoin selected configs: `src/results/bitcoin_report_best_configs.csv`
- Strict Bitcoin final metrics: `src/results/bitcoin_report_final_metrics.csv`
- Strict Bitcoin holdout predictions: `src/results/bitcoin_report_holdout_predictions.csv`
- Chapter 5 evidence pack: `src/results/report_chapter5_evidence_pack.md`

## Practical Caveats

- Run commands from `src/`, not from the repository root.
- The newer Bitcoin report pipeline is the authoritative Bitcoin path.
- Older files under `scripts/legacy/` are useful references, but they are not the main source-of-truth workflow.
- The strict Bitcoin path is intentionally more conservative than the older random-split path.
- Some test scripts are closer to runnable experiment demos than unit tests.

## Minimal Reproduction Paths

If you want the shortest possible route:

### Reproduce the main benchmark comparison

```powershell
cd src
python .\scripts\experiments\run_full_pipeline.py
```

### Reproduce the final Bitcoin report outputs

```powershell
cd src
python .\scripts\experiments\run_bitcoin_grid_report.py
python .\scripts\experiments\run_bitcoin_final_report_eval.py
python .\scripts\reporting\analyse_bitcoin_results.py
python .\scripts\reporting\plot_report_figures.py
```
