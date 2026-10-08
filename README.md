# Outlier Detection using the IQR Method

Part of my **CampusX 100 Days of Machine Learning** journey.

## Overview
This lesson covers how to detect and handle outliers using the **Inter Quartile Range (IQR) method**. IQR is best suited for **skewed distributions**, and it is the same logic used by box plots.

## Topics Covered
- What outliers are and why they matter
- When to use IQR vs Z-score
- Quartiles (Q1, Q3) and the IQR
- Calculating the lower and upper fences
- Detecting outliers with pandas
- Handling outliers: **Trimming** and **Capping**
- Visual checks with box plots and distplots

## The Method
1. Find **Q1** (25th percentile) and **Q3** (75th percentile)
2. Compute **IQR = Q3 - Q1**
3. Compute the limits:
   - `Lower limit = Q1 - 1.5 * IQR`
   - `Upper limit = Q3 + 1.5 * IQR`
4. Any value outside these limits is an **outlier**

### Example
If Q1 = 20 and Q3 = 40:
- IQR = 20
- Lower limit = 20 - 30 = **-10**
- Upper limit = 40 + 30 = **70**
- A value of 95 is an outlier. A value of 65 is not.

## Code Snippets

### Detecting outliers
```python
q1 = df['col'].quantile(0.25)
q3 = df['col'].quantile(0.75)
iqr = q3 - q1

lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr

outliers = df[(df['col'] < lower) | (df['col'] > upper)]
```

### Trimming (remove outliers)
```python
new_df = df[(df['col'] >= lower) & (df['col'] <= upper)]
```

### Capping (replace with limits)
```python
import numpy as np

new_df = df.copy()
new_df['col'] = np.where(df['col'] > upper, upper,
                 np.where(df['col'] < lower, lower, df['col']))
```

## Trimming vs Capping
| Approach | What it does | Best for |
|---|---|---|
| Trimming | Removes outlier rows | Large datasets |
| Capping | Replaces outliers with the limits | Small datasets |

## Z-score vs IQR
| | Z-score | IQR |
|---|---|---|
| Best for | Normal distribution | Skewed distribution |
| Rule | Beyond 3 standard deviations | Beyond 1.5 x IQR fences |
| Robust to outliers? | No | Yes |

## Tools Used
- Python
- pandas
- NumPy
- Matplotlib / Seaborn

## Files
- `outlier_detection_iqr.ipynb` - notebook with the code from the lesson
- `README.md` - this file

## Reference
CampusX - 100 Days of Machine Learning (YouTube playlist)
