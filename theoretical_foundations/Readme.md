## 📚 Theoretical Foundations

### 📌 Introduction & Problem Statement

The research paper ["Data Minimization for GDPR Compliance in Machine Learning Models"](https://doi.org/10.1016/j.cose.2022.102808) (Goldsteen et al., 2022) proposes data generalization methods at inference time to comply with GDPR's data minimization requirements. While insightful, we identified several critical weaknesses limiting their approach’s effectiveness, applicability, and robustness.

## 🔍 Critique of Existing Methods

We explicitly address key weaknesses in Goldsteen et al.'s (2022) approach:

### ⚠️ Direct Input of All Data into Models
- Risk of privacy violations, including susceptibility to undetectable backdoor attacks.

### ⚠️ Limited Handling of Numerical Features
- Ignores categorical features, reducing real-world applicability.

### ⚠️ Accuracy Trade-offs with Differential Privacy
- Significant accuracy degradation occurs with differential privacy (DP).

### ⚠️ Decision Tree Limitations
- Axis-aligned splits restrict capturing complex interactions.

### ⚠️ Over-Reliance on Decision Trees for Feature Generalization
- Decision trees alone miss subtle, complex feature interactions.

### ⚠️ Manual, User-Driven Feature Reduction
- Lacks automation and scalability in large-scale settings.

### ⚠️ Cross-Model Feature Contamination
- Neglects the risk posed by feature reuse across different ML models.

### ⚠️ Absence of Robust Security Assurance
- Privacy claims not validated explicitly against adversarial scenarios.

## 🎯 Theoretical Justifications of Our Approach

### 📖 SHAP (SHapley Additive exPlanations)
SHAP values derive from cooperative game theory, providing interpretable, theoretically grounded feature importance scores. Unlike decision trees, SHAP captures non-linear relationships, explicitly overcoming weaknesses 4 and 5 by nuanced evaluations generalizing across models.

### 🌐 Knowledge Graph (KG) Approach
KGs explicitly represent relational structures among features and outcomes. They effectively address weaknesses 4, 5, and 7 by identifying relevant features with meaningful correlations to the target variable, improving privacy and interpretability.

### 🔄 Hybrid Approach (KG & SHAP Intersection/Union)
Our hybrid method combines SHAP and KG analyses:

- **Intersection** ensures only consistently important features are selected.
- **Union** adds flexibility for identifying critical features, explicitly addressing weaknesses 1, 4, 5, and 6.

### 🧩 Autoencoders for Privacy Preservation
Autoencoders capture and reconstruct data in compressed latent spaces, identifying essential data dimensions. Combined with adversarial training, autoencoders explicitly reduce sensitive attribute leakage, addressing weaknesses 1, 3, 6, 7, and 8 by systematically eliminating unnecessary data and enhancing privacy.

### 🔐 PAC (Probably Approximately Correct) Privacy
PAC Privacy explicitly provides theoretical privacy guarantees with clear probabilistic bounds, offering:

- Stronger guarantees with rigorous theoretical privacy leakage bounds.
- Minimizes accuracy loss compared to traditional DP methods, explicitly addressing weakness 3.

## 🧮 Mathematical Foundations of Privacy-Utility Tradeoff

### 📊 PAC Privacy and Leakage Bounds
PAC Privacy ensures privacy leakage \( L(D) \) remains bounded with high probability:

\[
\mathbb{P}[L(D) \leq \epsilon] \geq 1 - \delta
\]

- \(L(D)\): Privacy leakage observed.
- \(\epsilon\): Privacy budget.
- \(\delta\): Probability of failure.

### 🔖 Explicit Example Comparing DP and PAC Privacy
Goldsteen et al. use high \(\epsilon\) (e.g., \(\epsilon=10\)), resulting in 5-7% accuracy drops. Our PAC Privacy explicitly calculates bounds ensuring privacy while limiting accuracy loss (<2%), significantly improving this trade-off.

### 📈 Privacy-Utility Tradeoff in Our Approach
DP degrades accuracy by indiscriminate noise addition. Our hybrid approach (KG & SHAP) selects the most relevant features (e.g., from 14 down to ~7 relevant features), enhancing accuracy:

\[
U_{hybrid} = U_{baseline} - (\alpha \cdot L_{privacy}) + \beta
\]

- \(U_{hybrid}\): Hybrid approach utility.
- \(U_{baseline}\): Baseline model utility.
- \(L_{privacy}\): Privacy loss factor.
- \(\alpha, \beta\): Impact of removing irrelevant features.

Our method explicitly ensures privacy-preserving noise applies only to necessary features, significantly improving utility.

## 📉 Empirical Comparison & Validation

Empirical validation (Adult, Nursery datasets) explicitly shows our hybrid method:

- Maintained predictive accuracy comparable to baselines.
- Reduced vulnerability to Membership and Attribute Inference attacks.
- PAC privacy leakage evaluation yielded ~15% leakage bound, validating theoretical predictions.

## ✅ Summary of Improvements & Contributions

Our theoretically justified, empirically validated hybrid approach explicitly counters identified weaknesses. Integrating SHAP interpretability, KG-driven analysis, autoencoder privacy embedding, and rigorous PAC Privacy guarantees establishes a robust, scalable framework for privacy-preserving, GDPR-compliant machine learning.

---

For an in-depth critique and detailed logical reasoning behind our approach, please refer to the **[`Logic_Behind`](./Logic_Behind)** folder in this repository.

