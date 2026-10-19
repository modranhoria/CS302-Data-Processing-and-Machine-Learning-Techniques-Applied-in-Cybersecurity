# Lab 1 — Phishing URL Detection with SVM and KNN

This practical lab is designed for **Google Colab**. Students inspect a real cybersecurity dataset, establish a rule-based baseline, train SVM and KNN classifiers, compare prediction metrics, review classification errors, and optionally evaluate generalization to unseen domains.

## Learning objectives

By completing the notebook, students should be able to:

- Work with a real phishing URL dataset and inspect its features and target labels.
- Explain the difference between a transparent rule-based baseline and supervised classifiers.
- Train and evaluate an SVM with an RBF kernel and a KNN classifier.
- Explain why scaling matters for distance-based and kernel-based methods.
- Compare accuracy, phishing precision, phishing recall, phishing F1-score, false-positive rate, and confusion matrices.
- Discuss false positives, false negatives, data limitations, and the consequences of using a prediction to trigger security actions.
- Compare a random train/test split with a group split that keeps domains separate.

## Dataset

The lab uses the **PhiUSIIL Phishing URL Dataset**, hosted by the UCI Machine Learning Repository (dataset ID 967). The UCI dataset page reports **235,795 examples**, **54 features**, and **two classes**. The dataset includes features derived from URLs and their associated web pages.

- **Official dataset page:** https://archive.ics.uci.edu/dataset/967/phiusil-phishing-url-dataset
- **Original target labels:** `1 = legitimate`, `0 = phishing`
- **Lab target labels:** `is_phishing = 1` means phishing; `is_phishing = 0` means legitimate
- **Dataset license:** CC BY 4.0, as stated on the UCI dataset page

Please consult the original dataset page for dataset details and attribution requirements.

## Files

- `Lab1_SupervisedLearning.ipynb` — the student notebook, with explanations and code output text.
- `README.md` — this guide.

## Running the notebook in Google Colab

### Option A — Upload directly

1. Open Google Colab: https://colab.research.google.com/
2. Select **File → Upload notebook** and upload `Lab1_SupervisedLearning.ipynb`.
3. Run the cells from top to bottom.
4. The setup cell installs `ucimlrepo`; an internet connection is required to download the dataset from UCI.

### Option B — Open from a GitHub repository

1. Upload `Lab1_SupervisedLearning.ipynb` to a GitHub repository.
2. Replace `USERNAME` and `REPOSITORY` in the URL below with your GitHub account and repository name:
   `https://colab.research.google.com/github/USERNAME/REPOSITORY/blob/main/Lab1_SupervisedLearning.ipynb`
3. Open the URL in a browser and run the notebook cells from top to bottom.

If you restart the Colab runtime, run the cells again from the beginning so that the package, dataset, and models are initialized correctly.

## Dataset size, sampling, and features

The full dataset contains approximately 235,000 rows. To keep SVM and KNN experiments responsive in an introductory lab, the notebook uses a reproducible, stratified sample of at most **6,000 rows**. The fixed random seed makes the sampling and random train/test split repeatable under the same data and library conditions. Stratification helps preserve the class proportions, but it does not guarantee that the sample perfectly represents current internet traffic.

The models use a subset of numeric features that describe URL structure, including URL length, domain length, subdomain count, HTTPS usage, obfuscation indicators, and counts or ratios of selected characters. The raw `URL`, `Domain`, and `Title` columns are not used as model inputs, and raw URLs are not displayed. The `Domain` column is retained separately only for the optional unseen-domain experiment, where it acts as a grouping variable—not as a prediction feature.

SVM and KNN are trained in pipelines with `StandardScaler`. The scaler is fitted on training data only and then applied to the test data, helping prevent preprocessing leakage.

## Interpreting the metrics

- **Accuracy:** proportion of all test examples classified correctly.
- **Phishing precision:** among URLs predicted as phishing, proportion labelled as phishing.
- **Phishing recall:** among all phishing URLs in the test set, proportion detected by the model.
- **Phishing F1-score:** harmonic mean of phishing precision and recall.
- **False-positive rate:** proportion of legitimate URLs incorrectly classified as phishing.
- **Confusion matrix:** counts of true negatives, false positives, false negatives, and true positives.

False negatives can allow phishing URLs to pass through a defensive control. False positives can generate unnecessary alerts, consume analyst time, or disrupt legitimate activity. The relative cost depends on the operational context.

## Optional extension: evaluation on unseen domains

A random split may place URLs from the same domain in both training and test sets. If those URLs share similar patterns, the test result may overstate performance on domains the model has never seen. The optional group-based split uses `Domain` to separate domains between the training and test sets without supplying that field to the classifier.

This is not a complete temporal evaluation. It does not test how performance changes as campaigns and attacker techniques evolve over time, and the group split can produce a different class distribution or sample size from the random split.

## Security and data-handling notes

- **Do not manually open dataset URLs.** The lab is designed to inspect numeric features without visiting the destination websites.
- Treat model outputs as signals for further investigation, not as conclusive proof that a URL is malicious or safe.
- The notebook's Zero Trust example is a teaching simulation only. It does not connect to security tooling or execute actions such as blocking a domain, quarantining email, or disabling an account.
- High-impact responses should be governed by suitable authorization, human review, logging, and other controls—not by a single model prediction.

## Limitations

PhiUSIIL is an academic benchmark, not a guarantee of performance on current traffic. Its labels and features reflect the dataset's collection and construction process. This notebook uses only a subset of the available features and samples at most 6,000 rows. URL-structure features alone cannot capture every relevant signal, such as current domain reputation, redirect behavior, email context, page content, or future attacker adaptations.

A random split does not simulate temporal change, while the unseen-domain split is only one additional generalization check. Results may vary with the sampled data, software environment, split, preprocessing, and model settings. Performance on this lab dataset does not guarantee detection of new phishing campaigns in deployment.

## Activities and student deliverables

Students should submit:

1. **Hyperparameter experiments:** compare linear and RBF kernels with `C = 0.1`, `1`, and `10`, and KNN with `k = 3`, `7`, and `11`.
2. **A metrics comparison** for the rule-based baseline, SVM, and KNN. Include phishing precision, phishing recall, phishing F1-score, and false-positive rate; accuracy and confusion matrices are also reported by the notebook.
3. **Error analysis:** identify examples of false positives and false negatives from the numeric feature tables, and explain what additional evidence an SOC analyst should request.
4. **A brief discussion:** explain why a phishing prediction should not, by itself, trigger a high-impact response such as blocking a domain.
