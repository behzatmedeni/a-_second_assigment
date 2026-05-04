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
<img width="588" height="559" alt="Ekran görüntüsü 2026-05-02 232156" src="https://github.com/user-attachments/assets/ca044f2c-8960-4d40-81ee-08b50b2e5fc2" />

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%"/>
</div>

## 🔍 PART I: Exploratory Data Analysis (EDA) & Pre-processing
Before subjecting the data to complex algorithms, a foundational statistical and visual inspection was performed.

### Statistical Baselines
Using the `Data Table` and `Column Statistics` widgets, we confirmed the integrity of the dataset. There were **zero missing values**, negating the need for imputation techniques.

<img width="992" height="650" alt="Ekran görüntüsü 2026-05-02 224057" src="https://github.com/user-attachments/assets/b765c148-20c4-48ce-89dd-5d6642aa602f" />


### Visual Feature Separability
To understand the complexity of the classification task, 2D projections of the multi-dimensional space were analyzed:
- **Scatter Plots:** By plotting various feature pairs (e.g., `tau1` vs. `p1`) and color-coding them by the `stabf` target, it became evident that the *stable* and *unstable* classes heavily overlap. This visual proof confirms that simple linear classifiers would fail, necessitating non-linear models.
- **Distributions:** Histograms were utilized to observe the density and spread of individual features across the two grid states.

<img width="992" height="650" alt="Ekran görüntüsü 2026-05-02 224057" src="https://github.com/user-attachments/assets/b7d511f3-ee7b-4f76-a89c-9f055c991ff1" />

<img width="1280" height="745" alt="Ekran görüntüsü 2026-05-02 224340" src="https://github.com/user-attachments/assets/ca821eb0-9e51-46ae-8809-4a9c1e3cc8df" />
<img width="1267" height="517" alt="Ekran görüntüsü 2026-05-02 224122" src="https://github.com/user-attachments/assets/e3c00280-668f-4038-a4a6-d1791011b74d" />

<img width="1267" height="510" alt="Ekran görüntüsü 2026-05-02 224112" src="https://github.com/user-attachments/assets/0144d461-58a3-4db1-a4d1-f8327f7804ab" />


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

<img width="553" height="400" alt="Ekran görüntüsü 2026-05-02 225216" src="https://github.com/user-attachments/assets/772f272f-8721-4f5e-9d7c-d6d96569f4c3" />


### 2. Hierarchical Clustering (Dendrogram Analysis)
A Euclidean distance matrix was generated, followed by Hierarchical Clustering using Ward's linkage. By manipulating the cut-off line (Top N pruning), we observed the hierarchical granularity of the grid events:
- **Top N = 2:** Shows the macro-division between the primary stability states.
- **Top N = 4 & Top N = 7:** Reveals how the primary clusters break down into micro-clusters as the distance threshold is lowered, indicating varying degrees of instability or operational variances.

<img width="1919" height="1012" alt="Ekran görüntüsü 2026-05-02 225924" src="https://github.com/user-attachments/assets/0f3d24f8-4051-4e36-b30f-956121a33c96" />
<img width="1919" height="1016" alt="Ekran görüntüsü 2026-05-02 225952" src="https://github.com/user-attachments/assets/50109020-1526-4ac0-bc8b-2e13e8a365a3" />
<img width="1919" height="1015" alt="Ekran görüntüsü 2026-05-02 230009" src="https://github.com/user-attachments/assets/453123a0-b7d6-4920-b139-dbe68f9f6d17" />


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

<img width="967" height="636" alt="Ekran görüntüsü 2026-05-02 231506" src="https://github.com/user-attachments/assets/fe58464e-f8d2-4dd6-af45-2b867ba3503a" />
<img width="969" height="648" alt="Ekran görüntüsü 2026-05-02 231517" src="https://github.com/user-attachments/assets/ad883702-5e42-4156-9449-87a0af90cd34" />
<img width="966" height="656" alt="Ekran görüntüsü 2026-05-02 231527" src="https://github.com/user-attachments/assets/e80e6e0d-802f-4cf5-ab88-6bdecd3ef14d" />

### 2. Random Forest
An ensemble learning method utilized for its resilience against overfitting in complex datasets. We experimented with the depth and width of the forest.
- **Experiment 1:** Number of Trees = 10
- **Experiment 2:** Number of Trees = 50
- **Experiment 3:** Number of Trees = 100

<img width="966" height="656" alt="Ekran görüntüsü 2026-05-02 231527" src="https://github.com/user-attachments/assets/6af17a42-b6a2-45e1-ad5f-a1860075a5af" />
<img width="971" height="654" alt="Ekran görüntüsü 2026-05-02 231623" src="https://github.com/user-attachments/assets/d03e3097-7d20-4b4c-8215-db9dc5249cd3" />
<img width="971" height="657" alt="Ekran görüntüsü 2026-05-02 231632" src="https://github.com/user-attachments/assets/68f9370f-0f1a-4b05-9adf-e584345c6271" />



### 3. Artificial Neural Networks (ANN)
A Multi-layer Perceptron (MLP) capable of mapping highly non-linear functions. We experimented with the network's architectural depth.
- **Experiment 1:** 1 Hidden Layer (50 neurons)
- **Experiment 2:** 2 Hidden Layers (100, 50 neurons)
- **Experiment 3:** 2 Hidden Layers (200, 100 neurons)

<img width="968" height="657" alt="Ekran görüntüsü 2026-05-02 231724" src="https://github.com/user-attachments/assets/a346ad72-11e5-47f1-88bf-737676315855" />

<img width="969" height="660" alt="Ekran görüntüsü 2026-05-02 231756" src="https://github.com/user-attachments/assets/4085663b-ecca-440b-8111-87b339145d06" />
<img width="975" height="651" alt="Ekran görüntüsü 2026-05-02 231821" src="https://github.com/user-attachments/assets/34b9dd01-ac9c-468e-ada4-813635b5e7e8" />

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

<img width="1919" height="1019" alt="Ekran görüntüsü 2026-05-03 190311" src="https://github.com/user-attachments/assets/7ae0824c-d5d1-4943-88fb-fb2d805796f2" />

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
