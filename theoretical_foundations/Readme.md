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

### 🎯 Why Each Component Is There

#### **SHAP (SHapley Additive exPlanations)**

SHAP values come from cooperative game theory (Lundberg & Lee, 2017): each feature's contribution to a prediction is its average marginal effect across all coalitions of features. Unlike the axis-aligned splits of a single decision tree, SHAP explains the actual model in use, including non-linear interactions, which addresses weaknesses 4 and 5. One caution: SHAP measures what a model *uses*, not what it *needs*; correlated features can share or swap importance, so SHAP rankings should be paired with a check that removing a feature really does not hurt accuracy.

#### **Feature Relationship Graph ("Knowledge Graph")**

In the current notebooks the graph links a feature to the target when their Pearson correlation exceeds a threshold. That makes relationships visible and inspectable, but it is effectively a correlation filter. Its value as a *knowledge* graph would come from adding information the data does not contain: which features are legally sensitive, which purposes each model serves, and which features are proxies for one another. That is the direction needed to address weaknesses 4, 5 and 7 fully.

#### **Hybrid Approach (Intersection / Union)**

- **Intersection** keeps only features that both methods flag: the most minimal set, at some risk of dropping a useful feature.
- **Union** keeps features flagged by either method: safer for accuracy, less minimal.

A practical rule for future work: choose the smallest set whose accuracy stays within a set tolerance of the baseline, and never select attributes declared sensitive.

#### **Autoencoders and Adversarial Training**

An autoencoder compresses the selected features into a lower-dimensional latent space. Trained against an adversary that tries to predict a sensitive attribute from the latent code (Edwards & Storkey, 2016), it is pushed to discard that information. Compression alone is not a privacy guarantee: an encoder is a deterministic function of the input, so whatever information survives in the latent space can still be inferred. The experiments confirm this, since a sensitive attribute removed from the input was still recovered from the embeddings through proxy features.

#### **PAC Privacy**

PAC Privacy (Xiao & Devadas, CRYPTO 2023) defines privacy as the difficulty, for any adversary, of recovering sensitive data within a chosen error and confidence. Its key practical feature is automation: by running the data-processing mechanism many times on subsamples and measuring the variance of its output, it computes how much noise must be added to reach a target guarantee, often far less noise than worst-case Differential Privacy requires.

**What the notebooks currently do is different and simpler.** They add hand-picked Gaussian noise (σ = 0.2 to 0.8) to the latent features and then report an *empirical leakage bound* for one trained attacker:

```math
\text{bound} = (\text{attack accuracy} - 0.5) + \sqrt{\frac{\ln(2/\delta)}{2n}}
```

This is a Hoeffding confidence bound: with probability $1-\delta$, that attacker's true advantage over a 50% guess is below the bound. It is a useful audit metric, but it does not cover other attackers, and the 0.5 reference should be replaced by the majority-class rate of the sensitive attribute (about 0.67 for both Adult `sex` and Nursery `social`). Implementing the actual PAC Privacy noise calibration is the main theoretical next step.

### 📐 Privacy-Utility Trade-off

The intuition behind the approach is that noise and generalisation spread over every feature cost accuracy everywhere, while removing features the task does not need costs little or nothing. Selecting first and protecting only what remains should therefore give a better privacy-utility trade-off than protecting the full feature set. On Adult this is borne out on the utility side: 14 features reduced to 6 kept accuracy at about 81%. Showing the privacy side convincingly requires the evaluation changes listed in the main README (one attack protocol, majority-class baselines, several seeds).

### 📉 Empirical Validation So Far

- Adult: accuracy maintained (81.65% → 81.89%) with 6 of 14 features; membership-inference success fell from 72.66% to 55.53% after the autoencoder, measured under two different attack protocols.
- Adult: removing `sex` alone did not prevent its recovery (about 78%), showing the need for proxy-aware selection.
- Nursery: attribute-inference results sit at or below the majority-class baseline for every configuration, because `social` is independent of the other features in this factorial dataset.

### ✅ Summary

The components are well motivated: SHAP for model-grounded relevance, a relationship graph for inspectability and purpose-level reasoning, adversarial autoencoders for removing sensitive signal, and PAC Privacy as a target for calibrated noise. The current implementation demonstrates the utility side of selective minimisation; the privacy side and the formal guarantees are the open work.

### 📚 References

- Goldsteen, A., et al. (2022). Data minimization for GDPR compliance in machine learning models. *AI and Ethics*, 2, 477–491. https://doi.org/10.1007/s43681-021-00095-8
- Lundberg, S. M., & Lee, S.-I. (2017). A unified approach to interpreting model predictions. *NeurIPS*.
- Edwards, H., & Storkey, A. (2016). Censoring representations with an adversary. *ICLR*.
- Xiao, H., & Devadas, S. (2023). PAC Privacy: Automatic privacy measurement and control of data processing. *CRYPTO 2023*. https://arxiv.org/abs/2210.03458
