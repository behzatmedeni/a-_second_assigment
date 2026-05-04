<div align="center">
  <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExcHZhZWEyMThmZmV0YnU0a3I3ZnMzNWd1ZGw4azQ4d3EzZWNpZTJ6eCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/L5oVD5XQYQn2Kj1kPj/giphy.gif" alt="AI and Machine Learning Animation" width="100%"/>

  <h1>⚡ Electrical Grid Stability Prediction Pipeline</h1>
  
  **An End-to-End Machine Learning Workflow Developed with Orange Data Mining**

  <p align="center">
    <img src="https://img.shields.io/badge/Machine%20Learning-FF6F00?style=for-the-badge&logo=scikitlearn&logoColor=white" alt="ML Badge"/>
    <img src="https://img.shields.io/badge/Orange%20Data%20Mining-FFA500?style=for-the-badge&logo=appveyor&logoColor=white" alt="Orange Badge"/>
    <img src="https://img.shields.io/badge/Data%20Science-20232A?style=for-the-badge&logo=python&logoColor=white" alt="Data Science"/>
    <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge" alt="Status"/>
  </p>
</div>

<br/>

## 📖 Project Overview
This repository showcases a comprehensive, visual machine learning pipeline aimed at predicting the stability of an electrical power grid. By leveraging both **Supervised Classification** and **Unsupervised Clustering** techniques, this project identifies critical patterns in grid node interactions and evaluates state-of-the-art predictive models.

The entire workflow—from data pre-processing and Exploratory Data Analysis (EDA) to hyperparameter optimization—was constructed without writing raw code, utilizing the node-based architecture of **Orange Data Mining**.

---

<div align="center">
  <img src="https://media.giphy.com/media/xT9IgzoKnwFNmISR8I/giphy.gif" alt="Data Analytics" width="400"/>
</div>

## 📊 Dataset Specifications
The dataset utilized is the **Electrical Grid Stability Simulated Dataset** from the UCI Machine Learning Repository[cite: 1].

- **Total Instances:** 10,000 grid stability events[cite: 1].
- **Features:** 12 continuous predictive attributes representing response times (`tau1`-`tau4`), power balances (`p1`-`p4`), and price elasticities (`g1`-`g4`)[cite: 1].
- **Target Variable:** `stabf` (Categorical variable: *stable* or *unstable*)[cite: 1].

### ⚠️ Critical Pre-processing: Preventing Data Leakage
The original dataset file (`Data_for_UCI_named.csv`) contains a continuous variable named `stab`, which is the direct mathematical formula output determining the categorical target `stabf`[cite: 1]. 
> **Engineering Decision:** To prevent severe **Data Leakage** and ensure the models learn the actual underlying distributions rather than cheating via a direct identifier, the `stab` column was explicitly assigned the *Skip/Meta* role during data ingestion[cite: 1]. The models were trained strictly on the 12 independent grid features[cite: 1].

---

## ⚙️ Architecture & Workflow

### 1. Exploratory Data Analysis (EDA)
Before model training, a thorough visual inspection of the dataset was conducted to understand feature separability and identify potential outliers:
- **Distributions:** Histograms revealed overlapping characteristics between the *stable* and *unstable* classes, indicating a highly non-linear classification problem.
- **Scatter Plots:** Multi-dimensional scatter projections confirmed that linear separation boundaries would be insufficient for this dataset.

*(Insert your Scatter Plot screenshot here: `![Scatter Plot](link_to_image)`)*

### 2. Unsupervised Learning (Clustering)
We assumed the dataset was unlabeled and applied clustering algorithms to discover organic groupings.

<div align="center">
  <img src="https://media.giphy.com/media/26tn33aiTi1jIGsnu/giphy.gif" alt="Clustering Animation" width="350"/>
</div>

- **K-Means Clustering:** 
  - Optimized the $k$ value from 2 to 6.
  - The maximum **Silhouette Score (0.105)** was achieved precisely at **$k=2$**. This mathematically validated our dataset's natural underlying structure, as we have exactly two ground-truth classes.
- **Hierarchical Clustering:** 
  - Computed Euclidean distances and generated a Dendrogram using Ward's linkage.
  - Applied pruning and varying cut-off lines to observe the hierarchical breakdown of grid events into 2, 4, and 7 sub-clusters.

*(Insert your Dendrogram screenshot here: `![Dendrogram](link_to_image)`)*

---

### 3. Supervised Learning (Classification)
To predict grid stability, the dataset was split using an **80% Training / 20% Test** ratio via a Stratified Data Sampler. All models were strictly evaluated on the unseen 20% test data to simulate real-world inference.

We trained and fine-tuned three distinct algorithms:

#### 🧠 Artificial Neural Networks (ANN)
- **Architecture:** Multi-layer Perceptron (MLP).
- **Hyperparameter Experiments:** Tested varying depth and complexity by altering the Hidden Layers (e.g., 50 neurons vs. [100, 50] vs. [200, 100]).

#### 🌲 Random Forest
- **Architecture:** Ensemble learning method utilizing decision trees.
- **Hyperparameter Experiments:** Optimized the Number of Trees (10 vs. 50 vs. 100) to balance bias and variance.

#### 📍 K-Nearest Neighbors (kNN)
- **Architecture:** Distance-based lazy learning.
- **Hyperparameter Experiments:** Adjusted the $k$ parameter (k=3, 5, 15) to find the optimal decision boundary smoothness.

---

## 🏆 Model Evaluation & Results

The models were evaluated comprehensively using **Classification Accuracy (CA)**, Area Under Curve (AUC), F1-Score, Precision, and Recall.

| Rank | Algorithm | Classification Accuracy (CA) | Best Hyperparameter Configuration |
| :---: | :--- | :---: | :--- |
| 🥇 | **Neural Network** | **~ 95.7%** | 2 Hidden Layers (100, 50) |
| 🥈 | **Random Forest** | ~ 88.4% | 100 Trees |
| 🥉 | **kNN** | ~ 77.8% | k = 5 Neighbors |

### Confusion Matrix Analysis
The Neural Network demonstrated exceptional precision in identifying *unstable* grid events, minimizing False Positives, which is critical for real-world electrical infrastructure security and proactive failure mitigation.

*(Insert your Test & Score / Confusion Matrix screenshot here: `![Results](link_to_image)`)*

---

## 🚀 Future Work & Scalability
While this pipeline was built locally, future iterations of this project could involve:
- **Cloud Deployment:** Exporting the trained Neural Network model into a Python environment (`scikit-learn` or `TensorFlow`) and deploying it as a microservice on **AWS (Amazon Web Services)** using AWS Lambda and API Gateway.
- **Real-time Monitoring:** Integrating the prediction endpoint with a distributed Go-based backend (similar to ASFDN structures) to monitor live smart-grid telemetry data.

---

<div align="center">
  <i>"Predicting the unpredictable to keep the lights on."</i>
  <br><br>
  <b>Developed by Behzat Medeni</b><br>
  <i>Computer Engineering Student (Technology Faculty) | Exchange Student at Riga Technical University (RTU)</i>
</div>
