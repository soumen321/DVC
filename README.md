# DVC + S3 + GIT

Git is not good for data versioning

- Large file
- Slow
- In effective cost solution

## Wine Prediction


Setup DVC for Wine Predection Dataset.
Use S3 as Remote clous storage service and Git to track pointers to Dataset.


# 📘 Dataset Versioning with DVC, Git & AWS S3
*A Practical & Story‑Driven Guide*

Machine Learning projects often involve **large datasets** that cannot be efficiently stored in Git. While Git is great for versioning code, it is not designed to handle big data files.

This is where **DVC (Data Version Control)** becomes essential.

DVC helps you version datasets just like code — but stores the actual data in remote storage such as **AWS S3**, while Git stores only metadata.

This README explains step‑by‑step how to set up, track, version, and share datasets using DVC.

---

## 🚀 Why Use DVC?

- Git cannot store large datasets efficiently  
- DVC tracks datasets using lightweight `.dvc` files  
- Actual data stays in **cloud storage (e.g., S3)**  
- Teams can sync datasets easily  
- Reproducibility becomes effortless  
- Perfect for ML/AI workflows  

---

# 🛠️ Setup Instructions

## 1️⃣ Create and Activate a Virtual Environment

```bash
python -m venv .venv
venv/Scripts/activate    # For Windows
```

---

## 2️⃣ Install DVC

```bash
pip install dvc
```

---

## 3️⃣ Initialize DVC in Your Project

```bash
dvc init
```

This creates a `.dvc/` folder similar to how Git initializes `.git/`.

---

# 📂 Tracking a Dataset with DVC

Assume your dataset is located at:

```
./data/wine_sample.csv
```

Add it to DVC:

```bash
dvc add .\data\wine_sample.csv
```

This generates:

- A `.csv.dvc` file → Contains metadata + checksum  
- Moves the actual CSV into DVC cache  

✔️ **Git stores the `.dvc` file**  
✔️ **The large CSV file stays outside Git**

---

# ☁️ Configure AWS S3 as Remote Storage

Before connecting DVC to S3:

```bash
aws configure
```

Install DVC S3 support:

```bash
pip install dvc-s3
```

Add your S3 bucket as the default remote:

```bash
dvc remote add -d wineremote s3://<bucket name>
```

Verify remote config in:

```
.dvc/config
```

Example:

```
[core]
    remote = wineremote
['remote "wineremote"']
    url = s3://<bucket name>
```

---

# 📤 Push Dataset to S3

Upload the dataset to the S3 remote storage:

```bash
dvc push
```

DVC uploads the actual CSV file to S3.

---

# 🔁 Versioning Datasets

Whenever you modify the dataset:

### 1. Add updated dataset again
```bash
dvc add .\data\wine_sample.csv
```

### 2. Add metadata to Git
```bash
git add .\data\wine_sample.csv.dvc
git commit -m "Updated dataset version"
```

### 3. Push new dataset version to S3
```bash
dvc push
```

⭐ Git stores version history  
⭐ S3 stores dataset versions  
⭐ DVC connects them together

---

# 👥 How Others Access the Dataset (e.g., College/Team)

When someone clones your repo:

```bash
git clone <repo-url>
cd <project-folder>
```

Then they can download the exact dataset version by running:

```bash
dvc pull
```

DVC reads:

- `.dvc` metadata  
- S3 remote URL  
- checksum  

And fetches the correct dataset version automatically.

---

# 📦 What Goes Where?

| Storage | What It Holds |
|--------|----------------|
| **Git** | Code + `.dvc` metadata |
| **S3** | Actual dataset files |
| **Local Cache** | Temporary dataset storage for DVC |

Together, they form the complete versioning system.

---

# 🏁 Summary

DVC + Git + S3 provides:

✔️ Reproducible dataset versioning  
✔️ Lightweight Git repository  
✔️ Full history of dataset changes  
✔️ Easy sharing with teams  
✔️ A professional ML workflow  

Your Git repo becomes the **source of truth**, storing only metadata.  
Your S3 bucket becomes the **data warehouse** storing real datasets.

---  


