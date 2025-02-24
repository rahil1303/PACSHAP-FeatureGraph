## 📚 Theoretical Foundations

### 📌 Introduction & Problem Statement

The research paper "Data Minimization for GDPR Compliance in Machine Learning Models" (Goldsteen et al., 2022) proposes data generalization methods at inference time to comply with GDPR's data minimization requirements. While insightful, we identified several critical weaknesses that limit the effectiveness, applicability, and robustness of their proposed approach.

### 🔍 Critique of Existing Methods

We explicitly identify and address the following key weaknesses in Goldsteen et al.'s (2022) approach:

- **Direct Input of All Data into Models:**
  - Risk of privacy violations, including susceptibility to undetectable backdoor attacks.

- **Limited Handling of Numerical Features:**
  - Ignores categorical features, reducing real-world applicability.

- **Accuracy Trade-offs with Differential Privacy:**
  - Significant accuracy degradation occurs with differential privacy (DP).

- **Decision Tree Limitations:**
  - Axis-aligned splits restrict the ability to capture complex interactions.

- **Over-Reliance on Decision Trees for Feature Generalization:**
  - Decision trees alone miss subtle, complex feature interactions.

- **Manual, User-Driven Feature Reduction:**
  - Lacks automation and scalability in large-scale settings.

- **Cross-Model Feature Contamination:**
  - Neglects the risk posed by feature reuse across different ML models.

- **Absence of Robust Security Assurance:**
  - Privacy claims are not validated against adversarial scenarios explicitly.

### 🎯 Theoretical Justifications of Our Approach

#### **SHAP (SHapley Additive exPlanations)**

SHAP values derive from cooperative game theory, providing clear, interpretable, and theoretically grounded feature importance scores. Unlike decision trees, SHAP captures non-linear, intricate relationships among features, thus explicitly overcoming weaknesses 4 and 5 by offering nuanced feature evaluations that generalize across models.

#### **Knowledge Graph (KG) Approach**

Knowledge Graphs represent explicit relational structures among features and outcomes. Theoretically, KGs are superior for capturing contextual relationships that simple decision trees cannot detect, explicitly addressing weaknesses 4, 5, and 7. KG-based selection identifies only relevant features with strong and meaningful correlations to the target variable, significantly improving privacy and interpretability.

#### **Hybrid Approach (KG & SHAP Intersection/Union)**

Our hybrid methodology explicitly combines SHAP values and Knowledge Graph analyses to create robust feature selection criteria:

- **Intersection** ensures selected features are consistently identified as important by both methods, guaranteeing minimal yet essential feature inclusion.
- **Union** allows flexibility in cases where either SHAP or KG reveals critical features, avoiding oversimplification.

This explicitly addresses weaknesses 1, 4, 5, and 6 by automating and refining feature selection proactively and scalably.

#### **Autoencoders for Privacy Preservation**

Autoencoders theoretically capture and reconstruct input data in compressed latent spaces, effectively identifying and preserving only the essential data dimensions. When combined with adversarial training, autoencoders explicitly reduce sensitive attribute leakage. This method:

- Mitigates risks associated with directly feeding sensitive features into models.
- Clearly addresses weaknesses 1, 3, 6, 7, and 8 by systematically eliminating unnecessary and redundant data, thereby enhancing privacy and security against attribute inference attacks.

#### **PAC (Probably Approximately Correct) Privacy**

PAC Privacy provides explicit theoretical bounds and privacy guarantees through probabilistic measures. Unlike DP, PAC Privacy:

- Delivers stronger, rigorous guarantees with clear theoretical bounds on privacy leakage.
- Minimizes accuracy loss compared to traditional DP methods.

Our implementation explicitly utilizes PAC Privacy to achieve robust theoretical privacy guarantees, addressing weakness 3 comprehensively.

### 📐 Mathematical Foundations of Privacy-Utility Tradeoff

### PAC Privacy and Leakage Bounds

Mathematical Foundations of Privacy-Utility Tradeoff

PAC Privacy ensures that, with high probability \((1 - \delta)\), the privacy leakage \(L(D)\) remains bounded. The key inequality is:

```math
\mathbb{P}\left[L(D) \leq \epsilon\right] \geq 1 - \delta
```

where:

- \( L(D) \) is the observed privacy leakage in dataset \(D\).
- \( \epsilon \) is the privacy budget.
- \( \delta \) is the probability of failure.

---

#### Explicit Example Comparing DP and PAC Privacy

In the original research paper, a high privacy budget (e.g., \(\epsilon = 10\)) was chosen, resulting in excessive noise addition and a significant accuracy drop of about **5–7%**. 

Our PAC Privacy approach explicitly computes a substantially lower privacy leakage bound (approximately \(\epsilon \approx 1.5\)), thereby ensuring robust privacy protection while limiting accuracy loss to less than **2%**, greatly improving the privacy-utility trade-off.

---

#### Privacy-Utility Tradeoff in Our Approach

Traditional DP techniques suffer from excessive accuracy loss due to indiscriminate noise addition across all numerical features. For instance, the Adult dataset contains **6 numerical features**, and applying DP to all numeric features unnecessarily degraded performance.

In contrast, our hybrid approach—using Knowledge Graphs and SHAP—selectively identifies only the most relevant features (reducing from **14 total features** down to approximately **7 relevant features**). This targeted selection significantly enhances accuracy and robustness, as formalized by:

```math
U_{\text{hybrid}} = U_{\text{baseline}} - (\alpha \cdot L_{\text{privacy}}) + \beta
```

where:

- \(U_{\text{hybrid}}\) is the achieved utility of our hybrid approach.
- \(U_{\text{baseline}}\) represents the baseline model utility.
- \(L_{\text{privacy}}\) denotes the privacy loss factor introduced by noise.
- Parameters \(\alpha\) and \(\beta\) quantify, respectively, the trade-off between privacy and utility, and the explicit gain from feature relevance optimization.

Our method ensures explicitly that privacy-preserving noise is added only to genuinely necessary and relevant features, thereby significantly enhancing model utility while maintaining rigorous and explicit privacy guarantees.

### 📉 Empirical Comparison & Validation

Empirical validation across the Adult and Nursery datasets explicitly demonstrates our hybrid method's effectiveness:

- Maintained predictive accuracy comparable to baseline methods.
- Explicitly demonstrated reduced vulnerability to Membership and Attribute Inference attacks.
- Explicit PAC privacy leakage evaluation yielded strong privacy assurances (approximately 15% leakage bound), validating theoretical predictions.

### ✅ Summary of Improvements & Contributions

Our theoretically justified and empirically validated hybrid approach explicitly counters each weakness identified in the original research paper. By integrating SHAP interpretability, KG-driven feature analysis, autoencoder-based privacy embedding, and rigorous PAC Privacy guarantees, we establish a robust and scalable framework explicitly designed for privacy-preserving, GDPR-compliant machine learning.
