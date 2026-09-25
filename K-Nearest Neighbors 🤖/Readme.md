# 🧠 K-Nearest Neighbors (KNN) Classification

> A beginner-friendly Machine Learning project that explains **KNN classification from data preprocessing to prediction and visualization**.

---

## 👋 About This Project

This repository is created to understand the **K-Nearest Neighbors (KNN)** algorithm practically using Python and Scikit-learn.

The project follows a complete beginner-level Machine Learning workflow:

```text
Dataset
   ↓
Data Cleaning
   ↓
Feature & Target Separation
   ↓
Train-Test Split
   ↓
Feature Scaling
   ↓
KNN Model
   ↓
Prediction
   ↓
Accuracy Evaluation
   ↓
Compare Different K Values
   ↓
Decision Boundary Visualization
```

The notebook is designed so that beginners can follow each step and understand **what is happening and why it is happening**.

---

# 🎯 Project Objective

The main objective of this project is to understand how a **KNN classification model** works.

By working through this project, you will learn:

* What KNN is
* How KNN makes predictions
* What K represents
* Why distance is important
* Why feature scaling is required
* How to split data into training and testing sets
* How to train a KNN classifier
* How to make predictions
* How to calculate accuracy
* How different K values affect the model
* What a decision boundary is
* How to visualize KNN interactively

---

# 🧠 What is KNN?

**KNN stands for K-Nearest Neighbors.**

It is a **supervised Machine Learning algorithm** that can be used for:

* Classification
* Regression

In this project, KNN is used for **classification**.

The basic idea is simple:

> **For a new data point, find its nearest neighbors and use their classes to decide the class of the new point.**

### 🧪 Simple Example

Suppose a new point has these 3 nearest neighbors:

```text
Neighbor 1 → Malignant
Neighbor 2 → Malignant
Neighbor 3 → Benign
```

If:

```text
K = 3
```

then:

```text
Malignant = 2
Benign    = 1
```

The majority class is:

```text
Malignant
```

Therefore, KNN predicts:

```text
Malignant
```

This process is called **majority voting**.

---

# 🔢 What Does K Mean?

`K` represents the number of nearest neighbors that KNN considers.

For example:

```python
KNeighborsClassifier(n_neighbors=3)
```

means:

```text
K = 3
```

The algorithm looks at the **3 nearest data points**.

Different values can be tested:

```text
K = 1
K = 2
K = 3
...
K = 15
```

The notebook compares K values from **1 to 15**.

The notebook's plotted results identify **K = 3** as the selected value.

> **Note:** For a more rigorous Machine Learning workflow, K should ideally be selected using validation or cross-validation instead of repeatedly checking the final test set.

---

# 📂 Repository Structure

```text
KNN-Classification/
│
├── 📓 KNN_Example.ipynb
├── 📊 data_knn.csv
└── 📖 README.md
```

| File                | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| `KNN_Example.ipynb` | Main Jupyter Notebook containing the complete KNN implementation |
| `data_knn.csv`      | Dataset used for the classification example                      |
| `README.md`         | Complete project documentation                                   |

---

# 📊 Dataset

The project uses:

```text
data_knn.csv
```

The CSV contains:

```text
569 rows
33 columns
```

Important columns include:

* `id`
* `diagnosis`
* `radius_mean`
* `texture_mean`
* `perimeter_mean`
* `area_mean`
* `smoothness_mean`
* `compactness_mean`
* `concavity_mean`
* `concave points_mean`
* `symmetry_mean`
* `fractal_dimension_mean`
* `_se` features
* `_worst` features
* `Unnamed: 32`

The target column is:

```text
diagnosis
```

The notebook removes:

```text
id
Unnamed: 32
```

before using the data for the model.

---

# 🛠️ Technologies Used

This project uses:

* 🐍 Python
* 📓 Jupyter Notebook
* 🐼 Pandas
* 🔢 NumPy
* 📈 Matplotlib
* 🤖 Scikit-learn
* 🎛️ ipywidgets

---

# 📦 Installation

Make sure Python is installed on your computer.

Then install the required libraries:

```bash
pip install pandas numpy matplotlib scikit-learn ipywidgets jupyter
```

---

# ▶️ How to Run the Project

## 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Move into the project directory:

```bash
cd KNN-Classification
```

## 2. Start Jupyter Notebook

```bash
jupyter notebook
```

## 3. Open the Notebook

Open:

```text
KNN_Example.ipynb
```

## 4. Check the Dataset Path

Make sure:

```text
KNN_Example.ipynb
data_knn.csv
```

are in the same directory.

Use:

```python
df = pd.read_csv("data_knn.csv")
```

If your notebook contains a Google Drive path such as:

```python
df = pd.read_csv("/content/drive/MyDrive/Machine Learning Csmpusx/KNN/data_knn.csv")
```

replace it with:

```python
df = pd.read_csv("data_knn.csv")
```

This makes the project easier to run after cloning it from GitHub.

---

# 🔄 Complete Project Workflow

```text
                Dataset
                   ↓
              Load CSV
                   ↓
            Data Cleaning
                   ↓
       Features & Target
                   ↓
          Train-Test Split
                   ↓
          Feature Scaling
                   ↓
          Create KNN Model
                   ↓
            Train Model
                   ↓
             Prediction
                   ↓
          Accuracy Score
                   ↓
        Try Different K Values
                   ↓
         K vs Accuracy Graph
                   ↓
       Decision Boundary Plot
```

---

# 1️⃣ Import Libraries

The notebook starts with libraries such as:

```python
import pandas as pd
import numpy as np
```

### Pandas

Pandas is used for handling tabular data.

For example:

```python
pd.read_csv("data_knn.csv")
```

loads the CSV file.

### NumPy

NumPy is used for numerical operations and arrays.

---

# 2️⃣ Load the Dataset

The dataset is loaded using:

```python
df = pd.read_csv("data_knn.csv")
```

To see the first five rows:

```python
df.head()
```

This helps us understand the structure of the dataset.

---

# 3️⃣ Remove Unnecessary Columns

The notebook removes:

```python
df.drop(columns=['id','Unnamed: 32'], inplace=True)
```

### Why?

`id` is an identifier and is not intended to be used as a predictive feature.

`Unnamed: 32` is an unnecessary column in this dataset.

Removing them makes the dataset cleaner for the Machine Learning workflow.

---

# 4️⃣ Check Dataset Shape

Use:

```python
df.shape
```

The original CSV contains:

```text
569 rows
33 columns
```

---

# 5️⃣ Check Missing Values

The notebook uses:

```python
df.isnull().sum()
```

This checks the number of missing values in every column.

Checking missing values is an important data-preprocessing step.

---

# 6️⃣ Separate Features and Target

Machine Learning data can be divided into:

```text
X → Features / Input
y → Target / Output
```

In this project:

```text
Target = diagnosis
```

The feature columns are used as input and `diagnosis` is the target.

---

# 7️⃣ Train-Test Split

The notebook uses:

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    df.iloc[:,1:],
    df.iloc[:,0],
    test_size=0.2,
    random_state=2
)
```

### `test_size=0.2`

This means:

```text
80% → Training Data
20% → Testing Data
```

### Training Data

Used to build the model.

### Testing Data

Used to evaluate the model on unseen data.

---

# 🎲 What is `random_state`?

The notebook uses:

```python
random_state=2
```

It makes the random train-test split reproducible.

Using the same dataset and same `random_state` gives the same split.

---

# 8️⃣ Feature Scaling

KNN is a **distance-based algorithm**, so feature scaling is very important.

Imagine:

```text
Age    = 20
Salary = 100000
```

Salary has a much larger numerical scale.

Without scaling, the larger-scale feature can have a much greater influence on distance calculations.

The notebook uses:

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

## `fit_transform()` vs `transform()`

For training data:

```python
X_train = scaler.fit_transform(X_train)
```

`fit()` learns the scaling parameters from the training data.

`transform()` applies the scaling.

For test data:

```python
X_test = scaler.transform(X_test)
```

We use only `transform()`.

This prevents the test data from being used to calculate the scaling parameters and helps avoid **data leakage**.

---

# 9️⃣ Create the KNN Model

The notebook uses:

```python
from sklearn.neighbors import KNeighborsClassifier

knn = KNeighborsClassifier(n_neighbors=3)
```

Here:

```text
n_neighbors = 3
```

means:

```text
K = 3
```

The model considers the 3 nearest neighbors when making a prediction.

---

# 🔟 Train the Model

The model is trained using:

```python
knn.fit(X_train, y_train)
```

KNN has a relatively simple training process compared with many other Machine Learning algorithms.

It primarily stores the training data and uses distance calculations when predictions are requested.

---

# 1️⃣1️⃣ Make Predictions

Predictions are made using:

```python
y_pred = knn.predict(X_test)
```

Now:

```text
y_test → Actual values
y_pred → Predicted values
```

We can compare them to evaluate the model.

---

# 1️⃣2️⃣ Calculate Accuracy

The notebook uses:

```python
from sklearn.metrics import accuracy_score

accuracy_score(y_test, y_pred)
```

Accuracy represents the proportion of predictions that were correct.

For example:

```text
100 predictions
90 correct
```

would give:

```text
Accuracy = 90%
```

---

# 📈 Finding a Suitable K

The notebook checks:

```python
for i in range(1,16):
```

This tests:

```text
K = 1
K = 2
K = 3
...
K = 15
```

For each K, the notebook:

1. Creates a KNN model
2. Trains the model
3. Makes predictions
4. Calculates accuracy
5. Stores the score

The scores are stored in:

```python
scores = []
```

---

# 📊 K vs Accuracy Graph

The notebook visualizes the scores using:

```python
plt.plot(range(1,16), scores)
```

This graph shows how accuracy changes as K changes.

The notebook identifies **K = 3** from the plotted results.

### Better practice for real projects

For a more rigorous model-selection process:

```text
Training Data
      ↓
Cross Validation
      ↓
Select K
      ↓
Final Model
      ↓
Test Data
      ↓
Final Evaluation
```

This avoids repeatedly using the final test set to choose hyperparameters.

---

# 🗺️ Decision Boundary Visualization

The notebook also demonstrates KNN using Scikit-learn's built-in breast cancer dataset:

```python
cancer = datasets.load_breast_cancer()
```

It selects two features:

```python
cancer.data[:, :2]
```

and standardizes them:

```python
X = StandardScaler().fit_transform(cancer.data[:, :2])
```

The target is:

```python
y = cancer.target
```

A grid is created using:

```python
np.meshgrid()
```

The KNN model then predicts the class for points across the grid.

These predictions are used to visualize the **decision boundary**.

---

# 🎛️ Interactive KNN Visualization

The notebook uses:

```python
from ipywidgets import interact, fixed
```

and:

```python
interact(
    plot_decision_boundaries,
    n_neighbors=(1, 20),
    data=fixed(X),
    labels=fixed(y)
)
```

This allows K to be changed interactively:

```text
K = 1 → 20
```

You can observe how the decision boundary changes as K changes.

---

# 🧮 Euclidean Distance

One of the common distance measures used with KNN is **Euclidean distance**.

For two points:

```text
A = (x₁, x₂)
B = (y₁, y₂)
```

the distance is:

[
d(A,B)=\sqrt{(x_1-y_1)^2+(x_2-y_2)^2}
]

For multiple features:

[
d(A,B)=\sqrt{\sum_{i=1}^{n}(x_i-y_i)^2}
]

KNN uses distance to find the nearest data points.

---

# 🧪 Simple KNN Example

Suppose:

```text
K = 5
```

and the nearest neighbors are:

```text
Benign
Benign
Malignant
Benign
Malignant
```

Count:

```text
Benign    = 3
Malignant = 2
```

Therefore, the majority class is:

```text
Benign
```

So the prediction is:

```text
Benign
```

---

# 📚 Important Concepts

While studying this project, make sure you understand:

### 1. Supervised Learning

Learning from labeled data.

### 2. Classification

Predicting a class/category.

### 3. Features

Input variables used by the model.

### 4. Target

The output that the model predicts.

### 5. Distance

A measure of how close two points are.

### 6. Feature Scaling

Putting features on comparable scales.

### 7. K Value

Number of nearest neighbors considered.

### 8. Majority Voting

The most common class among the selected neighbors becomes the prediction.

### 9. Accuracy

The proportion of correct predictions.

### 10. Decision Boundary

A boundary separating regions where different classes are predicted.

---

# ⚠️ Notes About the Current Notebook

### 1. Dataset Path

The original notebook contains a Google Drive/Colab-specific path.

For GitHub, use:

```python
pd.read_csv("data_knn.csv")
```

when the CSV is in the same folder.

### 2. Duplicate Train-Test Split

The notebook contains the train-test split code more than once.

The duplicate code can be removed to make the notebook cleaner.

### 3. K Selection

The notebook compares different K values using the same test set.

For a stronger Machine Learning workflow, use:

```text
Cross-Validation / Validation Set
```

to select K and keep the final test set untouched until final evaluation.

---

# 🚀 Future Improvements

You can extend this project by adding:

* Confusion Matrix
* Classification Report
* Precision
* Recall
* F1-Score
* Cross-Validation
* GridSearchCV
* Hyperparameter Tuning
* Different Distance Metrics
* Weighted KNN
* ROC-AUC
* Validation Set
* Better Visualizations

For example:

```python
from sklearn.model_selection import cross_val_score
```

can be used for cross-validation.

---

# 👨‍💻 Beginner Learning Path

If you are completely new to Machine Learning, follow this order:

```text
Python
  ↓
NumPy
  ↓
Pandas
  ↓
DataFrame
  ↓
Features & Target
  ↓
Train-Test Split
  ↓
Feature Scaling
  ↓
Distance Formula
  ↓
KNN
  ↓
K Value
  ↓
Prediction
  ↓
Accuracy
  ↓
Cross Validation
  ↓
Decision Boundary
```

---

# 🎯 What You Should Be Able to Explain

After completing this project, you should be able to answer:

* What is KNN?
* What does K represent?
* Why is KNN called a lazy learning algorithm?
* Why does KNN need feature scaling?
* What is Euclidean distance?
* What is train-test split?
* What does `test_size=0.2` mean?
* What does `random_state=2` do?
* What is `fit_transform()`?
* Why do we use only `transform()` on test data?
* What does `n_neighbors=3` mean?
* How does KNN classify a new data point?
* What is majority voting?
* What is accuracy?
* How does changing K affect the model?
* What is a decision boundary?
* Why is cross-validation useful for selecting K?

---

# 📌 Quick Summary

The complete project can be summarized as:

```text
📊 Dataset
    ↓
🧹 Data Cleaning
    ↓
🎯 Features & Target
    ↓
✂️ Train-Test Split
    ↓
📏 StandardScaler
    ↓
🤖 KNN Classifier
    ↓
🏋️ Model Training
    ↓
🔮 Prediction
    ↓
📈 Accuracy
    ↓
🔢 Compare K = 1 to 15
    ↓
🗺️ Decision Boundary
```

### The Core Idea of KNN

> **Find the nearest data points → look at their classes → use majority voting → make the prediction.**

---

# ⭐ If You Find This Project Helpful

If this project helps you understand KNN and Machine Learning, consider giving the repository a ⭐ on GitHub.

Your feedback and suggestions are welcome!

---

# 👨‍💻 Author

## Ayush Pandey

**B.Tech Student | Machine Learning & AI Enthusiast**

### Areas of Interest

* 🤖 Machine Learning
* 🧠 Artificial Intelligence
* 🗣️ Natural Language Processing
* 🐍 Python
* 📊 Data Science

### GitHub

**Ayush196043**

---

## 📜 License

This project is created for **educational and learning purposes**.

