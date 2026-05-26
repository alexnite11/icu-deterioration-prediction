# Predicting ICU Patient Deterioration and Mapping Patient Trajectories
**Author:** Alex Nite  
**Dataset:** eICU Collaborative Research Database (PhysioNet)

---

##  Project Overview
This project develops a machine‑learning framework to predict ICU patient deterioration and hospital mortality using high‑dimensional time‑series clinical data. Using the eICU Collaborative Research Database, I engineered patient‑level features from vital signs, labs, medications, and severity scores, then applied both supervised and unsupervised learning to:

- Predict hospital mortality  
- Identify physiological components driving deterioration  
- Discover natural patient subgroups based on clinical trajectories  

The project demonstrates how machine learning can support early warning systems, risk stratification, and clinical decision‑making in high‑acuity environments.

---

## Key Objectives
- Predict ICU deterioration and mortality using time‑series physiological data  
- Engineer clinically meaningful features from raw ICU telemetry  
- Reduce dimensionality using PCA to uncover latent physiological structure  
- Train and compare multiple supervised ML models  
- Use clustering to identify patient trajectory subgroups  
- Evaluate model performance and interpretability  

---

## Dataset Overview
This project uses the **eICU Collaborative Research Database**, a multi‑center ICU dataset containing:

- Demographics  
- Vital signs (periodic + aperiodic)  
- Laboratory values  
- Medications  
- APACHE severity scores  
- Mechanical ventilation  
- Mortality outcomes  

The dataset includes both time‑series and event‑based clinical data from thousands of ICU stays.

---

##  Data Preprocessing
Key preprocessing steps included:

- Standardizing age values (e.g., “>89” → 90)  
- Imputing missing demographics and severity scores  
- Cleaning lab values and medication dosages  
- Forward‑fill + backward‑fill for time‑series vitals  
- Removing invalid entries  
- One‑hot encoding categorical variables  
- Standardizing all numerical features  

This produced a clean, merged patient‑level dataset ready for modeling.

---

## Feature Engineering
Engineered features included:

### Vital Signs (24‑hour summaries)
- Mean, min, max, std of HR, RR, SpO₂, BP, Temp  

### Laboratory Values
- Mean, min, max per patient  

### Medications
- Total dosage  
- Number of unique medications  

### Severity Scores
- APACHE  
- Acute physiology score  

### Diagnosis & Treatment
- Counts of major diagnoses  
- Treatment categories  

### Time‑Series Aggregations
- Rolling averages  
- Variability measures  
- Trajectory slopes  

Final feature set: **884 features → reduced via PCA**

---

## Dimensionality Reduction (PCA)
- PCA reduced **884 → 427 components** while retaining **95% variance**  
- PC1 captured overall physiological severity  
- PC4 captured respiratory function  
- PC2 & PC3 captured renal function, inflammation, acid‑base balance  
- PCA revealed meaningful latent physiological structure  

---

##  Supervised Learning Models
Models trained:

- Logistic Regression  
- Random Forest  
- LightGBM  
- XGBoost  
- K‑Nearest Neighbors  

### **Best Model: LightGBM**
| Metric | Score |
|--------|--------|
| Accuracy | 0.992 |
| Precision | 0.982 |
| Recall | 0.903 |
| F1‑Score | 0.941 |
| ROC‑AUC | **0.993** |

LightGBM provided the best balance of sensitivity, specificity, and interpretability.

---

## Model Interpretation
Using SHAP, the most influential PCA components were:

- **PC1** — global severity  
- **PC4** — respiratory function  
- **PC119** — trajectory complexity  

These components aligned with known clinical deterioration patterns.

---

##  Unsupervised Learning (Clustering)
Two clustering methods were applied:

### 1. K‑Means
- Optimal k = 2 (Silhouette Score = 0.85)  
- Cluster 0: 2367 patients  
- Cluster 1: 8 patients (rare phenotype)  

### 2. Hierarchical Clustering
- Ward’s method  
- Dendrogram‑based cluster selection  

Clusters differed in:

- Respiratory rate  
- Central venous pressure  
- Severity indicators  

---

##  Key Findings
- Gradient boosting models (LightGBM, XGBoost) performed best  
- PCA revealed strong latent physiological structure  
- Mortality is driven by multi‑system interactions, not single variables  
- Clustering identified distinct patient subgroups  
- Time‑series features improved predictive performance  

---

##  Limitations
- PCA reduces interpretability  
- No deep temporal modeling (e.g., LSTMs)  
- Class imbalance remains challenging  
- External validation needed  
- Clustering produced one very small subgroup  

---

##  Future Work
- Incorporate LSTM/GRU models for raw time‑series  
- Use SHAP on original features for interpretability  
- Explore survival analysis (Cox models, RSF)  
- Validate on MIMIC‑IV dataset  
- Build a real‑time deterioration dashboard  

---

