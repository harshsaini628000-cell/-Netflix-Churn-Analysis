# -Netflix-Churn-Analysis
```python

readme_content = """# 📊 Netflix Churn Analysis & Customer Engagement EDA

A complete end-to-end Data Analysis project executed in **Google Colab** using **Python, Pandas, NumPy, Matplotlib, and Seaborn**.

---

## 📌 Project Summary

Customer retention is vital for streaming services like Netflix. This project evaluates customer behavioral data, subscription choices, financial contributions, and service interaction to identify at-risk subscribers and quantify lost revenue due to customer churn.

---

## 📁 Dataset Details

- **File**: `netflix_churn_dataset.csv`
- **Total Records**: 4,000 customers
- **Total Features**: 18 columns
- **Missing Values**: 0 nulls across all features

| Feature | Type | Description |
| :--- | :--- | :--- |
| `customer_id` | Object | Unique identifier for each subscriber |
| `age` | Integer | Customer age (13 to 74 years) |
| `gender` | Object | Female, Male, Other |
| `country` | Object | Subscriber country (USA, UK, Canada, Australia, etc.) |
| `subscription_type` | Object | Tier: `Basic`, `Standard`, `Premium` |
| `monthly_charges` | Integer | Monthly subscription fee |
| `tenure_months` | Integer | Account age in months (1 to 60) |
| `num_profiles` | Integer | Number of user profiles per account (1 to 5) |
| `device` | Object | Device used (Smart TV, Mobile, Laptop, Tablet, Gaming Console) |
| `payment_method` | Object | Debit Card, Credit Card, PayPal, UPI, Net Banking |
| `autopay_enabled` | Object | Autopay setup (`Yes` / `No`) |
| `watch_hours_per_week`| Float | Weekly streaming hours (0.0 to 60.0) |
| `favorite_genre` | Object | Top preferred content genre |
| `avg_rating_given` | Float | Average rating score given to titles (1.0 to 5.0) |
| `support_tickets_raised`| Integer | Support tickets opened (0 to 5) |
| `last_login_days_ago` | Integer | Days since the last login (0 to 89 days) |
| `promo_used` | Object | Promo coupon used at signup (`Yes` / `No`) |
| `churn` | Binary / Object| Retention label (`Yes`/`1` = Churned, `No`/`0` = Stayed) |

---

## 🔍 Key Findings

1. **Overall Churn Rate**: 
   - **801 users (20.03%)** churned.
   - **3,199 users (79.97%)** remained active.
2. **Subscription Tier Attrition**:
   - `Basic`: 319 churned users (Highest)
   - `Standard`: 288 churned users
   - `Premium`: 194 churned users
3. **Engagement Gap**:
   - Active users watched an average of **8.17 hours/week**.
   - Churned users watched an average of **6.64 hours/week**.
4. **Financial Impact**:
   - Cumulative historical lost revenue from churned customers reached **$10,009,304.00**.
5. **High-Risk Segment**:
   - **76 active users** have been flagged as immediate churn risks (`last_login_days_ago > 30`, `watch_hours_per_week < 5`, and `support_tickets_raised >= 2`).

---

## 🛠️ Tech Stack

- **Environment**: Google Colaboratory
- **Data Manipulation**: Pandas, NumPy
- **Data Visualization**: Matplotlib, Seaborn

---

## 🚀 How to Run in Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Create a new notebook or upload `netflix_churn.ipynb`.
3. Upload `netflix_churn_dataset.csv` using the file uploader prompt.
4. Run all notebook cells from top to bottom.
"""

with open("README.md", "w") as f:
    f.write(readme_content)

print("Enhanced README.md generated successfully!")


```

```text
Enhanced README.md generated successfully!


```

### 📖 Line-by-Line Explanation of  Colab Notebook (`netflix_churn.ipynb`)

Here is the breakdown of every code cell and line executed in your notebook:

---

#### **Cell 1: File Uploading via Colab Utility**

```python
from google.colab import files

uploaded = files.upload()

```

* **`from google.colab import files`**: Imports the Colab-specific utility module for managing file transfers between your local machine and the Colab virtual machine.
* **`uploaded = files.upload()`**: Launches an interactive browser upload prompt allowing you to upload `netflix_churn_dataset.csv` directly into the current session.

---

#### **Cell 2: Library Imports**

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

```

* **`import pandas as pd`**: Imports the Pandas library with alias `pd` for tabular data structures and DataFrame manipulation.
* **`import numpy as np`**: Imports NumPy as `np` for numerical array calculations and conditional logic.
* **`import matplotlib.pyplot as plt`**: Imports Matplotlib’s plotting interface for building figures and charts.
* **`import seaborn as sns`**: Imports Seaborn for statistical data visualization on top of Matplotlib.

---

#### **Cell 3: Loading and Inspecting the CSV**

```python
df = pd.read_csv("netflix_churn_dataset.csv")

df.head()

```

* **`df = pd.read_csv("netflix_churn_dataset.csv")`**: Reads the uploaded CSV file from the disk into a 2D tabular DataFrame named `df`.
* **`df.head()`**: Displays the first 5 records of the dataset to preview column names and sample values.

---

#### **Cell 4: Checking Structure and Data Types**

```python
df.info()

```

* **`df.info()`**: Prints a summary of the DataFrame: the total row count (4,000 entries), column names (18 columns), non-null counts, memory footprint (~562.6 KB), and data types (`int64`, `float64`, `object`).

---

#### **Cell 5: Checking for Null Values**

```python
df.isnull().sum()

```

* **`df.isnull()`**: Returns a DataFrame of boolean flags (`True` if a value is missing/NaN, `False` otherwise).
* **`.sum()`**: Sums all `True` values along each column. All columns returned `0`, confirming the dataset has zero missing values.

---

#### **Cell 6: Inspecting Top 2 Rows**

```python
df.head(2)

```

* Displays the first 2 rows of `df` to verify column names prior to performing churn value counts.

---

#### **Cell 7: Counting Churned vs. Active Users**

```python
churned = df['churn'].value_counts()['Yes']
stayed = df['churn'].value_counts()['No']

print(f'Number of customers who churned: {churned}')
print(f'Number of customers who stayed: {stayed}')

```

* **`df['churn'].value_counts()`**: Counts the frequency of each unique category in the `churn` column (`'Yes'` and `'No'`).
* **`['Yes']` / `['No']**`: Extracts the specific count for churned customers (801) and retained customers (3,199).
* **`print(...)`**: Prints both counts using Python f-strings.

---

#### **Cell 8: Subscription Tier with the Highest Churn**

```python
churn_counts_by_type = df.groupby('subscription_type')['churn'].value_counts()
churned_users_by_type = churn_counts_by_type.xs('Yes', level='churn')
highest_churn_subscription = churned_users_by_type.idxmax()

print("Number of churned users by subscription type:")
print(churned_users_by_type)
print(f"\nThe subscription tier with the highest number of churned users is: {highest_churn_subscription}")

```

* **`df.groupby('subscription_type')['churn'].value_counts()`**: Groups data by plan tier (`Basic`, `Standard`, `Premium`) and tabulates `'Yes'` / `'No'` counts within each group.
* **`.xs('Yes', level='churn')`**: Performs cross-section slicing to isolate only `'Yes'` (churned) rows across each plan tier.
* **`.idxmax()`**: Returns the index label holding the maximum churn count (`Basic` with 319 users).

---

#### **Cell 9: Viewing Unique Values in Churn**

```python
print(df['churn'].unique())

```

* **`df['churn'].unique()`**: Returns `['No', 'Yes']`, confirming there are only two string categories in the target column.

---

#### **Cell 10: Binary Encoding of the Target Column**

```python
df['churn'] = df['churn'].map({'Yes': 1, 'No': 0})

```

* **`.map({'Yes': 1, 'No': 0})`**: Replaces the string values with numerical equivalents (`'Yes'` $\rightarrow$ `1`, `'No'` $\rightarrow$ `0`) so they can be processed mathematically and modeled easily.

---

#### **Cell 11: Verifying Binary Distribution**

```python
print(df['churn'].value_counts(dropna=False))

```

* Checks the value counts of `df['churn']` to ensure no `NaN` values were created during the mapping step (outputs `0: 3199`, `1: 801`).

---

#### **Cell 12: Comparing Group Means**

```python
average_monthly_charges = df.groupby('churn')['monthly_charges'].mean()
average_watch_hours_per_week = df.groupby('churn')['watch_hours_per_week'].mean()

print("Average monthly charges:")
print("Active users:", round(average_monthly_charges[0], 2))
print("Churned users:", round(average_monthly_charges[1], 2))

print("\nAverage watch hours per week:")
print("Active users:", round(average_watch_hours_per_week[0], 2))
print("Churned users:", round(average_watch_hours_per_week[1], 2))

```

* **`df.groupby('churn')['monthly_charges'].mean()`**: Calculates the mean monthly fee for active (`0`) versus churned (`1`) users ($434.47 vs. $415.27).
* **`df.groupby('churn')['watch_hours_per_week'].mean()`**: Calculates the mean weekly streaming hours (Active: 8.17 hrs vs. Churned: 6.64 hrs).
* **`round(..., 2)`**: Rounds the calculated floats to two decimal places.

---

#### **Cell 13: Filtering Inactive Logins (> 30 Days)**

```python
df_last_login_days = df[df['last_login_days_ago'] > 30]
df_last_login_days

```

* **`df['last_login_days_ago'] > 30`**: Generates a boolean mask selecting users who haven't logged in for over a month.
* **`df[...]`**: Filters the DataFrame down to 2,600 matching records.

---

#### **Cell 14: Multi-Column Group Aggregation**

```python
avg_values = df.groupby('churn')[['monthly_charges', 'watch_hours_per_week']].mean()

print(round(avg_values), 1)

```

* **`df.groupby('churn')[['monthly_charges', 'watch_hours_per_week']].mean()`**: Computes the mean for both engagement and cost columns simultaneously grouped by churn status.

---

#### **Cell 15: Watch Hours Summary Statistics (NumPy)**

```python
minimum_value = np.min(df['watch_hours_per_week'])
maximum_value = np.max(df['watch_hours_per_week'])
median_value = np.median(df['watch_hours_per_week'])

print(f"Minimum value: {minimum_value}")
print(f"Maximum value: {maximum_value}")
print(f"Median value: {median_value}")

```

* **`np.min(...)`**: Finds the minimum watch time (`0.0` hours).
* **`np.max(...)`**: Finds the maximum watch time (`60.0` hours).
* **`np.median(...)`**: Finds the 50th percentile / median value (`5.5` hours).

---

#### **Cell 16: Flagging High-Risk Active Users**

```python
high_risk_active_users = df[
    (df['last_login_days_ago'] > 30) &
    (df['watch_hours_per_week'] < 5) &
    (df['churn'] == 0) &
    (df['support_tickets_raised'] >= 2)
]

high_risk_active_users

```

* Combines four boolean conditions with the bitwise AND operator (`&`):
1. Haven't logged in for over 30 days (`last_login_days_ago > 30`)
2. Low streaming engagement (`watch_hours_per_week < 5`)
3. Still currently active (`churn == 0`)
4. Encountered multiple service issues (`support_tickets_raised >= 2`)


* Filters out **76 active users** in critical danger of churning.

---

#### **Cell 17: Calculating Total Lost Revenue**

```python
total_lost_revenue = df[df['churn'] == 1]['monthly_charges'] * df[df['churn'] == 1]['tenure_months']
total_lost_revenue = total_lost_revenue.sum()

print(f"Total lost revenue by churned customers: ${total_lost_revenue:.2f}")

```

* **`df[df['churn'] == 1]['monthly_charges'] * df[df['churn'] == 1]['tenure_months']`**: Calculates historical lifetime revenue generated prior to cancellation for each churned user.
* **`.sum()`**: Aggregates the total historical revenue lost to churn ($10,009,304.00).

---

#### **Cell 18: Assigning Customer Risk Tiers (`np.select`)**

```python
risk_tiers = [
    (df['last_login_days_ago'] > 30) & (df['watch_hours_per_week'] < 5),
    (df['last_login_days_ago'] <= 30) & (df['watch_hours_per_week'] >= 10),
    (df['last_login_days_ago'] <= 30) & (df['watch_hours_per_week'] < 10)
]

risk_tier_labels = ['Low', 'Medium', 'High']

df['risk_tier'] = np.select(risk_tiers, risk_tier_labels, default='Unknown')

df

```

* **`risk_tiers`**: A list of conditional expressions based on recency and watch volume.
* **`risk_tier_labels`**: Corresponding labels mapped to each condition.
* **`np.select(...)`**: Applies the conditions element-wise across the series, assigning `'Unknown'` if no conditions are met.

---

#### **Cell 19: Calculating Total Lifetime Spend**

```python
total_lifetime_spend = df['monthly_charges'] * df['tenure_months']

df['total_lifetime_spend'] = total_lifetime_spend
df

```

* Multiplies monthly subscription fees by subscription tenure in months and assigns the result to a new column `total_lifetime_spend`.

---

#### **Cell 20: Engagement Percentiles**

```python
percentile_25th = np.percentile(df['watch_hours_per_week'], 25)
percentile_50th = np.percentile(df['watch_hours_per_week'], 50)
percentile_75th = np.percentile(df['watch_hours_per_week'], 75)
percentile_95th = np.percentile(df['watch_hours_per_week'], 95)

print(f"25th percentile: {percentile_25th}")
print(f"50th percentile: {percentile_50th}")
print(f"75th percentile: {percentile_75th}")
print(f"95th percentile: {percentile_95th}")

```

* Uses `np.percentile(...)` to measure the spread of watch hours across the user base (25th: 2.2 hrs, 50th: 5.5 hrs, 75th: 10.9 hrs, 95th: 23.7 hrs).

---

#### **Cell 21: Customer Distribution by Country (Matplotlib)**

```python
plt.figure(figsize=(10, 5))

plt.bar(df['country'].value_counts().index,
        df['country'].value_counts().values,
        color='purple',
        edgecolor='black')

plt.xlabel('Country')
plt.ylabel('Count')
plt.title('Count of Customers in Each Country')

plt.show()

```

* **`plt.figure(figsize=(10, 5))`**: Initializes a canvas 10 inches wide by 5 inches tall.
* **`plt.bar(..., color='purple', edgecolor='black')`**: Draws a bar chart showing the frequency of subscribers across all registered countries.
* **`plt.xlabel(...)`, `plt.ylabel(...)`, `plt.title(...)**`: Sets axis labels and main title.
* **`plt.show()`**: Renders the plot.

---

#### **Cell 22: Churn by Device (Matplotlib)**

```python
churned_by_device = df[df['churn'] == 1]['device'].value_counts()

plt.figure(figsize=(10, 5))

plt.bar(
    churned_by_device.index,
    churned_by_device.values,
    color='skyblue',
    edgecolor='black'
)

plt.xlabel('Device')
plt.ylabel('Churned Customers')
plt.title('Churn Counts Across Different Devices')

plt.show()

```

* Filters only churned customers (`df['churn'] == 1`) and creates a bar chart comparing cancellation counts across device types (Smart TV, Mobile, Laptop, etc.).

---

#### **Cell 23: Age Distribution Histogram (Matplotlib)**

```python
plt.figure(figsize=(10, 5))

plt.hist(
    df['age'],
    bins=20,
    color='cyan',
    edgecolor='black'
)

plt.xlabel('Age')
plt.ylabel('Frequency')
plt.title('Distribution of User Age')

plt.show()

```

* **`plt.hist(df['age'], bins=20, ...)`**: Divides subscriber ages into 20 equal-width bins to observe demographic distribution.

---

#### **Cell 24: $2 \times 2$ Subplot Distribution Grid (Matplotlib)**

```python
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

axes[0, 0].hist(df['age'], bins=20, color='cyan', edgecolor='black')
axes[0, 0].set_xlabel('Age')
axes[0, 0].set_ylabel('Frequency')
axes[0, 0].set_title('Distribution of User Age')

axes[0, 1].hist(df['monthly_charges'], bins=20, color='cyan', edgecolor='black')
axes[0, 1].set_xlabel('Monthly Charges')
axes[0, 1].set_ylabel('Frequency')
axes[0, 1].set_title('Distribution of Monthly Charges')

axes[1, 0].hist(df['tenure_months'], bins=20, color='cyan', edgecolor='black')
axes[1, 0].set_xlabel('Tenure (Months)')
axes[1, 0].set_ylabel('Frequency')
axes[1, 0].set_title('Distribution of Tenure')

axes[1, 1].hist(df['watch_hours_per_week'], bins=20, color='cyan', edgecolor='black')
axes[1, 1].set_xlabel('Watch Hours per Week')
axes[1, 1].set_ylabel('Frequency')
axes[1, 1].set_title('Distribution of Watch Hours per Week')

plt.tight_layout()
plt.show()

```

* **`plt.subplots(2, 2, figsize=(15, 10))`**: Creates a $2 \times 2$ figure grid yielding 4 distinct subplot axes (`axes[row, col]`).
* Populates each subplot with a histogram of a key numeric variable (`age`, `monthly_charges`, `tenure_months`, `watch_hours_per_week`).
* **`plt.tight_layout()`**: Automatically adjusts padding between subplots to prevent overlapping titles and labels.

---

#### **Cell 25: Churn Balance (Seaborn)**

```python
plt.figure(figsize=(10, 5))
sns.countplot(x='churn', data=df, palette={'0': 'green', '1': 'red'}, legend=False)

plt.xlabel('Churn')
plt.ylabel('Count')
plt.title('Churn Balance')
plt.xticks([0, 1], ['Active', 'Churned'])
plt.show()

```

* **`sns.countplot(x='churn', data=df, ...)`**: Plots a count bar graph comparing active users (`0`, green) and churned users (`1`, red).
* **`plt.xticks([0, 1], ['Active', 'Churned'])`**: Replaces tick labels `0` and `1` with descriptive text.

---

#### **Cell 26: Subscription Tier vs. Churn (Seaborn)**

```python
plt.figure(figsize=(10, 5))

sns.countplot(
    data=df,
    x='subscription_type',
    hue='churn',
    palette={0: 'green', 1: 'red'}
)

plt.xlabel('Subscription Type')
plt.ylabel('Number of Customers')
plt.title('Subscription Type vs Churn')

plt.legend(title='Churn', labels=['Active', 'Churned'])

plt.show()

```

* **`hue='churn'`**: Splits each subscription tier (`Basic`, `Standard`, `Premium`) into side-by-side comparative bars for active vs. churned users.
* **`plt.legend(...)`**: Configures the legend to clearly denote green as "Active" and red as "Churned".




---

---

## 🚀 How to Run in Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Create a new notebook or upload `netflix_churn.ipynb`.
3. Upload `netflix_churn_dataset.csv` using the file uploader prompt.
4. Run all notebook cells from top to bottom.
