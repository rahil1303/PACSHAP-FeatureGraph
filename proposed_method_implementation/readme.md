## Proposed Method Implementation

This folder contains the implementation and evaluation of our proposed hybrid feature selection approach combining Knowledge Graphs (KG), SHAP values, and Autoencoders for enhanced privacy-preserving feature selection. The details are available in the notebook:

**Notebook:** `hybrid_feature_privacy_attacks_adult.ipynb`

### Methodology Overview:

- **Data Preparation:** Loaded and preprocessed the Adult dataset.
- **Knowledge Graph (KG):** Constructed to identify relationships between features and the target variable (income).
- **SHAP Analysis:** Computed SHAP feature importances to objectively assess feature relevance.
- **Autoencoder Integration:** Applied autoencoders to capture non-linear feature interactions and further refine feature selection.
- **Hybrid Feature Selection:** Combined KG, SHAP, and Autoencoder results to select a minimal yet essential feature subset.
- **Privacy Evaluation:** Applied Membership Inference and Attribute Inference attacks to evaluate privacy risks.
- **Ethical and Privacy Compliance:** Removed sensitive attributes (such as sex) explicitly to demonstrate compliance with ethical standards, reduce discrimination risks, and enhance privacy.

### Key Results

| Model Configuration            | Accuracy | Membership Inference Attack Success | Attribute Inference Attack Success |
|--------------------------------|----------|-------------------------------------|------------------------------------|
| Baseline Model                 | 81.65%   | 72.88%                              | 84.19%                             |
| KG + SHAP Hybrid Approach      | 81.89%   | 72.79%                              | 72.86%                             |
| KG + SHAP + Autoencoder Hybrid | 80.93%   | 55.53%                              | -                                  |

### Key Observations:

- Integrating autoencoders with the hybrid approach effectively maintained accuracy and significantly reduced vulnerability to privacy attacks.
- The explicit removal of sensitive attributes further enhanced ethical compliance and privacy preservation, clearly demonstrating our method's effectiveness.

