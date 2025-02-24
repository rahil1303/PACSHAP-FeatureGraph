# 🚀 PACSHAP-FeatureGraph

This repository presents an advanced, privacy-preserving approach to feature selection for Machine Learning (ML) models, addressing critical shortcomings of existing methods proposed in the research paper:

> 📌 **[\"Data Minimization for GDPR Compliance in Machine Learning Models\"](https://doi.org/10.1016/j.cose.2022.102808) (Goldsteen et al., 2022)**

The authors of the cited paper propose data generalization at model inference to achieve GDPR-compliant data minimization.

## 🔎 Critique & Improvements:

We identified several critical weaknesses in the cited paper ([Goldsteen et al., 2022](https://doi.org/10.1016/j.cose.2022.102808)) that our project addresses:

### 🚩 Weakness 1: All Data Directly Fed Into ML Models

- **Issue:** Feeding all data directly into ML models poses significant privacy risks, including susceptibility to undetectable backdoors, increasingly prevalent in modern ML attacks.  
- **Our Counterpoint:** Our method proactively analyzes and selects only the necessary features, significantly reducing privacy risks by limiting unnecessary data exposure.

### 🚩 Weakness 2: Exclusive Handling of Numerical Features

- **Issue:** Their minimization and generalization techniques exclusively handle numerical features, neglecting categorical (string) data, which limits the approach’s applicability to comprehensive ML scenarios.  
- **Our Counterpoint:** We address both numerical and categorical features, enabling broader and more effective feature selection and minimization.

### 🚩 Weakness 3: Accuracy Loss with Differential Privacy

- **Issue:** Their differential privacy implementation often sacrifices accuracy, particularly noticeable during data generalization and minimization phases. Additionally, their implementation uses outdated libraries, requiring substantial rewriting.  
- **Our Counterpoint:** We adopt PAC Privacy, ensuring stronger theoretical privacy guarantees without significant accuracy degradation.

### 🚩 Weakness 4: Decision Tree Limitations

- **Issue:** Their reliance on decision trees limits their methodology’s ability to generalize complex feature interactions.  
- **Our Counterpoint:** We propose alternative methods involving Knowledge Graphs and SHAP values to offer improved, more accurate assessments of feature importance and interactions.


### 🚩 Weakness 5: Reliance on Decision Trees for Feature Generalization

- **Issue:** Their method heavily relies on decision trees, assuming they effectively capture model decision boundaries. However, decision trees only make axis-aligned splits, potentially missing subtle and complex feature interactions, especially in deep learning scenarios.  
- **Our Counterpoint:** Instead of decision trees, we employ a Knowledge Graph and SHAP-driven interpretability to capture complex and context-aware feature relationships accurately.

### 🚩 Weakness 6: User-Driven Dynamic Feature Reduction

- **Issue:** Their approach relies on users gradually disclosing data, making it manual and less practical for automated, large-scale real-world ML pipelines.  
- **Our Counterpoint:** Our feature selection method automates this process, proactively identifying and filtering unnecessary data before it reaches ML pipelines, enhancing scalability and reliability.

### 🚩 Weakness 7: Feature Contamination Across Multiple Models

- **Issue:** They assume minimizing data in a single model achieves compliance, neglecting cross-model data reuse (loan approvals, fraud detection, marketing), potentially causing privacy leakage.  
- **Our Counterpoint:** We propose a pre-check system to ensure that only genuinely necessary features enter multiple ML models, effectively preventing cross-model feature contamination.

### 🚩 Weakness 8: Lack of Security Assurance

- **Issue:** They assert that reducing data collection improves security but do not evaluate resistance against adversarial scenarios (e.g., feature inversion attacks).  
- **Our Counterpoint:** Our approach emphasizes proactive security by eliminating unnecessary data at the outset, significantly reducing potential adversarial misuse.

---

For an in-depth critique and detailed logical reasoning behind our approach, please refer to the **[`Logic_Behind`](./Logic_Behind)** folder in this repository.

We critique and substantially improve upon this by:

- **Introducing Knowledge Graphs (KG)** to accurately identify and validate feature relevance before data reaches ML pipelines.
- **Utilizing SHAP (SHapley Additive exPlanations)** values to achieve explainable, context-aware feature importance assessment.
- **Integrating PAC (Probably Approximately Correct) Privacy**, offering strong theoretical guarantees that selected features are both privacy-preserving and statistically sound.
- **Leveraging Autoencoders** to intelligently encode and reconstruct features, identifying critical feature dependencies and redundancies, thus further enhancing feature selection and ensuring minimal data exposure.

Unlike entropy-based or generalized data approaches, our methodology ensures that only genuinely necessary features are selected, significantly reducing the privacy risks associated with ML models. Autoencoders provide an additional layer of intelligent feature reduction, capturing complex relationships while maintaining model accuracy. This project demonstrates empirical improvements in privacy-accuracy trade-offs, providing a robust framework for real-world, large-scale applications in data protection and privacy-sensitive machine learning.

## 🗃️ Key Contributions:

- **Federated KG-driven feature relevance checks**, ensuring automated data minimization across multiple ML models.
- **SHAP-driven interpretability**, providing transparent and explainable feature selection.
- **PAC Privacy guarantees**, ensuring rigorous compliance with GDPR and privacy standards.
- **Autoencoder-based feature encoding**, intelligently identifying redundant and dependent features, thereby enhancing privacy-preserving feature selection.

---

For an in-depth critique and detailed logical reasoning behind our approach, please refer to the **[`Logic_Behind`](./Logic_Behind)** folder in this repository.


## 🔗 Usage:

The repository includes implementations, experiments, and visualizations (including 3D privacy-accuracy trade-off grids) that demonstrate the effectiveness of our proposed approach against existing state-of-the-art methodologies.

---

**📂 Repository Structure:**

- Case_Study: Detailed examinations and real-world scenarios illustrating our solution's practical effectiveness.
•	Logic_Experiment: Q&A formatted justifications for critical design decisions and explicit explanations on why our approach effectively addresses identified gaps.
•	Proposed_Method_Implementation: Contains clearly annotated code implementations and experiments demonstrating our approach and empirical validations against privacy attacks.
•	Theoretical_Foundations: Comprehensive theoretical and mathematical justifications for our selected methodologies, explicitly highlighting improvements over the original paper.
•	Archived_Notebooks This folder contains reference notebooks from the original research implementation associated with the paper "Data minimization for GDPR compliance in machine learning models" by Goldsteen et al. (2022).
•	modified_archived_notebooks: This folder contains notebooks adapted and modified from the original archived reference notebooks associated with the research paper titled "Data minimization for GDPR compliance in machine learning models" by Goldsteen et al. (2022). The original implementation and datasets became outdated and incompatible with current library versions and dependencies.



## 📚 References & Citations

- **Goldsteen, et al. (2022)**.  
  *"Data Minimization for GDPR Compliance in Machine Learning Models"*  
  IEEE Symposium on Security and Privacy (SP), 2022.

- **Mienye, Ibomoiye Domor, and Yanxia Sun (2023)**.  
  *"A Machine Learning Method with Hybrid Feature Selection for Improved Credit Card Fraud Detection"*  
  Applied Sciences 13, no. 12: 7254. [https://doi.org/10.3390/app13127254](https://doi.org/10.3390/app13127254)

- **Goldwasser, Shafi, Michael P. Kim, Vinod Vaikuntanathan, and Or Zamir (2022)**.  
  *"Planting Undetectable Backdoors in Machine Learning Models"*  
  IEEE Symposium on Foundations of Computer Science (FOCS), 2022.

- **Rubenstein, Paul, Peter Bartlett, and Thomas Steinke (2022)**.  
  *"PAC Privacy: Automatic Privacy Measurement and Control of Data Processing"*  
  Advances in Neural Information Processing Systems (NeurIPS), 2022.

- **Pulls, Tobias, and Mike Perry (2020)**.  
  *"Raising the Bar: Improved Fingerprinting Attacks and Defenses for Video Streaming Traffic"*  
  USENIX Security Symposium, 2020.

- **Shokri, Reza, Marco Stronati, Congzheng Song, and Vitaly Shmatikov (2017)**.  
  *"Membership Inference Attacks against Machine Learning Models"*  
  IEEE Symposium on Security and Privacy (Oakland), 2017.


**✉️ Contact:**

For questions or collaboration, please contact:

- **Your Name:** [r.sharma4@student.vu.nl](mailto:r.sharma4@student.vu.nl)

---

🌟 **Thank you for exploring PACSHAP-FeatureGraph!** 🌟
