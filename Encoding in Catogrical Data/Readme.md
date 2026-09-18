# Categorical Data Encoding

Machine Learning projects mein data hamesha numbers ke form mein nahi hota. Bahut baar humein categorical values milti hain, jaise:

```text
Gender → Male, Female
City → Delhi, Mumbai, Noida
Education → Bachelor's, Master's, PhD
Size → Small, Medium, Large
```

Machine Learning algorithms generally numerical input ke saath kaam karte hain. Isliye categorical values ko numerical representation mein convert karna padta hai.

Is process ko **Categorical Data Encoding** kehte hain.

Categorical data mainly do types ka hota hai:

1. **Ordinal Data**
2. **Nominal Data**

---

# 1. Ordinal Data

Ordinal data mein categories ke beech ek **meaningful order ya ranking** hoti hai.

Example:

```text
Small < Medium < Large
```

Yahan `Small`, `Medium` aur `Large` ke beech order important hai.

Similarly:

```text
Low < Medium < High
```

```text
Poor < Average < Good < Excellent
```

```text
School < Bachelor's < Master's < PhD
```

In sab mein categories ka order meaningful hai.

---

# 2. Ordinal Encoding

Ordinal Encoding mein hum ordered categories ko numbers mein convert karte hain.

Example:

```text
Small  → 0
Medium → 1
Large  → 2
```

Is case mein numerical values original ranking ko represent kar rahi hain.

### Python

```python
from sklearn.preprocessing import OrdinalEncoder

data = [
    ["Small"],
    ["Medium"],
    ["Large"],
    ["Medium"],
    ["Small"]
]

encoder = OrdinalEncoder(
    categories=[["Small", "Medium", "Large"]]
)

encoded_data = encoder.fit_transform(data)

print(encoded_data)
```

Output:

```text
[[0.]
 [1.]
 [2.]
 [1.]
 [0.]]
```

---

# 3. Why Custom Order is Important?

Ek common mistake ye hai ki categories ko automatically encode kar diya jaye.

Suppose:

```text
Low
Medium
High
```

Agar hum manually ya alphabetically encode karein, order galat represent ho sakta hai.

Isliye jab ordinal feature ho, **actual business/domain order explicitly define karna better practice hai**.

```python
encoder = OrdinalEncoder(
    categories=[["Low", "Medium", "High"]]
)
```

Yahan hum model ko clearly bata rahe hain:

```text
Low < Medium < High
```

---

# 4. Nominal Data

Nominal data mein categories ke beech **koi meaningful order nahi hota**.

Example:

```text
Red
Blue
Green
```

Colors ke beech koi ranking nahi hai.

Similarly:

```text
Delhi
Mumbai
Chennai
Kolkata
```

Cities ko kisi numerical order mein rakhna meaningful nahi hai.

---

# 5. One-Hot Encoding

Nominal data ko commonly **One-Hot Encoding** ke through encode kiya jata hai.

Suppose:

```text
Color
-----
Red
Blue
Green
Red
```

One-Hot Encoding ke baad:

| Red | Blue | Green |
| --: | ---: | ----: |
|   1 |    0 |     0 |
|   0 |    1 |     0 |
|   0 |    0 |     1 |
|   1 |    0 |     0 |

Yahan kisi category ko ranking nahi di gayi.

### Python

```python
from sklearn.preprocessing import OneHotEncoder

data = [
    ["Red"],
    ["Blue"],
    ["Green"],
    ["Red"]
]

encoder = OneHotEncoder(sparse_output=False)

encoded_data = encoder.fit_transform(data)

print(encoded_data)
```

---

# 6. Dummy Variable Trap

One-Hot Encoding ke baad agar `n` categories hain, theoretically `n` columns create ho sakte hain.

Example:

```text
Red
Blue
Green
```

Columns:

```text
Red
Blue
Green
```

Lekin linear regression jaise models mein ek column redundant ho sakta hai.

Isliye hum ek category drop kar sakte hain:

```python
encoder = OneHotEncoder(
    drop="first",
    sparse_output=False
)
```

Ab:

| Blue | Green |
| ---: | ----: |
|    0 |     0 |
|    1 |     0 |
|    0 |     1 |
|    0 |     0 |

Dropped category ko baaki columns se infer kiya ja sakta hai.

> Note: `drop="first"` har situation mein mandatory nahi hai. Modern models aur regularization ke saath full one-hot encoding bhi commonly use hoti hai.

---

# 7. Unknown Categories

Real-world Machine Learning mein ek important problem hoti hai: **unknown category**.

Suppose training data mein:

```text
Delhi
Mumbai
Noida
```

hain.

Lekin testing/production data mein:

```text
Delhi
Mumbai
Gurgaon
```

aa gaya.

`Gurgaon` training ke time encoder ne nahi dekha tha.

Aise case mein OneHotEncoder error de sakta hai.

Is problem ko handle karne ke liye:

```python
encoder = OneHotEncoder(
    handle_unknown="ignore",
    sparse_output=False
)
```

use kar sakte hain.

`handle_unknown="ignore"` unseen categories ko safely handle karne mein help karta hai.

---

# 8. Sparse Matrix Concept

One-Hot Encoding ke baad data mein bahut saare `0` ho sakte hain.

Example:

```text
Red    → 1 0 0 0 0 0
Blue   → 0 1 0 0 0 0
Green  → 0 0 1 0 0 0
```

Agar categories bahut zyada ho jaayein, to matrix bahut large ho sakti hai.

Example:

```text
100,000 rows
10,000 categories
```

Full dense matrix memory ke liye expensive ho sakti hai.

Isliye `OneHotEncoder` by default **sparse representation** use kar sakta hai.

```python
encoder = OneHotEncoder()
```

Agar specifically dense NumPy array chahiye:

```python
encoder = OneHotEncoder(sparse_output=False)
```

### Simple idea

```text
Dense Matrix
→ saare 0 explicitly store hote hain

Sparse Matrix
→ mostly non-zero values ko efficiently represent karti hai
```

---

# 9. High Cardinality

Jab kisi categorical feature mein bahut zyada unique categories hoti hain, usse **high-cardinality feature** kehte hain.

Example:

```text
City → thousands of cities
Product_ID → thousands/millions of products
User_ID → millions of users
```

Agar hum directly One-Hot Encoding karein:

```text
Product_ID
    ↓
10,000 unique values
    ↓
10,000 columns
```

Ye dimensionality bahut increase kar sakta hai.

Is situation mein alternatives consider kiye ja sakte hain:

* Target Encoding
* Frequency Encoding
* Count Encoding
* Hashing Encoding
* Feature Grouping
* Embeddings

Lekin in techniques ko carefully use karna chahiye, especially **target leakage** se bachne ke liye.

---

# 10. Target Encoding

Target Encoding mein category ko target variable ke statistical information se represent kiya jata hai.

Example:

Suppose:

```text
City       Average Salary
Delhi      70000
Mumbai     80000
Noida      60000
```

To city ko corresponding target statistic se represent kiya ja sakta hai.

Conceptually:

```text
Delhi  → 70000
Mumbai → 80000
Noida  → 60000
```

Ye high-cardinality features ke liye useful ho sakta hai.

### Important ⚠️

Target Encoding mein **data leakage** ka risk hota hai.

Isliye target statistics ko training data se carefully calculate karna chahiye, aur validation/test information ko training encoding mein use nahi karna chahiye.

Cross-validation based target encoding ek common approach hai.

---

# 11. Frequency / Count Encoding

Is technique mein category ko uski frequency ya count se replace karte hain.

Example:

```text
City
----
Delhi
Delhi
Mumbai
Delhi
Noida
Mumbai
```

Counts:

```text
Delhi  → 3
Mumbai → 2
Noida  → 1
```

Encoded data:

```text
Delhi  → 3
Delhi  → 3
Mumbai → 2
Delhi  → 3
Noida  → 1
Mumbai → 2
```

Ye simple hai aur high-cardinality categorical features ke liye useful ho sakta hai.

---

# 12. Ordinal Encoding vs Label Encoding

Dono ko beginners often confuse karte hain.

### Ordinal Encoding

Ordinal Encoding specifically **ordered categories** ke liye use ki ja sakti hai.

```text
Low    → 0
Medium → 1
High   → 2
```

### Label Encoding

Label Encoding commonly target labels/classes ko numerical values mein convert karne ke liye use hoti hai.

Example:

```text
Cat → 0
Dog → 1
Horse → 2
```

Yahan `0 < 1 < 2` ka numerical order classes ke beech meaningful ranking nahi batata.

Classification target ke context mein ye IDs ki tarah kaam karte hain.

> Input categorical features ke liye blindly `LabelEncoder` use karna generally correct approach nahi hai, especially nominal features mein.

---

# 13. Encoding Inside a Pipeline

Real Machine Learning projects mein preprocessing ko pipeline ke andar rakhna better practice hai.

Example:

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

categorical_features = ["Gender", "City"]

preprocessor = ColumnTransformer(
    transformers=[
        (
            "cat",
            OneHotEncoder(handle_unknown="ignore"),
            categorical_features
        )
    ]
)

model = Pipeline([
    ("preprocessor", preprocessor),
    ("classifier", LogisticRegression())
])

model.fit(X_train, y_train)
```

Is approach ka major benefit hai ki encoding aur model training ek proper workflow mein connected rehte hain.

---

# 14. Data Leakage and Encoding

Encoding karte time ek important rule:

> **Preprocessing ko training data par fit karo, test data par nahi.**

Wrong approach:

```python
encoder.fit(X)
```

Agar `X` mein training + test data dono included hain, to information leakage ho sakti hai.

Better approach:

```python
encoder.fit(X_train)

X_train_encoded = encoder.transform(X_train)
X_test_encoded = encoder.transform(X_test)
```

Ya `Pipeline` / `ColumnTransformer` use karo, jo workflow ko safer banata hai.

---

# 15. Tree Models vs Linear Models

Encoding ka effect model ke according different ho sakta hai.

### Linear Models

Linear Regression, Logistic Regression jaise models mein nominal categorical variables ke liye One-Hot Encoding commonly useful hoti hai.

### Tree-Based Models

Decision Trees, Random Forests aur Gradient Boosting models categorical features ko handle karne ke liye different encoding strategies use kar sakte hain, depending on the implementation.

Simple ordinal encoding ko nominal data par use karna carefully consider karna chahiye, kyunki kuch models numerical order ko split karte waqt interpret kar sakte hain.

---

# 16. Quick Decision Guide

Agar tumhare paas categorical column hai, pehle ye question pucho:

### Step 1

**Kya categories ke beech natural order hai?**

```text
Yes
 ↓
Ordinal Data
 ↓
Ordinal Encoding
```

Example:

```text
Low → Medium → High
```

### Step 2

Agar order nahi hai:

```text
No
 ↓
Nominal Data
 ↓
One-Hot Encoding
```

Example:

```text
Red | Blue | Green
```

### Step 3

Agar categories bahut zyada hain:

```text
High Cardinality
 ↓
One-Hot Encoding may create many columns
 ↓
Consider alternatives
```

For example:

```text
Target Encoding
Frequency Encoding
Hashing
Embeddings
```

---

# Ordinal vs Nominal — Final Comparison

| Property                | Ordinal              | Nominal                             |
| ----------------------- | -------------------- | ----------------------------------- |
| Categories              | Ordered              | Unordered                           |
| Ranking                 | Meaningful           | Not meaningful                      |
| Example                 | Small, Medium, Large | Red, Blue, Green                    |
| Common Encoding         | Ordinal Encoding     | One-Hot Encoding                    |
| Numerical Order         | Represents ranking   | Should not imply ranking            |
| Unknown Categories      | Need handling        | Need handling                       |
| High Cardinality        | Can still occur      | More problematic with One-Hot       |
| Common Advanced Options | Custom order         | Target/Frequency/Hashing/Embeddings |

---

# Easy Trick to Remember

```text
ORDINAL → ORDER
```

Agar category ka **order important hai**:

```text
Low → Medium → High
```

Use:

```text
Ordinal Encoding
```

And:

```text
NOMINAL → NAME
```

Agar categories sirf **different names/labels** hain:

```text
Red | Blue | Green
```

Use:

```text
One-Hot Encoding
```

---

# Key Takeaways

* Categorical data ko ML models ke liye numerical form mein convert karna padta hai.
* **Ordinal data** mein natural order hota hai.
* **Nominal data** mein natural order nahi hota.
* Ordinal data ke liye `OrdinalEncoder` useful hai.
* Nominal data ke liye `OneHotEncoder` commonly use hota hai.
* Ordinal categories ka order manually define karna important ho sakta hai.
* `handle_unknown="ignore"` unseen categories ko handle karne mein useful hai.
* One-Hot Encoding high-cardinality features mein bahut columns create kar sakti hai.
* High-cardinality ke liye Frequency, Target, Hashing ya Embedding based approaches consider ki ja sakti hain.
* Target Encoding mein leakage ka risk hota hai.
* Encoding ko `Pipeline` aur `ColumnTransformer` ke andar rakhna robust ML workflow banane mein help karta hai.
* Preprocessing ko **training data par fit** karna chahiye, test data par nahi.

## One-Line Summary

```text
Ordinal  → Order matters   → Ordinal Encoding

Nominal  → Order doesn't matter → One-Hot Encoding
```

