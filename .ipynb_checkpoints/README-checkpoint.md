# Customer Churn Prediction

This is a term project for the Introduction to Machine Learning course. The goal is to predict customer churn using the IBM Telco Customer Churn dataset. We trained four different machine learning models and compared their results: Logistic Regression, Decision Tree, Random Forest, and XGBoost.

## Team Members

- Ruken Yıldız — 042301180
- Ece Oğuzbal — 042101084
- Alp Karaman — 042301182
- Kutay Demir — 042301072

Prof. Dr. Yassine Drias — MEF University, 2026

## About the Dataset

- **Source:** IBM Telco Customer Churn Dataset
- **Size:** 7043 customers, 33 columns
- **Target column:** `Churn Label` (Yes or No)
- **File name:** `Telco_customer_churn.xlsx`

The dataset includes customer details such as demographics, the services they use, billing information, and whether they left the company or not.

## Requirements

You need Python 3.8 or higher. The following libraries are also needed:

```
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
imbalanced-learn
openpyxl
```

You can install them with this command:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost imbalanced-learn openpyxl
```

## How to Run

1. Download or clone this project.
2. Put the `Telco_customer_churn.xlsx` file in the same folder as `project.ipynb`.
3. Open Jupyter Notebook:

```bash
jupyter notebook project.ipynb
```

4. Run the cells one by one from top to bottom.

## Project Steps

### 1. Importing Libraries
We import all the libraries we need for the project, such as Pandas, NumPy, Matplotlib, Seaborn, scikit-learn, XGBoost, and imbalanced-learn.

### 2. Loading the Dataset
We load the Excel file and look at the shape, data types, missing values, and the class distribution. We see that the dataset is not balanced: 5174 customers stayed and only 1869 left.

In this part, we also create four plots to understand the data better:
- The number of customers who left and stayed (bar chart)
- Churn rate for each contract type (bar chart)
- Monthly charges compared to churn (boxplot)
- Tenure compared to churn (boxplot)

### 3. Preprocessing
- We remove columns that are not useful or can cause data leakage. These columns are `CustomerID`, `Count`, `Country`, `State`, `City`, `Zip Code`, `Lat Long`, `Latitude`, `Longitude`, `Churn Score`, `CLTV`, `Churn Reason`, and `Churn Value`.
- The `Total Charges` column has the wrong data type (object), so we change it to a number with `pd.to_numeric`. Empty values are filled with the median.
- We change the target column `Churn Label` from text to numbers. "Yes" becomes 1 and "No" becomes 0.
- We use `LabelEncoder` to turn other text columns into numbers.

### 4. SMOTE and Train/Test Split
- We split the data into 80% for training and 20% for testing. We use `stratify=y` to keep the same class ratio in both sets.
- The training data is not balanced, so we apply **SMOTE** only to the training set. SMOTE creates new examples for the smaller class.
- We use `StandardScaler` to scale the features. This is important for models like Logistic Regression.

### 5. Models
We train four models with the same data and test them on the same test set:

| Model | Main Parameters |
| --- | --- |
| Logistic Regression | `max_iter=1000`, `random_state=42` |
| Random Forest | `n_estimators=100`, `random_state=42` |
| XGBoost | `eval_metric='logloss'`, `random_state=42` |
| Decision Tree | `max_depth=6`, `class_weight='balanced'`, `random_state=42` |

### 6. Evaluation
We use these metrics to compare the models: Accuracy, Precision, Recall, F1-Score, MCC, ROC-AUC, and Error Rate.

We also create these plots to compare the models:
- The 10 most important features (from Random Forest)
- A bar chart that compares all metrics
- Confusion matrices for each model
- ROC curves for all four models on the same plot

## Results

| Model | Accuracy | Precision | Recall | F1-Score | MCC | ROC-AUC | Error Rate |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Logistic Regression | 0.7637 | 0.5404 | 0.7326 | 0.6220 | 0.4669 | 0.8346 | 0.2363 |
| Random Forest | 0.7977 | 0.6127 | 0.6471 | 0.6294 | 0.4908 | 0.8374 | 0.2023 |
| XGBoost | 0.7750 | 0.5718 | 0.6070 | 0.5888 | 0.4345 | 0.8302 | 0.2250 |
| Decision Tree | 0.7544 | 0.5278 | 0.7112 | 0.6059 | 0.4434 | 0.8077 | 0.2456 |

## Business Point of View

For churn prediction, **Recall** is the most important metric. If we miss a customer who is going to leave (False Negative), we lose that customer and all their future income. But if we predict that a customer will leave when they actually want to stay (False Positive), we only spend a small amount of money on a retention offer.

For this reason:
- For catching as many real churners as possible, **Logistic Regression** is the best choice because it has the highest Recall (0.7326).
- For a balanced performance, **Random Forest** is the best because it has the highest F1-Score, MCC, and ROC-AUC.
- If we need to explain the model to other people, **Decision Tree** is easier to understand.

The customers with the highest risk of leaving are the ones with month-to-month contracts, high monthly charges, and short tenure (less than six months).

## File Structure

```
.
├── project.ipynb                 # The main notebook
├── Telco_customer_churn.xlsx     # The dataset (download separately)
└── README.md
```

## Notes

- We apply SMOTE only to the training set. The test set keeps its original class ratio.
- We use `random_state=42` for all models so that the results can be repeated.
- We remove `Churn Score`, `CLTV`, and `Churn Reason` because they are directly connected to the target and would cause data leakage.
