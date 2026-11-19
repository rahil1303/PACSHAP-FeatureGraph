## Proposed Method Implementation

This folder contains implementations and evaluations of our proposed hybrid feature selection approach combining Knowledge Graphs (KG), SHAP values, and Autoencoders for enhanced privacy-preserving feature selection. Details for each dataset evaluation are provided below:

---

### 🗃️ Adult Dataset

**Notebook:** `hybrid_feature_privacy_attacks_adult.ipynb`

#### Methodology Overview:

- **Data Preparation:** Loaded and preprocessed the Adult dataset.
- **Knowledge Graph (KG):** Constructed to identify relationships between features and the target variable (income).
- **SHAP Analysis:** Computed SHAP feature importances to objectively assess feature relevance.
- **Autoencoder Integration:** Applied autoencoders to capture non-linear feature interactions and further refine feature selection.
- **Hybrid Feature Selection:** Combined KG, SHAP, and Autoencoder results to select a minimal yet essential feature subset.
- **Privacy Evaluation:** Applied Membership Inference and Attribute Inference attacks to evaluate privacy risks.
- **Ethical and Privacy Compliance:** Removed sensitive attributes (such as sex) explicitly to demonstrate compliance with ethical standards, reduce discrimination risks, and enhance privacy.

#### Key Results:

| Model Configuration | Features | Accuracy | Membership Inference Attack Success | Attribute Inference Attack Success (sex) |
|---|---|---|---|---|
| Baseline, all features | 14 | 81.65% ¹ | 72.88% ² | 84.19% ² |
| KG + SHAP hybrid selection | 6 | 81.89% | 72.66% | 72.86% |
| Hybrid selection without `sex` + autoencoder | 5 → 6 latent | 80.93% | 55.53% | n/a |
| Autoencoder + latent noise (σ = 0.3), `sex` removed | 13 → 6 latent | 73.02% | n/a | 77.54% |

¹ From the revived Goldsteen minimisation notebook. ² Reported in an earlier version of this README; not traceable to a saved notebook output.

#### Key Observations:

- Cutting the feature set from 14 to 6, and then compressing it with an autoencoder, kept accuracy around 81%.
- Removing `sex` did not stop an attacker from recovering it (about 78%), because other features act as proxies. Note that roughly two-thirds of records are male, so guessing the majority already scores about 67%.
- Membership-inference success dropped from 72.66% to 55.53%, but the two numbers come from different attack protocols (held-out vs. same data), so the size of the drop should be confirmed under one protocol. See "Another way to read these results" in the main README.

---

### 🗃️ Nursery Dataset

**Notebook:** `hybrid_feature_privacy_attacks_nursery.ipynb`

#### Methodology Overview:

- **Data Preparation:** Loaded and preprocessed the Nursery dataset from the UCI repository, encoding categorical features appropriately.
- **Knowledge Graph (KG):** Constructed to explicitly identify meaningful feature relationships with the target class.
- **SHAP Analysis:** Performed feature importance assessments using SHAP values to objectively determine significant features.
- **Autoencoder and Adversarial Training:** Integrated autoencoder-based dimensionality reduction with adversarial training to explicitly limit sensitive information leakage.
- **Hybrid Feature Selection:** Selected features using KG and SHAP results combined, refining the set further through autoencoder embeddings.
- **Privacy Evaluation:** Conducted comprehensive Black-Box and White-Box Attribute Inference attacks.
- **PAC Privacy Implementation:** Evaluated the theoretical privacy leakage bound explicitly.

#### Key Results:

| Model Configuration | Accuracy | Black-Box Attack Success | White-Box Attack Success | Empirical Leakage Bound (95%) |
|---|---|---|---|---|
| Baseline decision tree | 92.00% | 55.45% ³ | 60.82% ³ | n/a |
| KG + SHAP hybrid selection | 89.99% | 69.75% | 69.75% | n/a |
| Adversarial autoencoder + noise σ = 0.8 | 81.35% | 63.05% | n/a | 15.07% |
| Same, stronger adversary (weight 6.0) | 78.03% | 63.96% | n/a | 15.97% |
| Differential-privacy variant | 43.60% | 66.07% | n/a | 18.09% |

³ The baseline attack targeted column index 0, which after one-hot encoding is not `social`.

#### Key Observations:

- The hybrid selection kept accuracy close to the baseline (92.0% → 90.0%), and the autoencoder variants traded accuracy for compression (78–81%).
- Attribute inference did **not** go down after selection (55% → 70%): SHAP ranked `social` itself as important, so the sensitive attribute was selected as an input. Excluding declared sensitive attributes from selection fixes this.
- After the autoencoder, every configuration lands at 63–66% attack accuracy, which is at or below the 67% an attacker gets by always guessing "not problematic". Nursery is a full factorial dataset, so `social` is independent of the other features by construction; this dataset cannot show attribute-inference protection from features alone.
- The "leakage bound" is an empirical Hoeffding bound on one attacker's accuracy above 50%, not a formal PAC Privacy guarantee (Xiao & Devadas, 2023).
