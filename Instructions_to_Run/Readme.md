# PACSHAP-FeatureGraph 🚀  
A privacy-first approach to feature selection and inference attack mitigation. Instructions, althouhg the entire project is quite self explanatory but just in case attaching some insturctions below. Thank you! 

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
cd Case_Study/Baseline_Model
jupyter notebook Base_Line_Model_Company_Case_Study.ipynb
```

### 4️⃣ **Run the Hybrid Privacy Approach**  
For the improved model with Knowledge Graph, SHAP, Autoencoders, and PAC Privacy, execute:  
```bash
cd Case_Study/Hybrid_Proposed_Approach
jupyter notebook Hybrid_Proposed_Approach_Case_Study_KG_SHAP.ipynb
```

### 5️⃣ **Running Privacy Attack Evaluations**  
To compare inference attack vulnerabilities, execute:  
```bash
cd Privacy_Evaluation
jupyter notebook hybrid_privacy_inference_analysis.ipynb
```

### 6️⃣ **Feature Privacy Attacks on Different Datasets**  
To analyze and compare privacy attacks on the **Adult & Nursery datasets**, run:  
```bash
cd Feature_Attack_Analysis
jupyter notebook hybrid_feature_privacy_attacks_adult.ipynb
jupyter notebook hybrid_feature_privacy_attacks_nursery.ipynb
```

---

## 🛠️ **Additional Notes**  
✅ Ensure **Jupyter Notebook** is installed:  
```bash
pip install notebook
```  
✅ Adjust dataset paths if needed for external data sources.  

### 📌 **Feature Pooling Automation**  
The **Feature Pooling** mechanism ensures that ML models dynamically select only relevant features, reducing privacy risks and improving efficiency.  
This removes unnecessary feature selection overhead while aligning with **GDPR compliance**.

---

## 📢 **Need Help?**  
If you encounter any issues or need further clarification, feel free to **open an issue** in the repository.  
---
