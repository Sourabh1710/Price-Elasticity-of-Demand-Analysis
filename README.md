# Price Elasticity of Demand Analysis

## Overview
Price Elasticity of Demand (PED) measures the responsiveness of the quantity demanded of a product to changes in its price. It is calculated as the percentage change in quantity demanded divided by the percentage change in price. Understanding PED helps businesses make informed pricing decisions.

For instance:
- If demand is highly elastic (**PED > 1**), a small price reduction can lead to a significant increase in sales, potentially boosting revenue.
- If demand is inelastic (**PED < 1**), increasing prices will have minimal impact on sales volume, leading to higher profit margins.

---

## Price Elasticity of Demand Analysis with Python
### Step 1: Import Required Libraries and Dataset
I start by importing the necessary Python libraries and the [dataset](https://statso.io/price-elasticity-of-demand-case-study/).

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Step 2: Calculate Price Elasticity of Demand (PED)
```python
# Calculate the percentage change in price and quantity
data['Price_Change'] = data.groupby(['Store_ID', 'Item_ID'])['Price'].pct_change()
data['Quantity_Change'] = data.groupby(['Store_ID', 'Item_ID'])['Item_Quantity'].pct_change()

# Calculate the price elasticity of demand
data['PED'] = data['Quantity_Change'] / data['Price_Change']

data.replace([float('inf'), -float('inf')], float('nan'), inplace=True)
data.dropna(subset=['PED'], inplace=True)

# Summary statistics for PED
ped_summary = data['PED'].describe()
print(ped_summary)
```
The mean PED is approximately **-0.35**, indicating that demand is slightly inelastic. This means a **1% increase in price results in a 0.35% decrease in quantity demanded**.

---

### Step 3: Visualizing the Relationship Between Price and Quantity Changes
```python
plt.figure(figsize=(8,6))
sns.scatterplot(x=data['Price_Change'], y=data['Quantity_Change'], alpha=0.5)
plt.xlabel('Price Change (%)')
plt.ylabel('Quantity Change (%)')
plt.title('Price Change vs. Quantity Change')
plt.grid(True)
plt.show()
```

![Price Change vs Quantity Change](https://github.com/Sourabh1710/Price-Elasticity-of-Demand-Analysis/blob/main/images/Price%20Change%20vs%20Quantity%20Change.png)

This scatter plot shows that most data points are concentrated around the origin, indicating that many products experience **minimal changes in quantity demanded despite price changes**, suggesting inelastic demand. However, some products exhibit more elastic demand.

---

### Step 4: Segmenting the Market Based on PED
```python
# Define PED thresholds for segmentation
elastic_threshold = 1
inelastic_threshold = -1

# Segment the data based on PED
data['Segment'] = 'Unitary Elastic'
data.loc[data['PED'] > elastic_threshold, 'Segment'] = 'Highly Elastic'
data.loc[data['PED'] < inelastic_threshold, 'Segment'] = 'Inelastic'
data.loc[data['PED'] == 0, 'Segment'] = 'Zero Elasticity'
data.loc[data['PED'] < 0, 'Segment'] = 'Negative Elasticity'

# Count the number of items in each segment
segment_counts = data['Segment'].value_counts()
print(segment_counts)
```


![Price Change vs Quantity Change Across Different Market Segments](https://github.com/Sourabh1710/Price-Elasticity-of-Demand-Analysis/blob/main/images/Price%20Change%20vs%20Quantity%20Change%20Across%20Different%20Market%20Segments.png)

This visualization represents different market segments categorized by elasticity:
- **Negative Elasticity**: Price increases lead to quantity decreases.
- **Unitary Elastic**: Price changes result in proportionate quantity changes.
- **Highly Elastic**: Small price changes lead to significant quantity changes.
- **Zero Elasticity**: Quantity remains unchanged despite price changes.

---

## Pricing Strategies for Each Segment
Here are concise pricing strategies based on the identified segments:

1. **Negative Elasticity**: Reduce prices to stimulate demand. Price cuts can lead to significant increases in quantity sold, compensating for lower margins.
2. **Unitary Elastic**: Maintain stable pricing while focusing on improving product value.
3. **Highly Elastic**: Use dynamic pricing strategies like promotional discounts to leverage the high sensitivity of consumers to price changes.
4. **Zero Elasticity**: Focus on non-price strategies such as product improvements and customer service enhancements, as price changes have little impact.

---

## Conclusion
Price Elasticity of Demand (PED) is a crucial metric for understanding how consumers react to price changes. By analyzing PED, I can optimize pricing strategies to maximize revenue and profitability.

---


---

## Author
Sourabh Sonker <br>
Data Scientist
