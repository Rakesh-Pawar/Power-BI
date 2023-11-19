# Power-BI: Global Superstore Report with Currency Exchange 🌐💵

![Power-BI Image](https://github.com/Rakesh-Pawar/Power-BI/assets/112051343/035dc42b-3ffa-451a-9624-9caf0ca6ea62)

## Table of Contents
1. [DAX Used](#dax-used)
2. [Python Script](#python-script)
3. [Conclusion](#conclusion)

## DAX Used:

- **Total Sales Exchange:** 
  - Calculated total sales in the selected currency from the dropdown list.
  - DAX Formula:
    ```python
    Total Sales = 
    var SelectedCurrency = VALUES('Exchange Rate'[Currency])

    RETURN

    IF(
        COUNTROWS(SelectedCurrency)=1,
        [Total Sales] * LOOKUPVALUE(
            'Exchange Rate'[Exchange Rate USD], 'Exchange Rate'[Currency], SelectedCurrency
        )
    )
    ```

- **Total Profit Exchange:**
  - Calculated total profit in the selected currency from the dropdown list.
  - DAX Formula:
    ```python
    Total Profit = 
    var SelectedCurrency = VALUES('Exchange Rate'[Currency])

    RETURN

    IF(
        COUNTROWS(SelectedCurrency)=1,
        [Total Sales] * LOOKUPVALUE(
            'Exchange Rate'[Exchange Rate USD], 'Exchange Rate'[Currency], SelectedCurrency
        )
    )
    ```

## Python Script:

Utilized Python matplotlib package to compare Year-Over-Year (YOY) profit. Before that, calculated the previous year's profit.

```python
Last Year Profit = CALCULATE([Total Profit], PREVIOUSYEAR(Orders[Order Date].[Date]))
```

Lolipop chart in python- Power BI
```python

import matplotlib.pyplot as plt

# Assuming 'dataset' is your DataFrame
df = dataset.copy()

# Sorting the DataFrame by 'Total Profit'
sorted_df = df.sort_values(by='Total Profit')

# Creating an index for the y-axis
index_df = range(0, len(df.index))

# Set the figure size
fig, ax = plt.subplots(figsize=(12, 8))

# Plotting the horizontal lines
ax.hlines(y=index_df, xmin=sorted_df['Last Year Profit'], xmax=sorted_df['Total Profit'], color='tomato', alpha=0.7)

# Scatter plot for Current Year (CY) Profit
ax.scatter(sorted_df['Total Profit'], index_df, label="CY Profit", color='Green', s=120, alpha=0.7)

# Scatter plot for Last Year (LY) Profit
ax.scatter(sorted_df['Last Year Profit'], index_df, label='LY Profit', color='red', s=120, alpha=0.7)

# Adding labels and legend with increased font size and bold text
ax.legend(fontsize='large', labels=['CY Profit', 'LY Profit'])
ax.set_title("YOY Profit Comparison", fontsize='x-large', fontweight='bold')
ax.set_yticks(index_df)
ax.set_yticklabels(sorted_df['Region'], fontsize='medium', fontweight='bold')
ax.set_xlabel("Profit Values", fontsize='large', fontweight='bold')
ax.set_ylabel("Country", fontsize='large', fontweight='bold')

# Displaying Profit values on the x-axis
ax.tick_params(axis='x', labelrotation=45, labelsize='medium', width=2)  # Rotating x-axis labels for better readability

# Set background color to light grey
ax.set_facecolor('#F0F0F0')  # Hex color code for light grey

# Remove the outside borders
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['left'].set_visible(False)

# Display the chart
plt.show()


```
