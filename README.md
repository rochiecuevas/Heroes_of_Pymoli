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
To determine the purchase patterns of players, by gender, the dataframe was grouped first by gender.

```python
# group players and purchases by gender (no dropped duplicates)
gender_groups2 = purchase_df.groupby("Gender")
```

The total number of purchases, the number of purchases per person, the revenue generated, the average cost of purchases, and the average purchase price per person were calculated per gender.

``` python
# number of purchases per gender
purchase_count = gender_groups2["Purchase ID"].count()

# average number of purchases by gender
ave_no_purch = round((purchase_count / gender_count),2)

# total purchase price per gender
tot_purch_gender = gender_groups2["Price"].sum()

# average total purchase price per person by gender
ave_purch_person = round((tot_purch_gender / gender_count),2)

# average purchase price by gender
ave_purch_gender = round((tot_purch_gender / purchase_count),2)

# summary table
gender_purchases = pd.DataFrame(dict(purchase_count = purchase_count, 
                                     ave_no_purch = ave_no_purch,
                                     tot_purch_gender = tot_purch_gender,
                                     ave_purch_gender = ave_purch_gender,
                                     ave_purch_person = ave_purch_person
                                     ))

# rename columns for clarity
gender_purchases = gender_purchases.rename(columns = {"purchase_count": "Total Number of Purchases",
                                                      "ave_no_purch": "Number of Purchases Per Person",
                                                      "tot_purch_gender": "Total Cost of Purchases (USD)",
                                                      "ave_purch_gender": "Average Cost of Purchases (USD)",
                                                      "ave_purch_person": "Ave Total Purchase Per Person (USD)"
                                                      })

gender_purchases
```

#### Age Demographics
Similarly, the purchases were grouped, but this time based on age. To do so, the player age range was divided into nine bins. Then, the number and the proportion of players that belonged to each bin were determined.

```python
# Establish the bins
bins = [0, 10, 15, 20, 25, 30, 35, 40, 45, 50]

# Establish the groups
group_names = ["<10", "10–14", "15–19","20–24", "25–29", "30–34", "35–39","40–44", "45+"]

# Insert a column with Age Group in the dataframe
purchase_df["Age Grp"] = pd.cut(purchase_df["Age"], bins, right = False, labels = group_names)

# Group the players by Age Group
age_unique = purchase_df.drop_duplicates("SN").groupby("Age Grp")

# Count the number of players falling into each bin
size_age_grps = age_unique["Age"].count()

# Proportion of players per bin
pct_age_grps = round(((size_age_grps / player_count) * 100),2)
pct_age_grps

# Summary table
age_groups = pd.DataFrame(dict(size_age_grps = size_age_grps, 
                                     pct_age_grps = pct_age_grps
                                     ))
# Rename the columns
age_groups = age_groups.rename(columns = {"size_age_grps": "Number of players",
                                          "pct_age_grps": "Proportion (%)"})
age_groups
```

#### Purchase Analysis by Age
To determine purchases by age, these were grouped based on the age groups established previously. 

```python
# Group the purchases by age
age_purch = purchase_df.groupby("Age Grp")
```

The same statistics calculated in analysing gendered purchases were used in looking into the purchases per age.

```python
# Total number of purchases by age
age_purch_count = age_purch["Purchase ID"].count()

# Average number of purchases by age
ave_purch_count = round((age_purch_count / size_age_grps),2)

# Total purchase price by age group
age_total_purch = age_purch["Price"].sum()

# Average purchase price by age group
age_ave_purch = round((age_total_purch / age_purch_count),2)

# Average purchase price per person by age group
age_ave_person_purch = round((age_total_purch / size_age_grps),2)

# Summary table
age_purchases = pd.DataFrame(dict(age_purch_count = age_purch_count,
                                  ave_purch_count = ave_purch_count,
                                  age_total_purch = age_total_purch,
                                  age_ave_purch = age_ave_purch,
                                  age_ave_person_purch = age_ave_person_purch,
                                  ))

# Rename the columns
age_purchases = age_purchases.rename(columns = {"age_purch_count": "Total Number of Purchases",
                                                "ave_purch_count": "Number of Purchases Per Person",
                                                "age_total_purch": "Total Cost of Purchases (USD)",
                                                "age_ave_purch": "Average Cost of Purchases (USD)",
                                                "age_ave_person_purch": "Average Purchase Per Person (USD)"})
age_purchases
```

#### Top Spenders
Determining the top spenders required grouping the purchases by player.

```python
# Group the players by SN
big_spender = purchase_df.groupby("SN")
```

Three statistics were then calculated per player: the number of purchases, the total purchase cost, and the average purchase cost.

```python
# Calculate cost of purchase for each player
tot_purchase_SN = big_spender["Price"].sum()

# Calculate number of purchases for each player
count_purchase_SN = big_spender["Price"].count()

# Calculate the average cost of purchase for each player
ave_purchase_SN = round((tot_purchase_SN / count_purchase_SN),2)

# Combine in a dataframe
big_spender2 = pd.DataFrame(dict(count_purchase_SN = count_purchase_SN,
                                 tot_purchase_SN = tot_purchase_SN,
                                 ave_purchase_SN = ave_purchase_SN
                                ))

# Rename the column headers
big_spender2 = big_spender2.rename(columns = {"count_purchase_SN": "Number of Purchases",
                                              "tot_purchase_SN":"Total Purchase Cost (USD)",
                                              "ave_purchase_SN": "Ave Purchase Cost (USD)"})
```                                              

Then, the data was sorted based on the total purchase cost, in a descending order. The top five entries were the top five spenders.

```python
# Sort the players by purchase cost in descending order
big_spender2 = big_spender2.sort_values("Total Purchase Cost (USD)",ascending = False)

# Get the top 10 biggest spenders
top5_spender = big_spender2[0:5]
top5_spender
```

#### What were the players buying?
Aside from knowing the players with the biggest purchases, it was also important to know what the players were mostly buying. In this case, it was not important to know who was buying; hence, the player identifiers were excluded from the new dataframe.

```python
# Create dataframe with Item ID, Item Name, and Price
items = purchase_df[["Item ID", "Item Name", "Price"]]
```

The data was then grouped by Item ID and name so that calculations could be conducted per group.

```python
# Group items based on item ID and name
items_gb = items.groupby(["Item ID", "Item Name"])
```

The statistics that were derived for each item were: number of times the item was purchased, the price of each item, and the total purchase for each item. The top five items purchased were then determined after sorting the items in descending order of number of times they were purchased.

```python
# Calculations for purchase count and total purchases
items_count = items_gb["Item ID"].count() # number of purchases per item
items_tot_price = items_gb["Price"].sum() # total cost of purchases per item
items_price = items_gb["Price"].mean() # price per item; if all prices are the same, the mean is the price

# Create new dataframe
summary_items = pd.DataFrame(dict(items_count = items_count,
                                  items_price = items_price,
                                  items_tot_price = items_tot_price))

# Rename the columns for clarity
summary_items = summary_items.rename(columns = {"items_count": "Number of Items Purchased",
                                                "items_price": "Price per Item (USD)",
                                                "items_tot_price": "Total Item Purchase (USD)"})

# Rearrange items by the number of items purchased
summary_items2 = summary_items.sort_values("Number of Items Purchased",ascending = False)
summary_items2.head()
```

#### Most Profitable Items
The same dataframe was sorted, in descending order, by total purchase price per item. The top five items were considered the most profitable among the items on offer in the game.

```python
# Rearrange items by the total item purchase
summary_items3 = summary_items.sort_values("Total Item Purchase (USD)",ascending = False)
summary_items3.head()
``