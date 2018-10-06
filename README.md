# Purchase Analysis of "Heroes of Pymoli"

## Overview
"Heroes of Pymoli" is the latest release of a fictional gaming company. The object of this study was to obtain insights about the players of the game, including age, gender, and the items they purchased while playing the game.

## Getting Started
Pandas and NumPy were used during the data analyses.

```python
# Dependencies and Setup
import pandas as pd
import numpy as np
```

The data came from the dataset in `purchase_data.csv`.

```python
# Load the purchasing file (.csv)
file = "purchase_data.csv"

# Read purchasing file and store into Pandas data frame
purchase_df = pd.read_csv(file)
```

## Data Analyses

### Player Count
The number of players who bought optional items in the course of gameplay was determined.

```python
# determine number of players based on unique SN
player_count = len(purchase_df["SN"].unique())

player_count
```

### Purchase Analysis
An overview of the purchases of the players was generated. Statistics included the number of unique items purchased, the number of purchases, the revenue generated, and the average price of items purchased.

```python
no_items = len(purchase_df["Item ID"].unique()) # number of unique items purchased
no_purchases = purchase_df["Purchase ID"].count() # number of purchases
total_sales = "{:,.2f}".format(purchase_df["Price"].sum()) # total of price
ave_price = "{:.2f}".format(purchase_df["Price"].sum() / no_purchases)
summary_purchases = pd.DataFrame({"Number of unique items":[no_items],
                        "Number of purchases": [no_purchases],
                        "Total Sales (USD)": [total_sales],
                        "Average Price (USD)": [ave_price]})
summary_purchases
```

#### Gender Demographics
To understand the demographics of the players, they had to be grouped by gender. Before doing so, it was important to make sure that the list contained unique player identifiers only.

```python
# drop duplicate player entries and group players by gender
gender_groups = purchase_df.drop_duplicates("SN").groupby("Gender")
```

Then, the number of players and the proportion of players were calculated.

```python
# extract series for counts and for percentages by gender
gender_count = gender_groups.size()
gender_pct = round(((gender_groups.size() / player_count) * 100),2)

# create a dataframe containing gender counts and proportions
gender_demographics = pd.DataFrame(dict(gender_count = gender_count, gender_pct = gender_pct))

# rename the columns for clarity
gender_demographics = gender_demographics.rename(columns = {"gender_count":"Number of Players",
                                                            "gender_pct":"Proportion (%)"})

gender_demographics
```

#### Gendered Purchase Analysis

#### Age Demographics
#### Purchase Analysis by Age
#### Top Spenders
#### What were the players buying?
#### Most Profitable Items
