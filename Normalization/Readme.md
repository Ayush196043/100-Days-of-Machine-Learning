# 📊 Feature Normalization using Min-Max Scaling

> A practical Machine Learning preprocessing project demonstrating how **Min-Max Normalization** transforms numerical features into a common scale while preserving their relative distribution.

---

## ✨ Overview

Feature scaling is an important step in Machine Learning preprocessing.

In this project, the **Wine Dataset** is used to understand how numerical features with different ranges can be transformed into a common range using **Min-Max Normalization**.

The notebook focuses on two features:

* 🍷 **Alcohol**
* 🧪 **Malic Acid**

The dataset is explored visually before and after scaling to understand how normalization affects the feature values and their distributions.

---

## 🎯 What This Project Covers

* Loading and exploring a dataset using **Pandas**
* Visualizing feature distributions using **Seaborn**
* Understanding relationships between features
* Splitting data into training and testing sets
* Applying **Min-Max Scaling**
* Comparing data **before vs. after normalization**
* Analyzing feature statistics
* Visualizing changes in feature distributions

---

## 🧠 Min-Max Normalization

Min-Max Normalization transforms feature values into a fixed range, generally **0 to 1**.

### Formula

```text
X_scaled = (X - X_min) / (X_max - X_min)
```

After normalization:

```text
Minimum → 0
Maximum → 1
```

This allows features with different numerical ranges to be represented on a comparable scale.

---

## 🔄 Machine Learning Workflow

```text
            Wine Dataset
                 │
                 ▼
           Load Dataset
                 │
                 ▼
        Select Relevant Features
        ┌────────┴─────────┐
        │                  │
    Alcohol            Malic Acid
        │                  │
        └────────┬─────────┘
                 ▼
          Data Visualization
                 │
                 ▼
          Train-Test Split
                 │
                 ▼
        Fit MinMaxScaler
          on X_train
                 │
        ┌────────┴─────────┐
        ▼                  ▼
  Transform X_train   Transform X_test
        │                  │
        └────────┬─────────┘
                 ▼
       Before vs After Analysis
                 │
                 ▼
        Visualization & Statistics
```

---

## 📂 Project Structure

```text
Normalization/
│
├── 📓 Normalization.ipynb
├── 📄 wine_data.csv
└── 📖 README.md
```

### `Normalization.ipynb`

Contains the complete Python implementation, preprocessing steps, statistical analysis, and visualizations.

### `wine_data.csv`

The dataset used for demonstrating feature normalization.

### `README.md`

Project documentation and explanation.

---

## 🛠️ Tech Stack

| Technology      | Purpose                               |
| --------------- | ------------------------------------- |
| 🐍 Python       | Programming language                  |
| 🐼 Pandas       | Data loading & manipulation           |
| 🔢 NumPy        | Numerical operations                  |
| 📊 Matplotlib   | Data visualization                    |
| 📈 Seaborn      | Statistical visualization             |
| 🤖 Scikit-learn | Data preprocessing & train-test split |

---

## 📌 Dataset Preparation

The dataset is loaded using Pandas:

```python
df = pd.read_csv(
    'wine_data.csv',
    header=None,
    usecols=[0, 1, 2]
)
```

The selected columns are renamed as:

```python
df.columns = ['Class label', 'Alcohol', 'Malic acid']
```

### Features Used

| Column        | Description              |
| ------------- | ------------------------ |
| `Class label` | Target/Class information |
| `Alcohol`     | Input feature            |
| `Malic acid`  | Input feature            |

---

## 🔀 Train-Test Split

The dataset is divided into training and testing sets using an **80:20 split**.

```python
X_train, X_test, y_train, y_test = train_test_split(
    df.drop('Class label', axis=1),
    df['Class label'],
    test_size=0.2,
    random_state=42
)
```

### Why?

The model preprocessing workflow should learn scaling parameters from the **training data** and then apply those parameters to unseen test data.

---

## ⚙️ Applying Min-Max Scaling

The project uses Scikit-learn's `MinMaxScaler`:

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

scaler.fit(X_train)

X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

The scaler is fitted only on `X_train`.

This avoids using information from the test set while learning the scaling parameters.

---

## 📊 Before vs After Normalization

The notebook compares the original and normalized data through multiple visualizations.

### Scatter Plot

The relationship between **Alcohol** and **Malic Acid** is visualized before and after scaling.

```text
Before Scaling              After Scaling

Alcohol                     Alcohol
   │                           │
   │  ● ●                      │  ● ●
   │ ●   ●                     │ ●   ●
   │   ●                       │   ●
   └──────────                 └──────────
```

The values change in scale, while the underlying relative structure of the features is maintained.

---

## 📈 Distribution Analysis

The notebook also uses **KDE plots** to compare feature distributions.

The distributions are analyzed for:

* Alcohol before normalization
* Alcohol after normalization
* Malic Acid before normalization
* Malic Acid after normalization
* Combined feature distributions

This provides a visual understanding of how scaling changes the numerical range of the features.

---

## 📋 Statistical Comparison

The project compares descriptive statistics before and after normalization:

```python
np.round(X_train.describe(), 1)
```

and:

```python
np.round(X_train_scaled.describe(), 1)
```

The normalized features have a common scale, with the training data reaching approximately:

```text
Minimum = 0
Maximum = 1
```

---

## 🔍 Key Insight

Normalization **changes the scale of numerical features, not their underlying relative relationships**.

For example, a feature that originally contains values across a larger numerical range can be transformed into the same **0–1 scale** as another feature.

This makes the features easier to compare and can be useful for Machine Learning algorithms that are sensitive to feature magnitude.

---

## ⚠️ Important Practice

A key preprocessing principle demonstrated in this project is:

```text
Split Data
    ↓
Fit Scaler on Training Data
    ↓
Transform Training Data
    ↓
Transform Testing Data
```

Instead of:

```text
Scale Entire Dataset
    ↓
Split Data
```

The first approach prevents information from the test set from influencing the scaling process.

---

## ▶️ How to Run

### Google Colab

1. Open `Normalization.ipynb` in Google Colab.
2. Upload `wine_data.csv`.
3. Update the dataset path if required.
4. Run the notebook cells sequentially.

### Jupyter Notebook

Clone the repository:

```bash
git clone <your-repository-url>
```

Navigate to the project:

```bash
cd Normalization
```

Open:

```text
Normalization.ipynb
```

Install the required libraries if needed:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn
```

---

## 🚀 Possible Extensions

This project can be extended by exploring other scaling techniques:

* `StandardScaler`
* `RobustScaler`
* `MaxAbsScaler`
* Comparison between **Normalization vs Standardization**
* Applying scaled data to ML algorithms
* Comparing model performance before and after scaling

---

## 📚 Key Takeaway

> **Feature scaling is not just a preprocessing step — it is a foundation for building reliable Machine Learning workflows.**

Understanding how and when to normalize data is an essential skill for anyone working with Machine Learning.

---

## 👨‍💻 Author

**Ayush Pandey**

🎓 B.Tech Student
🤖 Machine Learning & NLP Enthusiast
💻 Python | Machine Learning | NLP | Data Science

---

## ⭐ If You Found This Useful

If this project helped you understand **Feature Normalization**, consider giving the repository a ⭐.

More Machine Learning preprocessing techniques will be added progressively.

---

### 🕉️ A Thought to Keep Learning

> **“कर्मण्येवाधिकारस्ते मा फलेषु कदाचन।”**
> *You have the right to perform your actions, but not to the fruits of those actions.*

**Keep Learning • Keep Building • Keep Improving 🚀**

