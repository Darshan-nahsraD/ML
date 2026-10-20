# Pandas Notes

Quick revision notes with practical examples and expected outputs. Revise Pandas without watching long videos.

```python
import numpy as np
import pandas as pd
```

## 1. Series

A Series is a one-dimensional labeled array.

### Creating a Series

```python
labels = ['a', 'b', 'c']
my_list = [10, 20, 30]

print(pd.Series(my_list))
print(pd.Series(my_list, index=labels))
print(pd.Series({'a': 10, 'b': 20, 'c': 30}))
```

**Output**
```text
0    10
1    20
2    30
dtype: int64

a    10
b    20
c    30
dtype: int64

a    10
b    20
c    30
dtype: int64
```

You can also create a Series from a NumPy array using `pd.Series(arr)`.

## 2. DataFrames

A DataFrame is a two-dimensional labeled data structure containing rows and columns.

### Creating a DataFrame

```python
df = pd.DataFrame({
    'Name': ['John', 'Anna', 'Peter', 'Linda'],
    'Age': [28, 34, 29, 42],
    'City': ['New York', 'Paris', 'Berlin', 'London'],
    'Salary': [65000, 70000, 62000, 85000]
})

print(df.shape)
print(df.columns.tolist())
print(df.head(2))
```

**Output**
```text
(4, 4)

['Name', 'Age', 'City', 'Salary']

   Name  Age      City  Salary
0  John   28  New York   65000
1  Anna   34     Paris   70000
```

### Basic operations

```python
df.shape
df.columns
df.info()
df.describe()
```

- `shape` — number of rows and columns.
- `columns` — column names.
- `info()` — column types and non-null counts.
- `describe()` — summary statistics for numeric columns.

### Selecting columns

```python
df[['Name', 'City']]
```

Returns only the `Name` and `City` columns.

### Adding and removing columns

```python
df['Designation'] = ['Doctor', 'Eng.', 'Doctor', 'Eng.']
df.drop('Designation', axis=1)
```

`axis=1` refers to columns. `axis=0` refers to rows.

### Selecting rows: `loc` vs `iloc`

```python
print(df.loc[0, 'Name'])
print(df.iloc[0, 1])
```

**Output**
```text
John
28
```

- `loc` — selects using row and column labels.
- `iloc` — selects using integer positions.

```python
df.loc[[0, 1], ['City', 'Salary']]
df.iloc[0:2, 0:2]
```

Note: `loc` label slices include the ending label, whereas `iloc` position slices exclude the ending position.

### Conditional selection

```python
df[df['Age'] > 30]
df[(df['Age'] > 30) & (df['City'] == 'Paris')]
```

The first filters people older than 30. The second filters people older than 30 who live in Paris.

### Applying functions

```python
df['Salary'].apply(lambda x: x + 1000)
```

Adds 1,000 to every salary value.

## 3. Missing Data

Sample DataFrame:

```python
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4, 5],
    'B': [1, 2, 3, 4, 5],
    'C': [1, 2, 3, np.nan, np.nan],
    'D': [1, np.nan, np.nan, np.nan, 5]
})
```

### Finding missing values

```python
df.isna()
df.isna().sum()
df.isna().any()
```

**Output of `df.isna().sum()`**
```text
A    1
B    0
C    2
D    3
dtype: int64
```

- `isna()` — identifies missing values.
- `sum()` — counts missing values per column.
- `any()` — checks whether a column contains any missing value.

### Removing missing values

```python
df.dropna()
df.dropna(thresh=3)
```

- `dropna()` removes rows containing at least one missing value.
- `dropna(thresh=3)` keeps rows with at least three non-missing values.

**Output of `df.dropna(thresh=3)`**
```text
     A  B    C    D
0  1.0  1  1.0  1.0
4  5.0  5  NaN  5.0
```

### Filling missing values

```python
df.fillna(0)

df.fillna({
    'A': 0,
    'B': 100,
    'C': 300,
    'D': 400
})

df.fillna(df.mean())
```

- Replace all missing values with zero.
- Replace missing values with column-specific values.
- Replace missing numeric values with their column means.

## 4. Merging, Joining and Concatenation

### Sample DataFrames

```python
employees = pd.DataFrame({
    'employee_id': [1, 2, 3, 4, 5],
    'name': ['John', 'Anna', 'Peter', 'Linda', 'Bob'],
    'department': ['HR', 'IT', 'Finance', 'IT', 'HR']
})

salaries = pd.DataFrame({
    'employee_id': [1, 2, 3, 6, 7],
    'salary': [60000, 80000, 65000, 70000, 90000],
    'bonus': [5000, 10000, 7000, 8000, 12000]
})
```

### Merging DataFrames

```python
pd.merge(employees, salaries, on='employee_id', how='inner')
pd.merge(employees, salaries, on='employee_id', how='left')
pd.merge(employees, salaries, on='employee_id', how='right')
pd.merge(employees, salaries, on='employee_id', how='outer')
```

| Merge type | Meaning |
|---|---|
| `inner` | Only matching IDs |
| `left` | All IDs from `employees` |
| `right` | All IDs from `salaries` |
| `outer` | All IDs from both DataFrames |

**Inner merge output**
```text
   employee_id   name department  salary  bonus
0            1   John         HR   60000   5000
1            2   Anna         IT   80000  10000
2            3  Peter    Finance   65000   7000
```

### Concatenation

```python
df1 = pd.DataFrame({
    'A': ['A0', 'A1'],
    'B': ['B0', 'B1']
})

df2 = pd.DataFrame({
    'A': ['A2', 'A3'],
    'B': ['B2', 'B3']
})

pd.concat([df1, df2])
pd.concat([df1, df2], axis=1)
```

- `axis=0` — stacks rows vertically.
- `axis=1` — places columns side by side, aligning their indexes.

### Joining DataFrames

```python
df1 = pd.DataFrame({'name': ['Alice', 'Bob', 'Charlie']},
                   index=[1, 2, 3])

df2 = pd.DataFrame({'score': [85, 90, 75]},
                   index=[2, 3, 4])

df1.join(df2, how='outer')
```

**Output**
```text
      name  score
1    Alice    NaN
2      Bob   85.0
3  Charlie   90.0
4      NaN   75.0
```

`join()` matches rows using indexes by default.

## 5. GroupBy and Aggregation

Sample DataFrame:

```python
df = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B', 'A', 'B', 'A', 'B'],
    'Store': ['S1', 'S1', 'S2', 'S2', 'S1', 'S2', 'S2', 'S1'],
    'Sales': [100, 200, 150, 250, 120, 180, 200, 300],
    'Quantity': [10, 15, 12, 18, 8, 20, 15, 25]
})
```

### Grouping by one column

```python
df.groupby('Category')['Sales'].sum()
```

**Output**
```text
Category
A    570
B    930
Name: Sales, dtype: int64
```

### Grouping by another column

```python
df.groupby('Store')['Sales'].sum()
```

**Output**
```text
Store
S1    720
S2    780
Name: Sales, dtype: int64
```

### Grouping by multiple columns

```python
df.groupby(['Category', 'Store'])['Sales'].sum()
```

**Output**
```text
Category  Store
A         S1       220
          S2       350
B         S1       500
          S2       430
Name: Sales, dtype: int64
```

### Aggregation functions

```python
df['Sales'].mean()
df['Sales'].agg(['sum', 'mean', 'min', 'max', 'count', 'std', 'median'])
```

Common aggregation functions:

- `sum()` — total
- `mean()` — average
- `median()` — middle value
- `min()` / `max()` — smallest / largest value
- `count()` — non-missing values
- `std()` — standard deviation

## 6. Pivot Tables and Cross Tabs

### Sample DataFrame

```python
df = pd.DataFrame({
    'Region': ['East', 'East', 'West', 'West'],
    'Product': ['A', 'B', 'A', 'B'],
    'Sales': [100, 200, 300, 400],
    'Units': [10, 20, 30, 40]
})
```

### Pivot table

Summarizes values by rows and columns.

```python
pd.pivot_table(
    df,
    values='Sales',
    index='Region',
    columns='Product',
    aggfunc='median'
)
```

**Output**
```text
Product    A    B
Region
East     100  200
West     300  400
```

### Multiple values

```python
pd.pivot_table(
    df,
    values=['Sales', 'Units'],
    index='Region',
    columns='Product'
)
```

By default, `pivot_table()` uses the mean for numeric values.

### Cross tabs

Counts the frequency of combinations between categories.

```python
pd.crosstab(df['Region'], df['Product'])
```

**Output**
```text
Product  A  B
Region
East     1  1
West     1  1
```

## 7. DataFrame Operations

Sample DataFrame:

```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, 30, 40, 50],
    'C': [100, 200, 300, 400, 500]
})
```

### Basic operations

```python
df.shape
df.columns
df.info()
df.describe()
```

### Arithmetic operations

```python
df['A'] + 10
```

**Output**
```text
0    11
1    12
2    13
3    14
4    15
Name: A, dtype: int64
```

### Applying a function

```python
df['D'] = df['B'].apply(lambda x: x**2)
print(df[['B', 'D']])
```

**Output**
```text
    B     D
0  10   100
1  20   400
2  30   900
3  40  1600
4  50  2500
```

## 8. Feature Extraction and Real-Data Analysis

These examples follow your `FeatureExtraction.ipynb` and `Countries.ipynb` notebooks. They depend on the corresponding CSV files.

### Loading data

```python
df = pd.read_csv('anime.csv')
df.head()
```

### Extracting information from strings

Your anime notebook extracts episode counts from titles such as `"Example (12 eps)"`.

```python
def extract_episodes(txt):
    check = False
    data = ""

    for char in txt:
        if char == ')':
            return data

        if check:
            data += char

        if char == '(':
            check = True

df['Episodes'] = df['Title'].apply(extract_episodes)
df['Episodes'] = df['Episodes'].str.replace(' eps', '')
df['Episodes'] = df['Episodes'].astype(int)
```

**Example**

```text
"Example (12 eps)" → 12
```

The resulting `Episodes` column contains integers.

### Finding maximum values

```python
df[df['Score'] == df['Score'].max()]['Title']
df[df['Episodes'] == df['Episodes'].max()]
```

These select anime with the highest score and the highest episode count, respectively.

### Extracting running time

Your notebook extracts the date range from a title and calculates the number of months between its start and end dates.

```python
from dateutil.relativedelta import relativedelta
from datetime import datetime

def calculate_total_months(period):
    try:
        start_str, end_str = period.split(' - ')
        start_date = datetime.strptime(start_str, '%b %Y')
        end_date = datetime.strptime(end_str, '%b %Y')

        r = relativedelta(end_date, start_date)
        return r.years * 12 + r.months + 1
    except:
        return None
```

The `+1` includes the starting month.

### Analyzing countries

```python
df = pd.read_csv('Countries.csv')
```

Find the country with the highest population:

```python
df[df['population'] == df['population'].max()]['country']
```

Find its capital:

```python
df[df['population'] == df['population'].max()]['capital_city']
```

Find the five highest democracy scores:

```python
df.sort_values(
    by='democracy_score',
    ascending=False
).head(5)
```

Count countries by region:

```python
df['region'].value_counts()
```

Find countries with missing political leaders:

```python
df[df['political_leader'].isna()]['country'].count()
```

Find the most populated country in Africa:

```python
africa_df = df[df['continent'] == 'Africa']

africa_df[
    africa_df['population'] == africa_df['population'].max()
]['country']
```

The exact outputs depend on the contents of `Countries.csv` and `anime.csv`.

---

## Revision Checklist

- [ ] Series creation
- [ ] DataFrame creation and selection
- [ ] `loc` vs `iloc`
- [ ] Conditional filtering
- [ ] Missing data
- [ ] Merge, join and concatenation
- [ ] GroupBy and aggregation
- [ ] Pivot tables and cross tabs
- [ ] Basic operations and `apply()`
- [ ] String-based feature extraction
- [ ] Real-data analysis with CSV files

**Revision tip:** Predict the output before running each example. Practice changing the conditions and values to understand how each operation behaves.
