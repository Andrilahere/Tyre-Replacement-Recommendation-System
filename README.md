# Tyre-Replacement-Recommendation-System
A Machine Learning based recommendation system that predicts the most suitable reusable tyre for vehicle tyre replacement using structured fleet inventory data.  The system ranks available reusable tyres based on operational constraints such as tread depth similarity, brand/model compatibility, and usage patterns.

## 📌 Problem Statement

Fleet operators often need to replace worn tyres with reusable inventory tyres.  
Selecting the best replacement manually is time-consuming and may lead to sub-optimal decisions.

This project builds an intelligent recommendation engine that:

- Evaluates all available reusable tyres for a given worn tyre
- Predicts a suitability score
- Returns a **ranked list of top replacement recommendations**

---

## 🧠 Solution Approach

### Feature Engineering

Key ranking features designed based on domain logic:

- **NSD Similarity** → Closest tread depth match (highest priority)
- **Model Match** → Exact model compatibility
- **Brand Match** → Brand-level matching when model differs
- **Pattern Match** → Tyre usage pattern compatibility
- Additional derived numerical and boolean interaction features

---

### Dataset Construction

- Created pairwise match dataset between worn tyres and reusable tyre inventory
- ~142K training samples generated from structured JSON fleet datasets
- Group-based split performed to ensure tyres in test set were completely unseen during training

---

### Model Training & Comparison

Models evaluated:

- ✅ XGBoost (Final Model)
- LightGBM
- K-Nearest Neighbors (KNN)

XGBoost selected due to:

- Best ranking performance
- Correct learning of business hierarchy  
  **NSD > Model > Brand > Pattern**
- Stable prediction performance on unseen tyres

---

## 📊 Evaluation Metrics

### Regression Metrics

- RMSE: **0.0012**
- R² Score: **1.00**

### Ranking Metrics (Primary KPIs)

- **Top-1 Accuracy:** 74.2%
- **Top-3 Accuracy:** 95.5%

Top-3 accuracy demonstrates that the correct replacement tyre appears within the top recommendations in almost all real-world scenarios.

---

## ✅ Generalization Strategy

- Used **GroupShuffleSplit by tyre_id**
- Ensured all candidate pairs of a tyre belong entirely to either train or test set
- Prevented data leakage and simulated real deployment conditions

---

## 🏗 Tech Stack

- Python
- Pandas / NumPy
- Scikit-learn
- XGBoost / LightGBM / KNN
- Matplotlib

---

## 📦 Output Format

The system returns structured ranked recommendations:

```json
{
  "vehicle_id": "V123",
  "position": "FL",
  "recommendations": [
    {
      "tyre_id": "T12",
      "score": 0.91,
      "reason": "closest NSD + same brand"
    }
  ]
}
