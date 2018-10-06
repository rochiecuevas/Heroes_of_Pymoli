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

#### Gender Demographics
#### Gendered Purchase Analysis
#### Age Demographics
#### Purchase Analysis by Age
#### Top Spenders
#### What were the players buying?
#### Most Profitable Items
