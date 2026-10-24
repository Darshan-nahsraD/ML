# Data Visualization Notes

Quick revision notes for Matplotlib, Seaborn, Plotly, regression plots, and the IPL 2022 capstone project. Learn through short code examples instead of watching long videos.

## 1. Matplotlib Basics

Matplotlib is used to create charts and customize their appearance.

### Setup and sample data

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

x = np.linspace(0, 5, 11)
y = x ** 3
```

**Output of `x`:**
```text
[0.  0.5 1.  1.5 2.  2.5 3.  3.5 4.  4.5 5. ]
```

### Line plot

```python
plt.plot(x, y)
plt.title("Cubic Graph")
plt.xlabel("X")
plt.ylabel("Y")
plt.show()
```

**Expected result:** A line graph showing \(y=x^3\).

### Scatter plot

```python
plt.scatter(x, y)
plt.show()
```

**Expected result:** Individual points instead of a connected line.

### Histogram

```python
data = [10, 20, 20, 30, 30, 30, 40, 50]
plt.hist(data, bins=5)
plt.show()
```

**Expected result:** Bars showing the frequency distribution of the data.

### Box plot

```python
plt.boxplot([10, 20, 25, 30, 35, 40, 100])
plt.show()
```

**Expected result:** A box showing the median and quartiles, with potential outliers.

### Subplots

```python
plt.subplot(1, 2, 1)
plt.plot(x, y)

plt.subplot(1, 2, 2)
plt.scatter(x, y)

plt.tight_layout()
plt.show()
```

**Expected result:** Two plots arranged side by side.

- `subplot(rows, columns, position)` selects a plotting area.
- `(1, 2, 1)` means one row, two columns, first position.
- `(1, 2, 2)` means the second position.

### Object-oriented approach

```python
fig, ax = plt.subplots(figsize=(8, 4))

ax.plot(x, y)
ax.set_title("Cubic Graph")
ax.set_xlabel("X")
ax.set_ylabel("Y")

plt.show()
```

- `fig` represents the entire figure.
- `ax` represents the plotting area.
- `figsize` sets the figure dimensions in inches.

### Adding multiple axes

```python
fig = plt.figure(figsize=(8, 4))

ax1 = fig.add_axes([0.1, 0.1, 0.35, 0.8])
ax2 = fig.add_axes([0.55, 0.1, 0.35, 0.8])

ax1.plot(x, y)
ax2.scatter(x, y)

plt.show()
```

The four values in `add_axes()` represent `[left, bottom, width, height]` as fractions of the figure.

### Saving a plot

```python
fig, ax = plt.subplots()
ax.plot(x, y)

fig.savefig("graph.png", dpi=300, bbox_inches="tight")
```

Saves the graph as a high-resolution PNG image.

### Working with images

```python
import matplotlib.image as mpimg

img = mpimg.imread("images.jpeg")
plt.imshow(img)
plt.axis("off")
plt.show()
```

Crop an image using array slicing:

```python
cropped_img = img[50:200, 100:300]
plt.imshow(cropped_img)
plt.axis("off")
plt.show()
```

The crop selects rows 50–199 and columns 100–299.

---

## 2. Seaborn Distribution Plots

Seaborn is built on Matplotlib and simplifies statistical visualization.

### Setup

```python
import seaborn as sns
import matplotlib.pyplot as plt

df = sns.load_dataset("tips")
```

The `tips` dataset includes `total_bill`, `tip`, `sex`, `smoker`, `day`, `time`, and `size`.

### Histogram with KDE

```python
sns.histplot(data=df, x="total_bill", bins=20, kde=True)
plt.show()
```

**Expected result:** Bars show bill frequencies; the KDE curve estimates the distribution's shape.

### Pair plot

```python
sns.pairplot(df)
```

**Expected result:** Pairwise plots for numerical columns, with distributions along the diagonal.

Use a categorical variable for color:

```python
sns.pairplot(df, hue="sex", palette="rainbow")
```

### Joint plot

```python
sns.jointplot(data=df, x="total_bill", y="tip", kind="scatter")
```

**Expected result:** A scatter plot in the center with separate marginal distributions.

Regression version:

```python
sns.jointplot(data=df, x="total_bill", y="tip", kind="reg")
```

Adds a fitted regression line.

### Rug plot

```python
sns.rugplot(data=df, x="tip")
```

**Expected result:** Small marks along the axis showing individual observations.

---

## 3. Seaborn Categorical Plots

Categorical plots compare groups such as days, genders, or smoker status.

### Count plot

```python
sns.countplot(data=df, x="sex", hue="smoker")
plt.show()
```

**Expected result:** Counts of observations by sex, separated by smoker status.

### Bar plot

```python
sns.barplot(data=df, x="sex", y="total_bill")
plt.show()
```

**Expected result:** The mean total bill for each sex by default, with uncertainty intervals.

Change the aggregation function:

```python
import numpy as np

sns.barplot(data=df, x="sex", y="tip", estimator=np.sum)
```

This displays the sum of tips for each group.

### Box plot

```python
sns.boxplot(data=df, x="tip", y="day")
```

Shows the median, quartiles, spread, and potential outliers for each day.

### Violin plot

```python
sns.violinplot(data=df, x="day", y="total_bill")
```

Shows the distribution's shape for each category.

### Strip plot

```python
sns.stripplot(data=df, x="day", y="total_bill")
```

Displays individual observations, which may overlap.

### Swarm plot

```python
sns.swarmplot(data=df, x="day", y="total_bill")
```

Displays individual observations while adjusting their positions to reduce overlap.

### Combining plots

```python
sns.violinplot(data=df, x="day", y="total_bill")
sns.swarmplot(data=df, x="day", y="total_bill", color="black")
plt.show()
```

Combines the distribution summary with individual data points.

---

## 4. Seaborn Matrix Plots

Matrix plots represent data in a grid, often using colors to show magnitude.

### Heatmap

```python
flights = sns.load_dataset("flights")

pvtflight = flights.pivot_table(
    values="passengers",
    index="month",
    columns="year"
)

sns.heatmap(pvtflight)
plt.show()
```

**Expected result:** A month-by-year grid colored according to passenger counts.

### Correlation matrix

```python
tips = sns.load_dataset("tips")

tipscorr = tips[["total_bill", "tip", "size"]]
print(tipscorr.corr())
```

**Expected result:** A \(3 \times 3\) matrix of correlation coefficients, with diagonal values equal to 1.

### Annotated heatmap

```python
sns.heatmap(tipscorr.corr(), annot=True)
plt.show()
```

`annot=True` displays the correlation values inside the cells.

### Cluster map

```python
sns.clustermap(tipscorr.corr())
```

**Expected result:** A heatmap with hierarchical clustering that rearranges rows and columns according to similarity.

---

## 5. Regression Plots

Regression plots show the relationship between numerical variables and a fitted trend.

### Basic regression plot

```python
tips = sns.load_dataset("tips")

sns.lmplot(data=tips, x="total_bill", y="tip")
```

**Expected result:** A scatter plot with a fitted regression line.

### Regression by category

```python
sns.lmplot(
    data=tips,
    x="total_bill",
    y="tip",
    hue="sex"
)
```

Separates observations and regression lines by sex.

### Customize colors and markers

```python
sns.lmplot(
    data=tips,
    x="total_bill",
    y="tip",
    hue="sex",
    palette="coolwarm",
    markers=["o", "v"],
    scatter_kws={"s": 50}
)
```

- `hue` — separates categories by color.
- `palette` — chooses a color scheme.
- `markers` — changes point shapes.
- `scatter_kws` — customizes scatter points.

---

## 6. Plotly and Cufflinks

Plotly creates interactive charts. Cufflinks connects Pandas DataFrames to Plotly plotting methods.

### Setup

```python
import seaborn as sns
import pandas as pd
import cufflinks as cf

cf.go_offline()

tips = sns.load_dataset("tips")
```

Install the packages if required:

```python
%pip install plotly cufflinks
```

### Interactive line plot

```python
tips["total_bill"].iplot()
```

Creates an interactive line chart of total bills.

### Interactive bar plot

```python
tips.groupby("day")["tip"].mean().iplot(kind="bar")
```

Shows average tips by day.

### Interactive scatter plot

```python
tips.iplot(
    kind="scatter",
    x="total_bill",
    y="tip",
    mode="markers"
)
```

Shows the relationship between bills and tips.

### Interactive box plot

```python
tips[["total_bill", "day"]].iplot(
    kind="box",
    x="total_bill",
    y="day"
)
```

Creates an interactive box plot grouped by day.

**Note:** Interactive output depends on your notebook environment and installed package versions.

---

## 7. IPL 2022 Capstone Project

The project analyzes IPL 2022 match-level data to understand match outcomes, toss decisions, player performances, and venues.

### Load the dataset

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.read_csv("IPL.csv")  # Use your actual CSV filename
```

### Inspect the data

```python
df.head()
df.info()
df.shape
df.isnull().sum()
```

These show sample rows, data types, dataset dimensions, and missing-value counts.

### 1. Which team won the most matches?

```python
print(df["match_winner"].value_counts().head(1))
```

Returns the team with the highest number of wins and its count.

Visualize it:

```python
sns.barplot(
    x=df["match_winner"].value_counts().index,
    y=df["match_winner"].value_counts().values
)

plt.title("Most Matches Won by Team")
plt.xticks(rotation=90)
plt.show()
```

### 2. Toss decision trends

```python
sns.countplot(data=df, x="toss_decision")
plt.show()
```

Compares the number of matches where teams chose to bat or field.

### 3. Toss winner vs match winner

```python
count = (df["toss_winner"] == df["match_winner"]).sum()
print(count)
```

Counts how many matches were won by the team that won the toss.

### 4. How do teams win?

```python
sns.countplot(data=df, x="won_by")
plt.show()
```

Compares the recorded win methods, such as runs and wickets, depending on the dataset values.

### 5. Player of the Match awards

```python
print(df["player_of_the_match"].value_counts().head(1))
```

Returns the player with the most awards and their count.

### 6. Top scorers

```python
print(df["top_scorer"].head())
```

Displays the top-scorer values from the first five rows.

### 7. Best bowling figures

```python
print(df["best_bowling_figure"].head(10))
```

Displays the first ten recorded bowling-figure values.

### 8. Most matches played by venue

```python
sns.countplot(data=df, y="venue")
plt.show()
```

Compares match counts by venue.

### 9. Highest winning margin by runs

```python
print(
    df[df["margin"] == df["margin"].max()][["match_winner"]]
)
```

Finds the winner or winners associated with the maximum recorded margin. Check `won_by` if you specifically need to restrict the analysis to victories by runs.

### 10. Highest individual score

```python
print(
    df.loc[df["highscore"].idxmax(), ["top_scorer", "highscore"]]
)
```

Returns the top scorer and highest individual score.

### 11. Best bowling performance

```python
print(
    df.loc[
        df["best_bowling_figure"].idxmax(),
        "best_bowling"
    ]
)
```

Use this only if `best_bowling_figure` is numeric. If the column stores strings such as `"5/20"`, parse wickets and runs into numeric columns before comparing performances.

**Important:** Exact IPL answers depend on the actual dataset file and its values.

---

## 8. Quick Revision Guide

| Goal | Function |
|---|---|
| Draw a line graph | `plt.plot()` |
| Compare individual points | `plt.scatter()` |
| Show frequencies | `plt.hist()` / `sns.histplot()` |
| Show category counts | `sns.countplot()` |
| Compare group averages | `sns.barplot()` |
| Show quartiles and outliers | `sns.boxplot()` |
| Show distribution shape | `sns.violinplot()` |
| Show pairwise relationships | `sns.pairplot()` |
| Show correlations | `sns.heatmap()` |
| Show a fitted trend | `sns.lmplot()` |
| Make interactive plots | `.iplot()` |

## Revision tip

For each plot, remember three things: **when to use it, which function creates it, and which columns go into it.** Run the examples, modify the column names, and predict the resulting chart before executing the code.
