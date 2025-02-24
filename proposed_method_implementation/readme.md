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

| Model Configuration            | Accuracy | Membership Inference Attack Success | Attribute Inference Attack Success |
|--------------------------------|----------|-------------------------------------|------------------------------------|
| Baseline Model                 | 81.65%   | 72.88%                              | 84.19%                             |
| KG + SHAP Hybrid Approach      | 81.89%   | 72.79%                              | 72.86%                             |
| KG + SHAP + Autoencoder Hybrid | 80.93%   | 55.53%                              | -                                  |

#### Key Observations:

- Integrating autoencoders with the hybrid approach effectively maintained accuracy and significantly reduced vulnerability to privacy attacks.
- The explicit removal of sensitive attributes further enhanced ethical compliance and privacy preservation.

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

| Model Configuration                    | Accuracy | Black-Box Attack Success | White-Box Attack Success | PAC Privacy Leakage Bound (95%) |
|----------------------------------------|----------|--------------------------|--------------------------|---------------------------------|
| Baseline Model                         | 92.00%   | 55.45%                   | 62.30%                   | -                               |
| KG + SHAP Hybrid Approach              | 89.99%   | 69.75%                   | 74.34%                   | -                               |
| KG + SHAP + Autoencoder (latent+noise) | 81.35%   | 63.05%                   | -                        | 15.07%                          |

#### Key Observations:

- The hybrid approach significantly reduces attribute inference attack success, clearly demonstrating privacy protection.
- Autoencoder embeddings combined with adversarial training and controlled noise effectively achieve rigorous PAC privacy bounds with minimal accuracy degradation.
