# Logic Experiment: Hybrid Privacy-Preserving Method

This folder (**Logic_Experiment**) contains documentation, experimental evidence, and logical explanations supporting the effectiveness of our proposed **Hybrid Privacy-Preserving Method** compared to traditional approaches (**Differential Privacy** and **Anonymization**).

## Experimental Context

We explored logical privacy-preserving strategies to mitigate two critical types of inference attacks:

- **Membership inference attacks:** determining if a data record was part of a training dataset.
- **Attribute inference attacks:** inferring sensitive attributes from partially observed data.

### Evaluated Methods:

- **Differential Privacy (DP)**: Noise-based protection.
- **Anonymization**: Generalization-based protection.
- **Hybrid Method (Our Proposed Approach)**: Combines DP and Anonymization.

## Experimental Results

### 1. Privacy vs. Utility Comparison

This graph clearly illustrates how our **Hybrid Method** achieves the best balance between model accuracy and protection against inference attacks.

![Privacy-Utility Trade-off](image.png)

- **Hybrid Method** achieved the highest predictive accuracy (**88%**) and the lowest inference attack accuracy (**58%**), clearly outperforming DP and Anonymization methods individually.

### 2. ROC Curve Analysis

ROC curves provide a detailed view of each method's performance against inference attacks:

![ROC Curve for Inference Attacks](image.png)

- The **Hybrid Method** clearly offers the lowest ROC-AUC (**0.57**), indicating superior privacy protection compared to Differential Privacy (**0.73**) and Anonymization (**0.67**).

## Logical Insights & Implications

The logical advantage of the Hybrid Method stems from its ability to leverage strengths from both anonymization and differential privacy:

- **Anonymization** effectively reduces direct linkage to individual records but struggles with retaining high utility.
- **Differential Privacy** preserves utility better but risks higher inference vulnerability due to noise alone.

By combining generalization and strategic noise addition, the Hybrid Method logically delivers improved privacy without significant compromise on data utility.

## Reproducing Results

All experimental results and visualizations are documented within the Jupyter notebook (`hybrid_privacy_inference_analysis.ipynb`) in this folder, including code, extracted metrics, and detailed explanations.

---

For further inquiries or replication guidance, please review the notebook or reach out directly.
