# 📊 Working with CSV Files using Pandas

Welcome! 👋

This repository contains beginner-friendly examples of **how to work with CSV/TSV files using Python and Pandas**.

The notebook `working_with_CSV(1).ipynb` explains different parameters of `pandas.read_csv()` and shows how they are useful when loading real-world datasets.

> **Goal:** After reading this README and running the notebook, a beginner should understand not only *how* to load a CSV file, but also *why* different `read_csv()` parameters are needed.

---

## 🧠 What is CSV?

**CSV** stands for **Comma-Separated Values**.

It is a simple file format used to store tabular data.

Example:

```text
name,age,city
Ayush,21,Delhi
Rahul,22,Noida
Aman,20,Lucknow
```

Here:

* `name`, `age`, `city` → column names
* Each line → one row
* `,` → separator/delimiter

CSV files are very common in:

* Data Science
* Machine Learning
* NLP
* Data Analysis
* Excel/data export
* Kaggle datasets

---

# 🐼 What is Pandas?

**Pandas** is a Python library used for working with structured/tabular data.

The main Pandas objects are:

### 1. DataFrame

A DataFrame looks like a table.

### 2. Series

A Series represents a single column of data.

---

# 📁 Notebook Topics

The notebook covers:

1. Import Pandas
2. Read a local CSV file
3. Read a CSV file from a URL
4. `sep`
5. `names`
6. `index_col`
7. `header`
8. `usecols`
9. Selecting a single column / Series
10. `skiprows`
11. `nrows`
12. `encoding`
13. Handling bad lines
14. `dtype`
15. Handling dates with `parse_dates`
16. `converters`
17. `na_values`
18. Loading large datasets in chunks

---

# 1️⃣ Import Pandas

```python
import pandas as pd
```

`pd` is the commonly used alias for Pandas.

Instead of:

```python
pandas.read_csv()
```

we can write:

```python
pd.read_csv()
```

---

# 2️⃣ Reading a Local CSV File

```python
df = pd.read_csv("aug_train.csv")
```

Here:

* `pd` → Pandas
* `read_csv()` → function used to read CSV files
* `"aug_train.csv"` → file name
* `df` → DataFrame containing the data

Basic workflow:

```text
CSV File
   ↓
pd.read_csv()
   ↓
DataFrame
   ↓
Analysis / Machine Learning
```

---

# 3️⃣ Reading a CSV from a URL

The notebook also demonstrates reading CSV data from an online URL.

```python
import requests
from io import StringIO
```

Then:

```python
req = requests.get(url)
data = StringIO(req.text)

df = pd.read_csv(data)
```

The flow is:

```text
URL
 ↓
requests.get()
 ↓
Text Data
 ↓
StringIO()
 ↓
pd.read_csv()
 ↓
DataFrame
```

---

# 4️⃣ `sep` Parameter

Not every file uses a comma as a separator.

For example, TSV files use a tab:

```python
pd.read_csv(
    "movie_titles_metadata.tsv",
    sep="\t"
)
```

Common separators:

```text
CSV → ,
TSV → \t
Other files → ;
```

`sep` tells Pandas **how columns are separated**.

---

# 5️⃣ `names` Parameter

Sometimes a dataset does not contain column names.

Example:

```text
101,Ayush,21
102,Rahul,22
103,Aman,20
```

We can provide our own names:

```python
pd.read_csv(
    "file.csv",
    names=["id", "name", "age"]
)
```

### Important

The correct parameter is:

```python
names
```

not:

```python
name
```

---

# 6️⃣ `index_col`

Pandas normally creates an index:

```text
0
1
2
3
```

A column can be used as the DataFrame index:

```python
pd.read_csv(
    "aug_train.csv",
    index_col="enrollee_id"
)
```

This is useful when the dataset already has a suitable identifier column.

---

# 7️⃣ `header`

Sometimes column names are not present on the first row.

Example:

```text
Some information
name,age,city
Ayush,21,Delhi
```

We can tell Pandas to use the second row as the header:

```python
pd.read_csv(
    "test.csv",
    header=1
)
```

Remember:

```text
0 → first row
1 → second row
2 → third row
```

Python uses zero-based indexing.

---

# 8️⃣ `usecols`

A dataset may contain many columns, but we may need only a few.

```python
pd.read_csv(
    "aug_train.csv",
    usecols=[
        "enrollee_id",
        "gender",
        "education_level"
    ]
)
```

This helps us:

* load only required columns
* reduce unnecessary data
* make analysis easier
* potentially reduce memory usage

---

# 9️⃣ Selecting One Column / Series

A single column can be converted into a Pandas Series:

```python
gender = pd.read_csv(
    "aug_train.csv",
    usecols=["gender"]
).squeeze("columns")
```

### DataFrame vs Series

```text
DataFrame
----------------
gender
Male
Female
Male
```

A Series represents one-dimensional data.

---

# 🔟 `skiprows`

Sometimes unwanted rows need to be skipped.

```python
pd.read_csv(
    "aug_train.csv",
    skiprows=[0, 1]
)
```

This skips rows `0` and `1`.

Useful when files contain extra information before the actual dataset.

---

# 1️⃣1️⃣ `nrows`

If you want to load only a limited number of rows:

```python
pd.read_csv(
    "aug_train.csv",
    nrows=30
)
```

This reads only the first 30 rows.

Useful for:

* testing
* quick exploration
* large datasets
* debugging

---

# 1️⃣2️⃣ `encoding`

Sometimes reading a CSV produces:

```text
UnicodeDecodeError
```

This can happen when the file uses a different character encoding.

Example:

```python
pd.read_csv(
    "Zomato.csv",
    encoding="latin-1"
)
```

Common encodings include:

```text
utf-8
latin-1
cp1252
```

The correct encoding depends on the actual file.

---

# 1️⃣3️⃣ Handling Bad Lines

Sometimes a CSV contains malformed rows.

For example:

```text
A,B,C,D
1,2,3,4
5,6,7
8,9,10,11
```

One row does not have the expected number of fields.

The notebook demonstrates:

```python
pd.read_csv(
    "BX-Book-Ratings.csv",
    sep=";",
    encoding="latin-1",
    on_bad_lines="skip"
)
```

`on_bad_lines="skip"` tells Pandas to skip problematic rows.

> ⚠️ Be careful: skipping bad rows can result in data loss. In a real project, investigate the malformed data before deciding to skip it.

---

# 1️⃣4️⃣ `dtype`

Pandas automatically detects data types.

You can inspect them using:

```python
df.info()
```

Examples:

```text
int
float
object/string
boolean
```

You can also specify a datatype:

```python
pd.read_csv(
    "aug_train.csv",
    dtype={"target": "int32"}
)
```

This can help with:

* controlling data types
* reducing memory usage
* avoiding unwanted type inference
* preparing data for Machine Learning

---

# 1️⃣5️⃣ `parse_dates`

Dates are often stored as text in CSV files.

Example:

```text
date
2020-01-01
2020-02-15
2020-03-20
```

The notebook uses:

```python
pd.read_csv(
    "IPL Matches 2008-2020.csv",
    parse_dates=["date"]
)
```

This tells Pandas to parse the `date` column as a datetime value.

This is useful for:

* extracting year/month/day
* sorting dates
* calculating date differences
* time-based analysis

---

# 1️⃣6️⃣ `converters`

`converters` allows us to apply a custom function to a column while loading the CSV.

For example:

```python
def rename(name):
    if name == "Royal Challengers Bangalore":
        return "RCB"
    if name == "Kolkata Knight Riders":
        return "KKR"
    if name == "Chennai Super Kings":
        return "CSK"
    if name == "Rajasthan Royals":
        return "RR"
    if name == "Mumbai Indians":
        return "MI"
    else:
        return name
```

Then:

```python
pd.read_csv(
    "IPL Matches 2008-2020.csv",
    converters={
        "team1": rename,
        "team2": rename,
        "toss_winner": rename,
        "winner": rename
    }
)
```

The process is:

```text
Royal Challengers Bangalore
            ↓
         rename()
            ↓
           RCB
```

---

# 1️⃣7️⃣ `na_values`

Real-world datasets can represent missing values using:

```text
-
NA
N/A
?
unknown
```

We can tell Pandas which values should be treated as missing:

```python
pd.read_csv(
    "file.csv",
    na_values=["-"]
)
```

After loading, missing values can be checked using:

```python
df.isnull()
```

and handled using methods such as:

```python
df.fillna(...)
```

or:

```python
df.dropna(...)
```

---

# 1️⃣8️⃣ Loading Huge Datasets with `chunksize`

Very large datasets may not fit comfortably into memory.

Pandas allows us to process them in chunks:

```python
for chunk in pd.read_csv(
    "large_file.csv",
    chunksize=10000
):
    print(chunk.shape)
```

Instead of:

```text
Entire Dataset → RAM
```

we process:

```text
10,000 rows
     ↓
process
     ↓
next 10,000 rows
     ↓
process
     ↓
...
```

This is useful for large datasets.

---

# 🗺️ Complete `read_csv()` Mental Model

```text
                         CSV / TSV File
                               │
                               ▼
                       pd.read_csv()
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
      sep                  encoding                header
        │                      │                      │
  column separator        character format       column names
        │                      │                      │
        └──────────────────────┼──────────────────────┘
                               ▼
                         DataFrame
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
       usecols              dtype             parse_dates
          │                    │                    │
   select columns        set datatype          parse dates
          │                    │                    │
          └────────────────────┼────────────────────┘
                               ▼
                         Clean Data
                               │
                               ▼
                    Analysis / Machine Learning
```

---

# 🔥 `read_csv()` Cheat Sheet

| Parameter      | Purpose                | Example                    |
| -------------- | ---------------------- | -------------------------- |
| `sep`          | Column separator       | `sep="\t"`                 |
| `names`        | Custom column names    | `names=["id","name"]`      |
| `index_col`    | Set index column       | `index_col="id"`           |
| `header`       | Select header row      | `header=1`                 |
| `usecols`      | Select columns         | `usecols=["name","age"]`   |
| `skiprows`     | Skip rows              | `skiprows=[0,1]`           |
| `nrows`        | Read limited rows      | `nrows=100`                |
| `encoding`     | Character encoding     | `encoding="latin-1"`       |
| `on_bad_lines` | Handle malformed rows  | `on_bad_lines="skip"`      |
| `dtype`        | Set data types         | `dtype={"age":"int32"}`    |
| `parse_dates`  | Parse date columns     | `parse_dates=["date"]`     |
| `converters`   | Apply custom functions | `converters={"name":func}` |
| `na_values`    | Define missing values  | `na_values=["-"]`          |
| `chunksize`    | Read in chunks         | `chunksize=10000`          |

---

# 🧪 Recommended Learning Order

If you are a beginner, learn these in this order:

### Level 1 — Basic

```python
pd.read_csv()
```

### Level 2 — Understand the File

```python
sep
names
header
index_col
```

### Level 3 — Select Data

```python
usecols
nrows
skiprows
```

### Level 4 — Handle Real-World Problems

```python
encoding
on_bad_lines
dtype
na_values
```

### Level 5 — Transform Data

```python
parse_dates
converters
```

### Level 6 — Large Datasets

```python
chunksize
```

---

# 💻 Requirements

Install Pandas:

```bash
pip install pandas
```

For the URL example:

```bash
pip install requests
```

You can run the notebook using:

* Jupyter Notebook
* JupyterLab
* Google Colab
* VS Code with Jupyter

---

# 📂 Project Structure

```text
Working-with-CSV/
│
├── working_with_CSV(1).ipynb
├── README.md
│
└── datasets/
    ├── aug_train.csv
    ├── movie_titles_metadata.tsv
    ├── IPL Matches 2008-2020.csv
    └── ...
```

> Dataset files are not necessarily included with the notebook. If you run the notebook locally, make sure the required dataset paths/files are available.

---

# 🎯 What You Will Learn

After completing this notebook, you should be able to:

* Understand CSV and TSV files
* Load datasets using Pandas
* Load CSV files from URLs
* Understand separators
* Create custom column names
* Select an index column
* Select specific columns
* Skip rows
* Limit the number of rows
* Handle encoding issues
* Handle malformed CSV rows
* Control column datatypes
* Parse date columns
* Transform values while loading data
* Define missing-value markers
* Work with large datasets using chunks

---

# 🚀 Why This Matters for Machine Learning

Before training a Machine Learning model, we first need to load and prepare our dataset.

A typical workflow is:

```text
Raw Dataset
     ↓
Load Data
     ↓
Inspect Data
     ↓
Handle Missing Values
     ↓
Fix Data Types
     ↓
Clean / Transform Data
     ↓
Feature Engineering
     ↓
Train ML Model
     ↓
Evaluate Model
```

`pd.read_csv()` is often one of the **first steps** in this pipeline.

That makes it an important foundation for:

* Data Science
* Machine Learning
* NLP
* Deep Learning
* Data Analytics

---

# 📚 Key Takeaway

The main lesson of this notebook is:

> **A real-world dataset is not always perfectly formatted. Pandas `read_csv()` provides parameters that control how the data is loaded and interpreted.**

Don't try to memorize every parameter.

Instead, understand **what problem each parameter solves**.

```text
Wrong separator?
        → sep

Wrong/missing column names?
        → names / header

Need a particular index?
        → index_col

Need only some columns?
        → usecols

Need only a few rows?
        → nrows

Need to skip rows?
        → skiprows

Encoding error?
        → encoding

Bad/malformed rows?
        → on_bad_lines

Wrong datatype?
        → dtype

Date stored as text?
        → parse_dates

Need transformation during loading?
        → converters

Custom missing-value markers?
        → na_values

Huge dataset?
        → chunksize
```

---

## ⭐ Keep Learning. Keep Building. 🚀

```text
Learn → Practice → Experiment → Build → Repeat
```
## 👨‍💻 Author

**Ayush Pandey**

B.Tech Student | Data Science & Machine Learning Enthusiast

---

⭐ If you found this repository helpful, consider giving it a **Star** on GitHub!

**Keep Learning. Keep Building. 🚀**

