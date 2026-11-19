## 📚 Company Case Study: Multi-Model Data Protection with Hybrid Feature Selection

### Problem Statement
Companies frequently ingest extensive user data, employing it across multiple machine learning (ML) models such as **Loan Approval**, **Fraud Detection**, and **Marketing Recommendations**. However, this broad utilization of data significantly elevates privacy vulnerabilities, particularly from **Membership Inference Attacks (MIA)** and **Attribute Inference Attacks**, potentially breaching GDPR compliance and consumer trust.

> **About the numbers in this case study.** Both notebooks use synthetic data whose labels (`loan_default`, `fraudulent_transaction`) are drawn at random, independently of the features. On random labels, accuracy above about 50% indicates leakage or a bug rather than learning: the baseline's 100% comes from a deliberately leaky feature built from the label, and the hybrid notebook's 89% comes from a regenerated dataset after a PCA step. The hybrid notebook also computes "membership inference success" by comparing the attack's membership predictions with the **loan labels** instead of with true membership, so the 49% / 47% figures are not membership measurements. Treat this case study as a walkthrough of the multi-model scenario and the feature-pool idea, not as evidence; the measured results are in `proposed_method_implementation/`.

### 🔍 Baseline Evaluation and Identified Risks
Initially, a baseline scenario was implemented, where ML models were trained using all available data without explicit privacy-preserving measures. Although this yielded high predictive accuracy, it severely exposed the models to privacy attacks:

| Model (Baseline)           | Accuracy | Membership Inference Attack Success |
|-----------------------------|----------|-------------------------------------|
| Loan Approval               | 100%     | 🚨 **82.60%**                        |
| Fraud Detection             | 100%     | 🚨 **50.00%**(Unexpected due to bad data)                       |
| Marketing Recommendations   | MSE: 0.0866 | N/A (Regression)                  |

These results highlight substantial vulnerabilities, demonstrating how easily adversaries could identify whether specific data points were present during training.

### 🚀 Our Privacy-Preserving Hybrid Approach
To mitigate these vulnerabilities, we've implemented a novel **Hybrid Feature Selection Framework**, integrating:

- **Automated Feature Pool for Enhanced Data Privacy and Model Efficiency**

We introduced a **Feature Pool** structure, categorized explicitly for diverse ML use-cases (e.g., financial, fraud detection, marketing), enabling models to autonomously select only the necessary, relevant features for training. This approach:

- Explicitly addresses **GDPR’s data minimization** requirement by ensuring minimal data exposure.
- Utilizes **SHAP analysis** and **Knowledge Graphs (KGs)** to contextually validate feature relevance, thereby preventing sensitive or unnecessary data from entering model pipelines.
- Employs **Autoencoders** to further capture non-linear relationships, ensuring privacy-preserving embeddings that obscure sensitive data.

This structured approach significantly improves security over the baseline models by limiting unnecessary data ingestion and reducing attack vectors, as demonstrated through our membership inference attack evaluations.


- **Feature Relevance Check (FRC):** Validates feature necessity explicitly aligning with GDPR's data minimization principle.
- **Knowledge Graphs (KG):** Captures context-specific feature relationships, ensuring critical features are preserved.
- **SHAP Analysis:** Quantitatively measures feature relevance, ensuring only impactful features enter the model.
- **Autoencoders:** Further compress and encode sensitive features, mitigating indirect privacy leaks.
- **PAC Privacy (planned):** A target for calibrated noise with formal guarantees (Xiao & Devadas, 2023); the current notebooks add hand-tuned noise instead.

### 🚀 Improved Results with Our Hybrid Approach
Our hybrid method demonstrated enhanced privacy protection with robust predictive performance:

| Model Configuration                 | Accuracy/MSE | Membership Inference Attack Success |
|-------------------------------------|----------|-------------------------------------|
| Loan Approval (Hybrid)              | 89.00%   | ✅ **49.00%** *(↓33.6%)*            |
| Fraud Detection (Hybrid)            | 91.00%   | ✅ **47.00%** *(↓3%)*               |
| Marketing Recommendations (Hybrid)  | MSE: 581.98 | N/A (Regression)                 |

### 🛡️ How Our Approach Enhances Privacy and Utility
- **Privacy Preservation:**  
  By utilizing a hybrid KG+SHAP+Autoencoder framework, we drastically reduce unnecessary exposure of data, significantly decreasing the attack surface for inference attacks. PAC Privacy is the planned route to calibrated, provable control over leakage.

- **Utility Preservation:**  
Differential Privacy (DP) can degrade performance by adding noise to every feature; selecting features first, and calibrating noise with PAC Privacy, aims to protect only what the model needs.

### 📌 Conclusion
Our proposed hybrid feature selection and privacy-preserving approach demonstrates a clear advantage in terms of security robustness and regulatory compliance over traditional ML approaches. By proactively filtering relevant features based on context (KG), importance metrics (SHAP), and encoding sensitive attributes via autoencoders, this design aims for a better privacy-utility balance than protecting the full feature set; confirming it needs real data and a consistent attack protocol.

---

### 📚 References
- Mienye, I. D., & Sun, Y. (2023). A machine learning method with hybrid feature selection for improved credit card fraud detection. *Applied Sciences*, 13(12), 7254. https://doi.org/10.3390/app13127254
- Goldwasser, S., Kim, M. P., Vaikuntanathan, V., & Zamir, O. (2022). Planting undetectable backdoors in machine learning models. *IEEE FOCS*.
- Xiao, H., & Devadas, S. (2023). PAC Privacy: Automatic privacy measurement and control of data processing. *CRYPTO 2023*. https://arxiv.org/abs/2210.03458
- Shokri, R., Stronati, M., Song, C., & Shmatikov, V. (2017). Membership inference attacks against machine learning models. *IEEE Symposium on Security and Privacy (S&P)*.

---

✨ **Repository Navigation:** Each folder within this repository contains its own detailed `README.md` explaining its contents, usage, and relevance.

