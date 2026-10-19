# Lab 2 — Unsupervised Cybersecurity Analytics with K-Means and DBSCAN

This lab introduces unsupervised clustering for network intrusion analysis. Students prepare a small set of numeric network-connection features and compare **K-Means** and **DBSCAN**. The notebook uses the KDD Cup 1999 dataset through scikit-learn's `fetch_kddcup99` function.

## Learning objectives

By completing the notebook, students should be able to:
- Explain the difference between supervised classification and unsupervised clustering, particularly in the context of network intrusion detection.
- Apply and compare K-Means and DBSCAN to explore patterns in network-connection data and investigate potential differences between normal and attack traffic.
- Evaluate and interpret clustering results using the Adjusted Rand Index (ARI), the number of clusters identified, cluster composition, and the proportion of noise points detected by DBSCAN.
- Analyze the impact of DBSCAN's eps parameter on the number of clusters, the noise fraction, and agreement with the reference labels.
- Recognize the limitations of clustering-based intrusion detection.

## Dataset

The downloaded records are grouped into two reference labels for evaluation only:

- `0` — normal connection
- `1` — attack connection

The clustering algorithms do not receive these labels during fitting. They are used only after the clusters have been produced.

The notebook requests the `SA` subset with `percent10=True`, shuffles it reproducibly, and selects a balanced teaching sample of at most 1,250 normal and 1,250 attack records. It uses these eight numeric features:

- `duration`
- `src_bytes`
- `dst_bytes`
- `count`
- `srv_count`
- `serror_rate`
- `same_srv_rate`
- `dst_host_srv_count`

Magnitude/count fields are transformed using `log1p`, then the selected features are standardized. The notebook records the sample counts and model outputs at run time; actual results depend on the data and software environment and should not be assumed in advance.

**Sampling note:** the sample is intentionally balanced to make both groups visible in a short class. That changes the class proportions and is not representative of normal operational network traffic.

## Files

- `Lab2_UnsupervisedLearning.ipynb` — the student notebook, with explanations and code output text.
- `README.md` — this guide.

## Running the notebook in Google Colab

### Option A — Upload directly

1. Open Google Colab: https://colab.research.google.com/
2. Select **File → Upload notebook** and upload `Lab2_UnsupervisedLearning.ipynb`.
3. Run the cells from top to bottom.
4. Allow internet access for the first dataset download. The dataset is cached by scikit-learn after download.

### Option B — Open from a GitHub repository

1. Upload `Lab2_UnsupervisedLearning.ipynb` to a GitHub repository.
2. Replace `USERNAME` and `REPOSITORY` in the URL below with your GitHub account and repository name:
   `https://colab.research.google.com/github/USERNAME/REPOSITORY/blob/main/Lab2_UnsupervisedLearning.ipynb`
3. Open the URL in a browser and run the notebook cells from top to bottom.

If you restart the Colab runtime, run the cells again from the beginning so that the package, dataset, and models are initialized correctly.

## Lab workflow

1. Load and inspect the KDD Cup 1999 `SA` subset.
2. Convert the selected features to numeric values and build the binary reference labels.
3. Sample up to 1,250 records from each reference group.
4. Apply logarithmic transformations to selected non-negative fields and standardize the features.
5. Fit K-Means with `k=2` and evaluate it using Adjusted Rand Index (ARI) and a cluster-composition table.
6. Fit DBSCAN with `eps=2.5` and `min_samples=10`. Record the number of clusters (excluding noise), ARI, and cluster-composition table.
7. Inspect a two-dimensional PCA visualization. PCA is for visualization only; the models are fitted using all eight scaled features.
8. Evaluate using the hidden reference labels and the Silhouette score.
9. Performe the activities mentioned below.

## Evaluation guidance

Good submissions interpret the metrics and tables rather than merely copying printed numbers. Please note that cluster IDs are arbitrary, that the noise label `-1` in DBSCAN is not synonymous with an attack, and that a higher ARI alone does not necessarily make a configuration operationally preferable. Their conclusions should be supported by the actual results produced by their run.

## Limitations and responsible use

KDD Cup 1999 is a historical benchmark built from simulated traffic in a closed network environment. It does not represent all current protocols, applications, attacker behaviors, or normal network conditions. This lab also uses only eight features, collapses many attack categories into one reference group, and deliberately balances normal and attack records. A balanced sample can make attack patterns look more common than they would be in deployment.

The clustering outputs are exploratory signals, not verified intrusion decisions. In particular:

- a DBSCAN noise point is not necessarily malicious;
- a point inside a dense cluster is not necessarily benign;
- cluster labels cannot be interpreted directly as normal/attack without examining their composition;
- evaluation on one historical sample does not establish real-world effectiveness.

Do not deploy the resulting settings or block traffic based solely on the notebook's outputs. A real SOC workflow needs current representative data, independent validation, alert review, monitoring for changing traffic patterns, and a clear understanding of the costs of false positives and false negatives.

## Activities and student deliverables

Students should submit:

1. **K-Means**: ARI, Silhouette Score and its cluster-composition table.
2. **DBSCAN**: results for the default setting, including number of clusters, noise fraction, ARI, and the cluster-composition table.

