# 🚀 PACSHAP-FeatureGraph

This repository presents an advanced, privacy-preserving approach to feature selection for Machine Learning (ML) models, addressing critical shortcomings of existing methods proposed in the research paper:

> 📌 **[\"Data Minimization for GDPR Compliance in Machine Learning Models\"](https://doi.org/10.1016/j.cose.2022.102808) (Goldsteen et al., 2022)**

The authors of the cited paper propose data generalization at model inference to achieve GDPR-compliant data minimization. We critique and substantially improve upon this by:

- **Introducing Knowledge Graphs (KG)** to accurately identify and validate feature relevance before data reaches ML pipelines.
- **Utilizing SHAP (SHapley Additive exPlanations)** values to achieve explainable, context-aware feature importance assessment.
- **Integrating PAC (Probably Approximately Correct) Privacy**, offering strong theoretical guarantees that selected features are both privacy-preserving and statistically sound.

Unlike entropy-based or generalized data approaches, our methodology ensures that only genuinely necessary features are selected, significantly reducing the privacy risks associated with ML models. This project demonstrates empirical improvements in privacy-accuracy trade-offs, providing a robust framework for real-world, large-scale applications in data protection and privacy-sensitive machine learning.

## 🗃️ Key Contributions:

- **Federated KG-driven feature relevance checks** ensuring automated data minimization across multiple ML models.
- **SHAP-driven interpretability** providing transparent and explainable feature selection.
- **PAC Privacy guarantees**, ensuring rigorous compliance with GDPR and privacy standards.

## 🔗 Usage:

The repository includes implementations, experiments, and visualizations (including 3D privacy-accuracy trade-off grids) that demonstrate the effectiveness of our proposed approach against existing state-of-the-art methodologies.

---

**📂 Repository Structure:**


**✉️ Contact:**

For questions or collaboration, please contact:

- **Your Name:** [your.email@example.com](mailto:r.sharma4@student.vu.nl)

---

🌟 **Thank you for exploring PACSHAP-FeatureGraph!** 🌟
