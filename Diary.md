21/10/25: added the initial KNNClassifier file and set up local project structure (/data, /src, README). also sketched out the evaluation and testing process (iris dataset being initial testing and benchmarking dataset and then later expanding on more ucl datasets)

22/10/25: created and fixed the src/ folder and updated initial folders + README.

23/10/25: Collected and added the benchmark datasets (UCI) into data/, noting sources for later citation.

24/10/25:Pre-reading + design notes: k-NN behaviour, distance calc, and leakage pitfalls and drafted loader & preprocessing API surface (DataLoader, DataPreprocessor)

25/10/25: Pre-reading for designing and coding the StratifiedKFold and the train_test_split 

26/10/25: added .gitkeep so empty folders are tracked; finalised dataset folder structure and coded CSV load tests on Iris to verify paths and parsing.

27/10/25: Prototyped KNN v1 with vectorised squared-Euclidean + argpartition for neighbour lookup. Started the DataPreprocessor skeleton (impute mean/median, label encoder, standard scaler).

28/10/25: Committed KNN v1; fixed a bug so predictions return actual class labels (not index positions).Sanity-checked on Iris: first end-to-end pass locally.

29/10/25: Implemented and committed the data preprocessing pipeline: imputation, label encode/decode, and scaling with safe handling for constant features.Added the Iris test script: simple train/test split = scale on train only = predict = accuracy + sample predictions.

30/10/25: implemented StratifiedKFold, train_test_split, cross_validate, and ARFF support. also made sure preprocessing happens inside each fold so scaling is fitted only on training folds and data leakage is avoided.

31/10/25: merged the quick KNN checks into a cleaner evaluation script and fixed issues in the test flow so Iris could be run repeatedly with the same preprocessing/evaluation path.

03/11/25: cleaned up the early KNN experiment flow and tightened the dataset-loading path handling so the repo structure was easier to work with before starting the next algorithm.

05/11/25: did design work for the decision tree implementation: split scoring, stopping rules, recursion layout, and how to support both Gini and entropy cleanly.

07/11/25: started building the decision tree internals and mapped how categorical-heavy benchmark datasets would be fed through the existing preprocessing and evaluation pipeline.

10/11/25: iterated on the decision tree node/split logic and checked how to keep the API aligned with the earlier KNN work so both models could use the same evaluation helpers.

12/11/25: reviewed edge cases around shallow trees, empty branches, and impurity calculations. also noted where the testing scripts would need updating once the tree was in place.

14/11/25: added the first version of Diary.md and documented the work completed so far across the opening weeks of the project.

17/11/25: resumed the decision tree work with more focused testing on Iris and small subsets, mainly to verify that recursive fitting and prediction were behaving as expected.

19/11/25: compared Gini vs entropy behaviour and cleaned the experiment scripts so the later benchmarking stage would be able to report the two criteria clearly.

21/11/25: prepared the repo for model-to-model comparison work by tidying test scripts and making the benchmark output easier to inspect side by side.

24/11/25: set up the larger dataset evaluation direction for Bank Marketing and reviewed how one-hot encoding and larger sample counts would affect runtime and later hyperparameter tuning.

26/11/25: implemented the decision tree algorithm properly with both gini and entropy impurity measures and added the main test script. initial result was around 94% accuracy on the early benchmark run.

27/11/25: added a first direct decision tree vs KNN comparison and cleaned up the older test scripts so the repo had a clearer baseline comparison path.

28/11/25: prepared the larger-dataset run and checked the pipeline behaviour on a heavier benchmark case before moving into full hyperparameter search.

29/11/25: added a larger dataset run for the decision tree and saw roughly 91% accuracy there. also removed the old dedicated data loaders folder, consolidated the dataset-loading logic, and implemented the decision tree grid-search pipeline with CSV export.

30/11/25: reduced the decision tree tuning grid because the original search space was too expensive per fold. also started the decision tree visualisation work, although some plotting issues still needed fixing.

01/12/25: extended the evaluation pipeline with precision, recall, and F1. implemented the KNN grid search, updated the tests to print the fuller metric set, and added broader plotting/visualisation scripts for tuned runs.

03/12/25: added confusion matrices for KNN and Decision Tree, improved the newer grid-search reporting around precision/recall/F1, and completed the first round of model-comparison visual outputs.

15/01/26: restarted the second major phase of the project by planning the SVM track and reviewing how the existing benchmarking framework could support a third algorithm without rewriting the whole evaluation stack.

17/01/26: mapped the binary-first SVM approach, including hinge-loss training, margin-based scoring, and how to keep the API consistent with the earlier KNN and Decision Tree implementations.

20/01/26: reviewed the evaluation code with SVM in mind and identified the need for cleaner handling of decision scores, imbalanced metrics, and stricter train/test separation on the Bitcoin data.

22/01/26: drafted the intended training loop structure for a linear SVM solver and planned how to benchmark it first on simple separable data and then on the Iris binary case.

24/01/26: revisited the dataset strategy and decided that Bitcoin would need a more careful methodology than the earlier random-split style, especially because of temporal leakage risk.

27/01/26: worked through the Bitcoin binary framing decisions, including collapsing labels into legit vs ransomware and thinking about what metadata should be preserved for later strict splitting.

29/01/26: planned the stricter Bitcoin evaluation path around year-based grouping and holdout handling so that the project could later report a more credible result than the easy split.

02/02/26: reviewed the preprocessing and evaluation pipeline again with the aim of supporting model scores, threshold tuning, and grouped cross-validation without breaking the earlier KNN/Decision Tree flows.

04/02/26: sketched the imbalance-aware evaluation direction, especially around precision-recall trade-offs, class weighting, and why accuracy should not be the headline metric on Bitcoin.

06/02/26: designed the multiclass SVM direction as a One-vs-Rest wrapper so the custom linear SVM could still be used on datasets like Iris while remaining binary at the solver level.

10/02/26: prepared the repo structure for the reporting phase by separating experiment runners from reporting scripts and deciding which result files should become the authoritative outputs.

12/02/26: reviewed how the final dissertation evidence would be assembled and planned a later set of scripts for summary tables, figure generation, and source-of-truth documentation.

17/02/26: went back through the benchmark loaders and final evaluation design so Breast Cancer and Bank Marketing could join the later all-model comparison in a consistent way.

19/02/26: tightened the intended comparison workflow between custom implementations and sklearn baselines so final evaluation would not only show scores but also implementation parity.

24/02/26: prepared the final SVM implementation push by locking down the interface, runtime expectations, and the first set of solver hyperparameters to explore.

26/02/26: completed the last pre-implementation review for the SVM phase and finalised the plan to add threshold-aware Bitcoin experiments after the initial linear model was stable.

03/03/26: resumed hands-on coding work around the SVM branch and refreshed the benchmarking plan for binary Iris, Bank Marketing, and later Bitcoin.

05/03/26: checked how the new SVM outputs would feed into the existing CSV result format so the later summary scripts could rank all three model families consistently.

10/03/26: finalised the initial SVM implementation/testing checklist and prepared the branch for the main burst of model development.

12/03/26: added the initial binary linear SVM scaffold and then implemented the first working hinge-loss mini-batch training loop. also added synthetic separable-data testing, objective monitoring, early stopping, class_weight/sample_weight support, the binary Iris evaluation script, the Pegasos-style update schedule, the projection step, and the first stable working linear SVM version with 5-fold binary Iris evaluation.

17/03/26: validated the new SVM path more broadly and checked how well it fit into the existing benchmark/test setup compared with the earlier KNN and Decision Tree implementations.

20/03/26: refined the benchmarking direction for SVM so later scripts could report macro metrics, PR-AUC style information, and stronger comparison outputs rather than accuracy alone.

24/03/26: prepared the repo for the larger SVM experiment expansion by cleaning how the custom and sklearn baselines would be constructed and compared.

27/03/26: reviewed the Bitcoin workflow again and aligned it with the newer SVM scoring approach so threshold tuning and stricter evaluation could be added next.

02/04/26: added the more robust from-scratch Linear SVM and One-vs-Rest flow, expanded the grid search/benchmarking pipeline, and updated the visualisation path to use richer multi-metric CV outputs including macro-style and PR-focused reporting.

05/04/26: added the full experiment pipeline, best-model summary generation, final evaluation script, and cleaner benchmark output structure so all major datasets could be compared through one reproducible flow.

06/04/26: implemented threshold tuning, class weighting support in the broader experiment pipeline, and stronger imbalanced-dataset evaluation so Bitcoin results could be interpreted more responsibly.

08/04/26: built out the final prediction export direction and the comparison layer needed to analyse custom vs sklearn predictions beyond simple aggregate scores.

10/04/26: added the paired final-prediction analysis work, including disagreement summaries, supporting tables, and the groundwork for statistical comparison/reporting outputs.

13/04/26: tightened the evaluation scripts and added the strict Bitcoin grid-search workflow so tuning could be done on training-period data only rather than through the older easier setup.

15/04/26: reorganised the experiment/reporting scripts, added the newer Bitcoin reporting outputs, and consolidated the repo around the final strict-holdout reporting flow with summary files, figure scripts, and evidence-pack generation.
