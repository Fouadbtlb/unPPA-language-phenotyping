# Mapping diagnostic uncertainty in unclassified PPA

Code for: Mahut Dubos A, et al. *Mapping diagnostic uncertainty in unclassified primary progressive aphasia through probabilistic language phenotyping.*

## Contents
- `python_classification/`: supervised classification (notebook `PPA_supervised_classification.ipynb`):
  1. imputation quality check on the full cohort (exploratory only);
  2. train/test split (test: 5 lvPPA, 5 nfvPPA, 5 svPPA, 24 controls; seed 42);
  3. imputation (iterative chained equations within groups of related subtests) and residualization on age, sex and education, fitted on the training set only;
  4. feature selection by sequential backward floating selection in a nested cross-validation (20 stratified outer folds, leave-one-out inner loop), training set only; features kept if selected in at least 10 of 20 folds;
  5. one-vs-rest L1 logistic regression (liblinear, C = 0.2, balanced class weights): leave-one-out on the training set, then test-set evaluation;
  6. class probabilities for unclassified PPA (unPPA).
- `R_pca_group_comparisons/`: PCA and group comparisons (R).

## Data
Patient data are not shared. Anonymized data may be made available on reasonable request, subject to a data-sharing agreement with Lille and Toulouse University Hospitals. The notebook expects the data file as `data.xlsx` in `python_classification/` (never committed).

## Running the Python part
    cd python_classification
    pip install -r requirements.txt
    jupyter notebook PPA_supervised_classification.ipynb

Use the versions in `requirements.txt`: scikit-learn 1.8 and later no longer support the liblinear solver for more than two classes. The nested cross-validation (section 3) takes several hours; the selection counts from the published run are stored in the cell "Stockage des variables sélectionnées", so sections 4 and 5 can be run without it.
