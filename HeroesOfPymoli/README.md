
<p><b>Heroes Of Pymoli Data Analysis</b></p>
<p>Trends Noticed:
<p>1. The age-group of 20-24 years is the most likely to purchase items in this game than any other age-group. </p>
<p>2. This game is much more popular amongst male players than female players or those of undisclosed gender. </p>
<p>3. The most profitable item is usually cheaper than the most expensive item, meaning that profitability depends more on its popularity (how many times its bought) than its price.</p>


```python
import pandas as pd
import os
import json

path1 = os.path.join("purchase_data.json")
path2 = os.path.join("purchase_data2.json")

with open(path1) as data_file:    
    list_1 = json.load(data_file)

with open(path2) as data_file:    
    list_2 = json.load(data_file) 
    
#Working with list_1 for now
```


```python
####################****Player Count****####################

#total_df = pd.DataFrame(list_1)
total_df = pd.DataFrame(list_2)

#total_df.head()

```


```python
player_count = total_df["SN"].nunique(dropna=True)
di = {"Total Number of Players": player_count}
players = pd.DataFrame.from_dict([di])
players
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Number of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################****Purchasing Analysis (Total)****####################

item_count = total_df["Item ID"].nunique(dropna=True)
avg_price = total_df["Price"].mean()
total_purchases = len(total_df)
total_revenue = total_df["Price"].sum()

info_dict={"Number of Unique Items":item_count,
       "Average Purchase Price":avg_price,
       "Total Number of Purchases":total_purchases,
       "Total Revenue":total_revenue}
purchasing_analysis_df = pd.DataFrame.from_dict([info_dict])
purchasing_analysis_df["Average Purchase Price"] = purchasing_analysis_df["Average Purchase Price"].map('${:,.2f}'.format)
purchasing_analysis_df["Total Revenue"] = purchasing_analysis_df["Total Revenue"].map('${:,.2f}'.format)

purchasing_analysis_df.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.92</td>
      <td>64</td>
      <td>78</td>
      <td>$228.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################****Gender Demographics****####################

# * Percentage and Count of Male Players
# * Percentage and Count of Female Players
# * Percentage and Count of Other / Non-Disclosed

g_df = total_df.groupby("Gender")
gender_demographics = g_df["SN"].nunique()
gender_demographics = pd.DataFrame(gender_demographics)
gender_demographics["Percentage of Players"] = gender_demographics["SN"]*100/player_count
gender_demographics["Percentage of Players"] = gender_demographics["Percentage of Players"].map('{:,.2f}%'.format)
gender_demographics.rename(columns={"SN":"Total Count"}, inplace=True)

gender_demographics.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>13</td>
      <td>17.57%</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>60</td>
      <td>81.08%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1</td>
      <td>1.35%</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################****Purchasing Analysis (Gender)****####################

gender_purchases = g_df["SN"].count()
gender_purchases = pd.DataFrame(gender_purchases)
gender_purchases["Average Purchase Price"] = g_df["Price"].mean()
gender_purchases["Total Purchase Value"] = g_df["Price"].sum()
gender_purchases["Normalized Totals"] = gender_purchases["Total Purchase Value"]/gender_purchases["SN"]
gender_purchases.rename(columns={"SN":"Purchase Count"},inplace=True)
gender_purchases["Average Purchase Price"] = gender_purchases["Average Purchase Price"].map('${:,.2f}'.format)
gender_purchases["Normalized Totals"] = gender_purchases["Normalized Totals"].map('${:,.2f}'.format)
gender_purchases["Total Purchase Value"] = gender_purchases["Total Purchase Value"].map('${:,.2f}'.format)

gender_purchases.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>13</td>
      <td>$3.18</td>
      <td>$41.38</td>
      <td>$3.18</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>64</td>
      <td>$2.88</td>
      <td>$184.60</td>
      <td>$2.88</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1</td>
      <td>$2.12</td>
      <td>$2.12</td>
      <td>$2.12</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################****Age Demographics****####################

bins = [0,9,14,19,24,29,34,39,100]
group_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
new_df = total_df
new_df["Age Category"] = pd.cut(total_df["Age"], bins, labels=group_names)
age_df = new_df.groupby("Age Category")

age_breakdown = age_df["SN"].nunique()
age_breakdown_df = pd.DataFrame(age_breakdown)
age_breakdown_df["Percentage of Players"] = (age_breakdown_df["SN"]*100/player_count).map('{:,.2f}%'.format)
age_breakdown_df.rename(columns={"SN":"Total Count"},inplace=True)
age_breakdown_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Category</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>6.76%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>4.05%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>14.86%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>34</td>
      <td>45.95%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>8</td>
      <td>10.81%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>6</td>
      <td>8.11%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>8.11%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>1.35%</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################****Purchase Analysis (Age Demographics)****####################

age_dem_df = age_df["SN"].count()
age_dem_df = pd.DataFrame(age_dem_df)
age_dem_df["Average Purchase Price"] = age_df["Price"].mean().map('${:,.2f}'.format)
age_dem_df["Total Purchase Value"] = age_df["Price"].sum()
age_dem_df["Normalized Value"] = (age_dem_df["Total Purchase Value"]/age_dem_df["SN"]).map('${:,.2f}'.format)
age_dem_df.rename(columns={"SN":"Purchase Count"},inplace=True)
age_dem_df["Total Purchase Value"] = age_dem_df["Total Purchase Value"].map('${:,.2f}'.format)
age_dem_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Value</th>
    </tr>
    <tr>
      <th>Age Category</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>$2.76</td>
      <td>$13.82</td>
      <td>$2.76</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>$2.99</td>
      <td>$8.96</td>
      <td>$2.99</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>$2.76</td>
      <td>$30.41</td>
      <td>$2.76</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>$3.02</td>
      <td>$108.89</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>$2.90</td>
      <td>$26.11</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>$1.98</td>
      <td>$13.89</td>
      <td>$1.98</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>$3.56</td>
      <td>$21.37</td>
      <td>$3.56</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>$4.65</td>
      <td>$4.65</td>
      <td>$4.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
##################### **Top Spenders**####################

spenders_df = total_df.groupby("SN")
max_spenders = spenders_df["Price"].sum()
max_spenders = max_spenders.sort_values(ascending=False)
five_max = max_spenders.iloc[:5]
#five_max_total = five_max.sum()

five = list(five_max.to_frame().index)

max_five_info_df = total_df[total_df['SN'].isin(five)]
#max_five_purchases = max_five_info_df["Age"].count()
max_five_info_df = max_five_info_df.groupby("SN")
max_five_stats = max_five_info_df["Price"].count()
max_five_stats = pd.DataFrame(max_five_stats)
max_five_stats["Average Purchase Price"] = max_five_info_df["Price"].mean().map('${:,.2f}'.format)
max_five_stats["Total Purchase Value"] = max_five_info_df["Price"].sum().map('${:,.2f}'.format)
max_five_stats.rename(columns={"Price": "Purchase Count"},inplace=True)
max_five_stats.sort_values("Total Purchase Value", ascending=False)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Sundaky74</th>
      <td>2</td>
      <td>$3.71</td>
      <td>$7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>$2.56</td>
      <td>$5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>$4.81</td>
      <td>$4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>$4.78</td>
      <td>$4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>$4.71</td>
      <td>$4.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################**Most Popular Items**####################

pop_items_df = total_df.groupby(["Item ID", "Item Name","Price"])
pop_items_p_count = pop_items_df["Age"].count()
pop_items_p_count = pop_items_p_count.sort_values(ascending=False)
pop_items_purchaes = pop_items_p_count.iloc[:5]



pop_items_purchases = pd.DataFrame(pop_items_purchaes)

pop_items_purchases = pop_items_purchases.reset_index()
pop_items_purchases["Total Purchased"] = (pop_items_purchases["Age"]*pop_items_purchases["Price"]).map('${:,.2f}'.format)
pop_items_purchases.rename(columns={"Age":"Purchase Count"}, inplace=True)
pop_items_purchases["Price"] = pop_items_purchases["Price"].map('${:,.2f}'.format)

pop_items_purchases

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>Purchase Count</th>
      <th>Total Purchased</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>$3.64</td>
      <td>3</td>
      <td>$10.92</td>
    </tr>
    <tr>
      <th>1</th>
      <td>98</td>
      <td>Deadline, Voice Of Subtlety</td>
      <td>$1.29</td>
      <td>2</td>
      <td>$2.58</td>
    </tr>
    <tr>
      <th>2</th>
      <td>117</td>
      <td>Heartstriker, Legacy of the Light</td>
      <td>$4.71</td>
      <td>2</td>
      <td>$9.42</td>
    </tr>
    <tr>
      <th>3</th>
      <td>111</td>
      <td>Misery's End</td>
      <td>$1.79</td>
      <td>2</td>
      <td>$3.58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>$4.11</td>
      <td>2</td>
      <td>$8.22</td>
    </tr>
  </tbody>
</table>
</div>




```python
####################****Most Profitable Items****####################

#pop_items_df = total_df.groupby(["Item ID", "Item Name","Price"])
pop_items_total = pop_items_df["Price"].sum()
pop_items_total = pop_items_total.sort_values(ascending=False)

pop_items_profit = pop_items_total.iloc[:5]
pop_items_profit = pd.DataFrame(pop_items_profit)

pop_items_profit.rename(columns={"Price":"Total Purchase Value"}, inplace=True)
pop_items_profit = pop_items_profit.reset_index()

pop_items_profit["Purchase Count"] = (pop_items_profit["Total Purchase Value"]/pop_items_profit["Price"])
pop_items_profit["Price"] = pop_items_profit["Price"].map('${:,.2f}'.format)
pop_items_profit["Total Purchase Value"] = pop_items_profit["Total Purchase Value"].map('${:,.2f}'.format)

pop_items_profit.rename(columns={"Price":"Item Price"}, inplace=True)

pop_items_profit

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
      <th>Purchase Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>$3.64</td>
      <td>$10.92</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>117</td>
      <td>Heartstriker, Legacy of the Light</td>
      <td>$4.71</td>
      <td>$9.42</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>$4.49</td>
      <td>$8.98</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>$4.12</td>
      <td>$8.24</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>$4.11</td>
      <td>$8.22</td>
      <td>2.0</td>
    </tr>
  </tbody>
</table>
</div>


