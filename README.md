# Weighted_Average_Rating_Calculation

# Business Problem

In this section, we will rank the datasets by their weighted average ratings. This method takes into account both the ratings and their relative importance, providing a more accurate comparison. As a result, the rankings better reflect the true quality of each dataset.

# Import Necessary Libraries

```
import numpy as np
import pandas as pd
from sklearn.preprocessing import MinMaxScaler

pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", None)
pd.set_option("display.width", 500)
pd.set_option("display.float_format", lambda x: "%.2f" % x)
```

# Import Dataset

```
df = pd.read_csv("course_reviews.csv")
df.head()
```

# General Information About Dataset

Since we want to check the data to get a general idea about it, we create and use a function called check_df(dataframe, head=5, tail=5) that prints the referenced functions:

```
def check_df(dataframe, head=5):
    print(20*"#", "Head", 20*"#")
    print(dataframe.head(head))
    print(20*"#", "Tail", 20*"#")
    print(dataframe.tail(head))
    print(20*"#", "Shape", 20*"#")
    print(dataframe.shape)
    print(20*"#", "Type", 20*"#")
    print(dataframe.dtypes)
    print(20*"#", "NA", 20*"#")
    print(dataframe.isnull().sum())
    print(20*"#", "Quartiles", 20*"#")
    print(dataframe.describe([0, 0.01, 0.05, 0.10, 0.20, 0.30, 0.40, 0.50, 0.60, 0.70, 0.80, 0.90, 0.95, 0.99, 1]).T)
```

# Data Pre-Processing

In the data preprocessing stage, we use the pandas library to convert the "Time Stamp" feature into a format that pandas can recognize as dates. We then find the maximum date and define `current_date` as a few days after this maximum. Finally, we calculate the difference in days between `current_date` and each timestamp, and add this difference as a new feature called "days" in the dataframe.

```
df["Timestamp"] = pd.to_datetime(df["Timestamp"])
current_date = pd.to_datetime("2021-02-10 00:00:00")
df["days"] = (current_date - df["Timestamp"]).dt.days
```

# Time Based Average

Time-Based Average calculation involves computing average values of data by considering specific time intervals to understand trends. This code snippet calculates the average ratings for different time ranges and applies weights to these averages based on the time period. It gives higher weights to more recent ratings, reflecting the impact of time on the average score and resulting in a balanced rating that accounts for temporal effects.

```
df.loc[df["days"]<=30, "Rating"].mean() * 28/100 + \
df.loc[(df["days"]>30) & (df["days"]<=90), "Rating"].mean() * 26/100 + \
df.loc[(df["days"]>90) & (df["days"]<=180), "Rating"].mean() * 24/100 + \
df.loc[df["days"]>180, "Rating"].mean() * 22/100
```

# User Based Average

