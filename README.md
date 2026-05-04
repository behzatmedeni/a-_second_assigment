<div align="center">
  <img src="https://media.giphy.com/media/1n7dpJ1vQvoR1w0xGj/giphy.gif" alt="Neural Network Animation" width="100%"/>

  <h1>⚡ Smart Electrical Grid Stability Prediction Pipeline</h1>
  
  **An Exhaustive End-to-End Machine Learning Workflow Built with Orange Data Mining**

  <p align="center">
    <img src="https://img.shields.io/badge/Machine%20Learning-FF6F00?style=for-the-badge&logo=scikitlearn&logoColor=white" alt="ML Badge"/>
    <img src="https://img.shields.io/badge/Orange%20Data%20Mining-FFA500?style=for-the-badge&logo=appveyor&logoColor=white" alt="Orange Badge"/>
    <img src="https://img.shields.io/badge/Data%20Science-20232A?style=for-the-badge&logo=python&logoColor=white" alt="Data Science"/>
    <img src="https://img.shields.io/badge/Clustering-Unsupervised-blue?style=for-the-badge" alt="Unsupervised"/>
    <img src="https://img.shields.io/badge/Classification-Supervised-green?style=for-the-badge" alt="Supervised"/>
  </p>
</div>

<br/>

## 📖 Executive Summary
This repository contains a highly detailed and visually constructed machine learning pipeline designed to predict the stability of an electrical power grid. The project encompasses the full data science lifecycle: **Data Ingestion, Pre-processing, Exploratory Data Analysis (EDA), Unsupervised Clustering**, and **Supervised Classification with extensive hyperparameter tuning**. 

The entire architecture was developed utilizing the node-based visual programming environment of **Orange Data Mining**, allowing for an intuitive yet mathematically rigorous approach to model evaluation.

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%"/>
</div>

## 🗄️ Dataset Specifications
The project utilizes the **Electrical Grid Stability Simulated Dataset** from the UCI Machine Learning Repository.

- **Dataset Size:** 10,000 recorded instances.
- **Independent Variables (Features):** 12 continuous numerical attributes.
  - `tau1` to `tau4`: Reaction times of the grid nodes.
  - `p1` to `p4`: Nominal power produced/consumed.
  - `g1` to `g4`: Coefficient (price elasticity) values.
- **Target Variable:** `stabf` (Categorical: **stable** or **unstable**).

### 🚨 Critical Engineering Step: Preventing Data Leakage
The raw dataset contains an additional continuous feature named `stab`. This feature represents the direct mathematical calculation used to derive the target label `stabf`. Including `stab` in the feature set would result in severe **Data Leakage**, causing the models to achieve 100% accuracy without genuinely learning the grid's operational parameters. 
> **Resolution:** During the initial data loading phase via the `File` widget, the `stab` column was explicitly assigned the **Skip/Meta** role. All subsequent learning was strictly constrained to the 12 independent operational features.

---

## 🏗️ Orange Workflow Architecture

Below is the complete macro-architecture of the visual pipeline designed for this research:

*[Buraya Tüm Orange Ekranının (Genel Akışın) Görüntüsünü Sürükle]*

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%"/>
</div>

## 🔍 PART I: Exploratory Data Analysis (EDA) & Pre-processing
Before subjecting the data to complex algorithms, a foundational statistical and visual inspection was performed.

### Statistical Baselines
Using the `Data Table` and `Column Statistics` widgets, we confirmed the integrity of the dataset. There were **zero missing values**, negating the need for imputation techniques.

*[Buraya Column Statistics / Data Table Ekran Görüntüsünü Sürükle]*

### Visual Feature Separability
To understand the complexity of the classification task, 2D projections of the multi-dimensional space were analyzed:
- **Scatter Plots:** By plotting various feature pairs (e.g., `tau1` vs. `p1`) and color-coding them by the `stabf` target, it became evident that the *stable* and *unstable* classes heavily overlap. This visual proof confirms that simple linear classifiers would fail, necessitating non-linear models.
- **Distributions:** Histograms were utilized to observe the density and spread of individual features across the two grid states.

*[Buraya 2 Adet Scatter Plot ve 2 Adet Histogram/Distribution Ekran Görüntüsünü Sürükle]*

---

## 🧩 PART II: Unsupervised Learning (Clustering)
We simulated a scenario where the target labels were unknown to observe if the algorithms could naturally discover the grid's stability states.

<div align="center">
  <img src="https://media.giphy.com/media/l41lOemX4rGMBB1W8/giphy.gif" alt="Clustering Concept" width="400"/>
</div>

### 1. K-Means Clustering & Silhouette Optimization
The K-Means algorithm was deployed with an automated optimization parameter to test $k$ values ranging from 2 to 6.
- **Finding:** The algorithm dynamically calculated the highest **Silhouette Score (0.105)** at exactly **$k=2$**. 
- **Conclusion:** Even without knowing the labels, the algorithm organically deduced that the data is best represented in two distinct clusters, perfectly mirroring our ground truth (*stable* and *unstable*).

*[Buraya K-Means Silhouette Skorlarının Olduğu Ekran Görüntüsünü Sürükle]*

### 2. Hierarchical Clustering (Dendrogram Analysis)
A Euclidean distance matrix was generated, followed by Hierarchical Clustering using Ward's linkage. By manipulating the cut-off line (Top N pruning), we observed the hierarchical granularity of the grid events:
- **Top N = 2:** Shows the macro-division between the primary stability states.
- **Top N = 4 & Top N = 7:** Reveals how the primary clusters break down into micro-clusters as the distance threshold is lowered, indicating varying degrees of instability or operational variances.

*[Buraya Dendrogram'ın 2, 4 ve 7 Küme İçin Alınmış 3 Farklı Ekran Görüntüsünü Sürükle]*

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%"/>
</div>

## 🚀 PART III: Supervised Learning & Hyperparameter Tuning

To create robust predictive models, the dataset was piped through a **Data Sampler** and split using an **80% Training / 20% Test** ratio (Stratified sampling). To prevent overfitting, models were evaluated *strictly* on the isolated 20% test data (`Test on test data` mode in the `Test and Score` widget).

Three machine learning models were deployed, and rigorous hyperparameter experiments were conducted for each:

### 1. K-Nearest Neighbors (kNN)
A distance-based lazy learner. We experimented with the smoothness of the decision boundary by altering the number of neighbors ($k$).
- **Experiment 1:** $k = 3$
- **Experiment 2:** $k = 7$
- **Experiment 3:** $k = 15$

*[Buraya kNN İçin Yaptığın Deneylerin Test and Score Ekran Görüntülerini Sürükle]*

### 2. Random Forest
An ensemble learning method utilized for its resilience against overfitting in complex datasets. We experimented with the depth and width of the forest.
- **Experiment 1:** Number of Trees = 10
- **Experiment 2:** Number of Trees = 50
- **Experiment 3:** Number of Trees = 100

*[Buraya Random Forest İçin Yaptığın Deneylerin Test and Score Ekran Görüntülerini Sürükle]*

### 3. Artificial Neural Networks (ANN)
A Multi-layer Perceptron (MLP) capable of mapping highly non-linear functions. We experimented with the network's architectural depth.
- **Experiment 1:** 1 Hidden Layer (50 neurons)
- **Experiment 2:** 2 Hidden Layers (100, 50 neurons)
- **Experiment 3:** 2 Hidden Layers (200, 100 neurons)

*[Buraya Neural Network İçin Yaptığın Deneylerin Test and Score Ekran Görüntülerini Sürükle]*

---

## 🏆 Final Model Evaluation & Conclusion

The models were evaluated using a comprehensive suite of metrics including **Classification Accuracy (CA)**, Area Under ROC Curve (AUC), F1-Score, Precision, and Recall.

| Rank | Algorithm | Classification Accuracy (CA) | Best Hyperparameter Configuration |
| :---: | :--- | :---: | :--- |
| 🥇 | **Neural Network** | **~ 95.7%** | 2 Hidden Layers (100, 50) |
| 🥈 | **Random Forest** | ~ 88.4% | 100 Trees |
| 🥉 | **kNN** | ~ 77.8% | k = 5 Neighbors |

### Confusion Matrix Analysis
The evaluation via the `Confusion Matrix` widget revealed that the **Neural Network** not only had the highest overall accuracy but also exceptional precision in minimizing False Positives. In the context of electrical grids, incorrectly classifying an *unstable* state as *stable* could lead to catastrophic power failures, making the Neural Network's high recall rate critical for deployment.

*[Buraya Sonuçları Gösteren Test and Score ve Confusion Matrix Ekran Görüntülerini Sürükle]*

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%"/>
</div>

## 🔮 Future Work & Scalability
The methodology established in this visual pipeline lays the groundwork for programmatic implementation. Future iterations of this project will focus on:
- **Backend Integration:** Translating the optimal Neural Network architecture into Python (`TensorFlow`/`PyTorch`) and integrating it with a distributed high-performance **Go (Golang)** backend.
- **Cloud Deployment:** Utilizing **Amazon Web Services (AWS)** to host the predictive model as a scalable API, enabling real-time telemetry analysis and automated infrastructure protection for smart grids.

<br/>

<div align="center">
  <img src="https://media.giphy.com/media/26xBEamXwaMSUbV72/giphy.gif" alt="End Animation" width="200"/>
  <br><br>
  <b>Developed by Behzat Medeni</b><br>
  <i>Computer Engineering Student (Technology Faculty) | Exchange Student at Riga Technical University (RTU)</i>
</div>
