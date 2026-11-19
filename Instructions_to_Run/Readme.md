# PACSHAP-FeatureGraph 🚀  
A privacy-first approach to feature selection and inference attack mitigation. The notebooks are largely self-explanatory, but here are the steps to run them.

## 🚀 How to Run the Project  

### 1️⃣ **Clone the Repository**  
First, clone the repository and navigate to the project folder:  
```bash
git clone https://github.com/rahil1303/PACSHAP-FeatureGraph.git
cd PACSHAP-FeatureGraph
```

### 2️⃣ **Install Dependencies**  
Ensure all required dependencies are installed using:  
```bash
pip install -r requirements.txt
```

### 3️⃣ **Run the Baseline Model**  
Navigate to the Baseline Case Study and execute the Jupyter Notebook:  
```bash
jupyter notebook Case_Study/Base_Line_Model_Company_Case_Study.ipynb
```

### 4️⃣ **Run the Hybrid Privacy Approach**  
For the multi-model case study with the feature pool, SHAP and autoencoders, execute:  
```bash
jupyter notebook Case_Study/Hybrid_Proposed_Approach_Case_Study_KG_SHAP.ipynb
```

### 5️⃣ **Running Privacy Attack Evaluations**  
To compare inference attack vulnerabilities, execute:  
```bash
jupyter notebook Logic_Experiment/hybrid_privacy_inference_analysis.ipynb   # illustrative only, see its banner
```

### 6️⃣ **Feature Privacy Attacks on Different Datasets**  
To analyze and compare privacy attacks on the **Adult & Nursery datasets**, run:  
```bash
jupyter notebook proposed_method_implementation/hybrid_feature_privacy_attacks_adult.ipynb
jupyter notebook proposed_method_implementation/hybrid_feature_privacy_attacks_nursery.ipynb
```

---

## 🛠️ **Additional Notes**  
✅ Ensure **Jupyter Notebook** is installed:  
```bash
pip install notebook
```  
✅ The Adult and Nursery notebooks download their data from the UCI Machine Learning Repository, so you need an internet connection.
✅ The notebooks were developed in Google Colab (Python 3.11); the `!pip install` cells inside them install `ai-privacy-toolkit`, `adversarial-robustness-toolbox`, `xgboost` and `shap` if needed.  

### 📌 **Feature Pooling Automation**  
The **Feature Pooling** mechanism ensures that ML models dynamically select only relevant features, reducing privacy risks and improving efficiency.  
It is demonstrated on synthetic data in `Case_Study/`.

---

## 📢 **Need Help?**  
If you encounter any issues or need further clarification, feel free to **open an issue** in the repository.  
---
