
# Three observable trends based on the data
1)  Most players are males by a large amount.
2)  Ages 20 - 24 make the most purchases.
3)  The average item purchase price is $2.82 to $3.25.


```python
# Import modules
import pandas as pd
import json
import os
```


```python
# Load JSON File
file = os.path.join('purchase_data.json')

data = pd.read_json(file)
```


```python
# Check JSON File
data.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



# Player Count


```python
# Player Count
# Total Number of Players

player_count = len(data['SN'].unique())

players_df = pd.DataFrame([{'Total Players': player_count}])

players_df.set_index('Total Players', inplace = True)
players_df
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
    </tr>
    <tr>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>573</th>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Total)


```python
# Purchasing Analysis (Total)
# Number of Unique Items
# Average Purchase Price
# Total Number of Purchases
# Total Revenue

no_dup_items = data.drop_duplicates(['Item ID'], keep = 'last')

total_unique = len(no_dup_items)

total_pur = data['Price'].count()

total_rev = round(data['Price'].sum(),2)

avg_price = round(total_rev/total_pur, 2)

pur_analysis = pd.DataFrame([{
    
    "Number of Unique Items": total_unique,
    'Average Purchase Price': avg_price,
    'Total Purchases': total_pur,
    'Total Revenue': total_rev
}])

pur_analysis.style.format({'Average Purchase Price': '${:.2f}', 'Total Revenue': '${:,.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_2eeb234c_dc88_11e7_b8fc_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Number of Unique Items</th> 
        <th class="col_heading level0 col2" >Total Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_2eeb234c_dc88_11e7_b8fc_7c04d0bccd24level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_2eeb234c_dc88_11e7_b8fc_7c04d0bccd24row0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_2eeb234c_dc88_11e7_b8fc_7c04d0bccd24row0_col1" class="data row0 col1" >183</td> 
        <td id="T_2eeb234c_dc88_11e7_b8fc_7c04d0bccd24row0_col2" class="data row0 col2" >780</td> 
        <td id="T_2eeb234c_dc88_11e7_b8fc_7c04d0bccd24row0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 



# Gender Demographics


```python
# Gender Demographics
# Percentage and Count of Male Players
# Percentage and Count of Female Players
#Percentage and Count of Other / Non-Disclosed

no_dup_players = data.drop_duplicates(['SN'], keep ='last')

gender_counts = no_dup_players['Gender'].value_counts().reset_index()

gender_counts['% of Players'] = gender_counts['Gender']/player_count * 100

gender_counts.rename(columns = {'index': 'Gender', 'Gender': '# of Players'}, inplace = True)

gender_counts.set_index(['Gender'], inplace = True)

gender_counts.style.format({"% of Players": "{:.1f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Players</th> 
        <th class="col_heading level0 col1" >% of Players</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24row0_col0" class="data row0 col0" >465</td> 
        <td id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24row0_col1" class="data row0 col1" >81.2%</td> 
    </tr>    <tr> 
        <th id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24row1_col0" class="data row1 col0" >100</td> 
        <td id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24row1_col1" class="data row1 col1" >17.5%</td> 
    </tr>    <tr> 
        <th id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24row2_col0" class="data row2 col0" >8</td> 
        <td id="T_2f55261e_dc88_11e7_b3d3_7c04d0bccd24row2_col1" class="data row2 col1" >1.4%</td> 
    </tr></tbody> 
</table> 



# Purchasing Analysis (Gender)


```python
# Purchasing Analysis (Gender)
# The below each broken by gender
# Purchase Count
# Average Purchase Price
# Total Purchase Value
# Normalized Totals

count_by_gen = pd.DataFrame(data.groupby('Gender')['Gender'].count())

total_by_gen = pd.DataFrame(data.groupby('Gender')['Price'].sum())

analysis_gen = pd.merge(count_by_gen, total_by_gen, left_index = True, right_index = True)

analysis_gen.rename(columns = {'Gender': '# of Purchases', 'Price':'Total Purchase Value'}, inplace=True)

analysis_gen['Average Purchase Price'] = analysis_gen['Total Purchase Value']/analysis_gen['# of Purchases']

analysis_gen = analysis_gen.merge(gender_counts, left_index = True, right_index = True)

analysis_gen['Normalized Totals'] = analysis_gen['Total Purchase Value']/analysis_gen['# of Players']
analysis_gen

del analysis_gen['% of Players']
del analysis_gen['# of Players']

analysis_gen.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}', 'Normalized Totals': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Purchases</th> 
        <th class="col_heading level0 col1" >Total Purchase Value</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row0_col0" class="data row0 col0" >136</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row0_col1" class="data row0 col1" >$382.91</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row0_col2" class="data row0 col2" >$2.82</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row0_col3" class="data row0 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row1_col0" class="data row1 col0" >633</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row1_col1" class="data row1 col1" >$1867.68</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row1_col2" class="data row1 col2" >$2.95</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row1_col3" class="data row1 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row2_col0" class="data row2 col0" >11</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row2_col1" class="data row2 col1" >$35.74</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row2_col2" class="data row2 col2" >$3.25</td> 
        <td id="T_2fca82ee_dc88_11e7_8c47_7c04d0bccd24row2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 



# Age Demographics


```python
# Age Demographics
# The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
# Purchase Count
# Average Purchase Price
# Total Purchase Value
# Normalized Totals

data.loc[(data['Age'] < 10), 'age_bin'] = "< 10"
data.loc[(data['Age'] >= 10) & (data['Age'] <= 14), 'age_bin'] = "10 - 14"
data.loc[(data['Age'] >= 15) & (data['Age'] <= 19), 'age_bin'] = "15 - 19"
data.loc[(data['Age'] >= 20) & (data['Age'] <= 24), 'age_bin'] = "20 - 24"
data.loc[(data['Age'] >= 25) & (data['Age'] <= 29), 'age_bin'] = "25 - 29"
data.loc[(data['Age'] >= 30) & (data['Age'] <= 34), 'age_bin'] = "30 - 34"
data.loc[(data['Age'] >= 35) & (data['Age'] <= 39), 'age_bin'] = "35 - 39"
data.loc[(data['Age'] >= 40), 'age_bin'] = "> 40"

count_age = pd.DataFrame(data.groupby('age_bin')['SN'].count())

avg_price_age = pd.DataFrame(data.groupby('age_bin')['Price'].mean())

tot_pur_age = pd.DataFrame(data.groupby('age_bin')['Price'].sum())

no_dup_age = pd.DataFrame(data.drop_duplicates('SN', keep = 'last').groupby('age_bin')['SN'].count())

merge_age = pd.merge(count_age, avg_price_age, left_index = True, right_index = True).merge(tot_pur_age, left_index = True, right_index = True).merge(no_dup_age, left_index = True, right_index = True)

merge_age.rename(columns = {"SN_x": "# of Purchases", "Price_x": "Average Purchase Price", "Price_y": "Total Purchase Value", "SN_y": "# of Purchasers"}, inplace = True)

merge_age['Normalized Totals'] = merge_age['Total Purchase Value']/merge_age['# of Purchasers']

merge_age.index.rename("Age", inplace = True)

merge_age.style.format({'Average Purchase Price': '${:.2f}', 'Total Purchase Value': '${:.2f}', 'Normalized Totals': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Purchases</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" ># of Purchasers</th> 
        <th class="col_heading level0 col4" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row0" class="row_heading level0 row0" >10 - 14</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row0_col0" class="data row0 col0" >35</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row0_col1" class="data row0 col1" >$2.77</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row0_col2" class="data row0 col2" >$96.95</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row0_col3" class="data row0 col3" >23</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row0_col4" class="data row0 col4" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row1" class="row_heading level0 row1" >15 - 19</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row1_col0" class="data row1 col0" >133</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row1_col1" class="data row1 col1" >$2.91</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row1_col2" class="data row1 col2" >$386.42</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row1_col3" class="data row1 col3" >100</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row1_col4" class="data row1 col4" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row2" class="row_heading level0 row2" >20 - 24</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row2_col0" class="data row2 col0" >336</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row2_col2" class="data row2 col2" >$978.77</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row2_col3" class="data row2 col3" >259</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row2_col4" class="data row2 col4" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row3" class="row_heading level0 row3" >25 - 29</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row3_col0" class="data row3 col0" >125</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row3_col1" class="data row3 col1" >$2.96</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row3_col2" class="data row3 col2" >$370.33</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row3_col3" class="data row3 col3" >87</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row3_col4" class="data row3 col4" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row4" class="row_heading level0 row4" >30 - 34</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row4_col0" class="data row4 col0" >64</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row4_col1" class="data row4 col1" >$3.08</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row4_col2" class="data row4 col2" >$197.25</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row4_col3" class="data row4 col3" >47</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row4_col4" class="data row4 col4" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row5" class="row_heading level0 row5" >35 - 39</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row5_col0" class="data row5 col0" >42</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row5_col1" class="data row5 col1" >$2.84</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row5_col2" class="data row5 col2" >$119.40</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row5_col3" class="data row5 col3" >27</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row5_col4" class="data row5 col4" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row6" class="row_heading level0 row6" >< 10</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row6_col0" class="data row6 col0" >28</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row6_col1" class="data row6 col1" >$2.98</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row6_col2" class="data row6 col2" >$83.46</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row6_col3" class="data row6 col3" >19</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row6_col4" class="data row6 col4" >$4.39</td> 
    </tr>    <tr> 
        <th id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24level0_row7" class="row_heading level0 row7" >> 40</th> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row7_col0" class="data row7 col0" >17</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row7_col1" class="data row7 col1" >$3.16</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row7_col2" class="data row7 col2" >$53.75</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row7_col3" class="data row7 col3" >11</td> 
        <td id="T_306a04fe_dc88_11e7_be40_7c04d0bccd24row7_col4" class="data row7 col4" >$4.89</td> 
    </tr></tbody> 
</table> 



# Top Spenders


```python
# Top Spenders
# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
# SN
# Purchase Count
# Average Purchase Price
# Total Purchase Value

purchase_amt_by_SN = pd.DataFrame(data.groupby('SN')['Price'].sum())
num_purchase_by_SN = pd.DataFrame(data.groupby('SN')['Price'].count())
avg_purchase_by_SN = pd.DataFrame(data.groupby('SN')['Price'].mean())

merged_top5 = pd.merge(purchase_amt_by_SN, num_purchase_by_SN, left_index = True, right_index = True).merge(avg_purchase_by_SN, left_index=True, right_index=True)

merged_top5.rename(columns = {'Price_x': 'Total Purchase Value', 'Price_y':'Purchase Count', 'Price':'Average Purchase Price'}, inplace = True)

merged_top5.sort_values('Total Purchase Value', ascending = False, inplace=True)

merged_top5 = merged_top5.head()

merged_top5.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Purchase Value</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24level0_row0" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row0_col0" class="data row0 col0" >$17.06</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row0_col1" class="data row0 col1" >5</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row0_col2" class="data row0 col2" >$3.41</td> 
    </tr>    <tr> 
        <th id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24level0_row1" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row1_col0" class="data row1 col0" >$13.56</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row1_col1" class="data row1 col1" >4</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row1_col2" class="data row1 col2" >$3.39</td> 
    </tr>    <tr> 
        <th id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24level0_row2" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row2_col0" class="data row2 col0" >$12.74</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row2_col1" class="data row2 col1" >4</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row2_col2" class="data row2 col2" >$3.18</td> 
    </tr>    <tr> 
        <th id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24level0_row3" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row3_col0" class="data row3 col0" >$12.73</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row3_col1" class="data row3 col1" >3</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row3_col2" class="data row3 col2" >$4.24</td> 
    </tr>    <tr> 
        <th id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24level0_row4" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row4_col0" class="data row4 col0" >$11.58</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row4_col1" class="data row4 col1" >3</td> 
        <td id="T_30e150ae_dc88_11e7_9a46_7c04d0bccd24row4_col2" class="data row4 col2" >$3.86</td> 
    </tr></tbody> 
</table> 



# Most Popular Items


```python
# Most Popular Items
# Identify the 5 most popular items by purchase count, then list (in a table):
# Item ID
# Item Name
# Purchase Count
# Item Price
# Total Purchase Value

top5_items_ID = pd.DataFrame(data.groupby('Item ID')['Item ID'].count())

top5_items_ID.sort_values('Item ID', ascending = False, inplace = True)

top5_items_ID = top5_items_ID.iloc[0:5][:]

top5_items_total = pd.DataFrame(data.groupby('Item ID')['Price'].sum())

top5_items = pd.merge(top5_items_ID, top5_items_total, left_index = True, right_index = True)

no_dup_items = data.drop_duplicates(['Item ID'], keep = 'last')

top5_merge_ID = pd.merge(top5_items, no_dup_items, left_index = True, right_on = 'Item ID')

top5_merge_ID = top5_merge_ID[['Item ID', 'Item Name', 'Item ID_x', 'Price_y', 'Price_x']]

top5_merge_ID.set_index(['Item ID'], inplace = True)

top5_merge_ID.rename(columns =  {'Item ID_x': 'Purchase Count', 'Price_y': 'Item Price', 'Price_x': 'Total Purchase Value'}, inplace=True)

top5_merge_ID.style.format({'Item Price': '${:.2f}', 'Total Purchase Value': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Item Price</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24level0_row0" class="row_heading level0 row0" >39</th> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row0_col0" class="data row0 col0" >Betrayal, Whisper of Grieving Widows</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row0_col1" class="data row0 col1" >11</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row0_col2" class="data row0 col2" >$2.35</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row0_col3" class="data row0 col3" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24level0_row1" class="row_heading level0 row1" >84</th> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row1_col0" class="data row1 col0" >Arcane Gem</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row1_col1" class="data row1 col1" >11</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row1_col2" class="data row1 col2" >$2.23</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row1_col3" class="data row1 col3" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24level0_row2" class="row_heading level0 row2" >31</th> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row2_col0" class="data row2 col0" >Trickster</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row2_col1" class="data row2 col1" >9</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row2_col2" class="data row2 col2" >$2.07</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row2_col3" class="data row2 col3" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24level0_row3" class="row_heading level0 row3" >175</th> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row3_col0" class="data row3 col0" >Woeful Adamantite Claymore</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row3_col1" class="data row3 col1" >9</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row3_col2" class="data row3 col2" >$1.24</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row3_col3" class="data row3 col3" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24level0_row4" class="row_heading level0 row4" >13</th> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row4_col0" class="data row4 col0" >Serenity</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row4_col1" class="data row4 col1" >9</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row4_col2" class="data row4 col2" >$1.49</td> 
        <td id="T_317b41f0_dc88_11e7_ac95_7c04d0bccd24row4_col3" class="data row4 col3" >$13.41</td> 
    </tr></tbody> 
</table> 



# Most Profitable Items


```python
# Most Profitable Items
# Identify the 5 most profitable items by total purchase value, then list (in a table):
# Item ID
# Item Name
# Purchase Count
# Item Price
# Total Purchase Value

top5_profit = pd.DataFrame(data.groupby('Item ID')['Price'].sum())
top5_profit.sort_values('Price', ascending = False, inplace = True)

top5_profit = top5_profit.iloc[0:5][:]

pur_count_profit = pd.DataFrame(data.groupby('Item ID')['Item ID'].count())

top5_profit = pd.merge(top5_profit, pur_count_profit, left_index = True, right_index = True, how = 'left')
top5_merge_profit = pd.merge(top5_profit, no_dup_items, left_index = True, right_on = 'Item ID', how = 'left')
top5_merge_profit = top5_merge_profit[['Item ID', 'Item Name', 'Item ID_x', 'Price_y','Price_x']]
top5_merge_profit.set_index(['Item ID'], inplace=True)
top5_merge_profit.rename(columns = {'Item ID_x': 'Purchase Count', 'Price_y': 'Item Price', 'Price_x': 'Total Purchase Value'}, inplace = True)
top5_merge_profit.style.format({'Item Price': '${:.2f}', 'Total Purchase Value': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Item Price</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24level0_row0" class="row_heading level0 row0" >34</th> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row0_col0" class="data row0 col0" >Retribution Axe</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row0_col1" class="data row0 col1" >9</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row0_col2" class="data row0 col2" >$4.14</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row0_col3" class="data row0 col3" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24level0_row1" class="row_heading level0 row1" >115</th> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row1_col0" class="data row1 col0" >Spectral Diamond Doomblade</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row1_col1" class="data row1 col1" >7</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row1_col2" class="data row1 col2" >$4.25</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row1_col3" class="data row1 col3" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24level0_row2" class="row_heading level0 row2" >32</th> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row2_col0" class="data row2 col0" >Orenmir</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row2_col1" class="data row2 col1" >6</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row2_col2" class="data row2 col2" >$4.95</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row2_col3" class="data row2 col3" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24level0_row3" class="row_heading level0 row3" >103</th> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row3_col0" class="data row3 col0" >Singed Scalpel</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row3_col1" class="data row3 col1" >6</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row3_col2" class="data row3 col2" >$4.87</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row3_col3" class="data row3 col3" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24level0_row4" class="row_heading level0 row4" >107</th> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row4_col0" class="data row4 col0" >Splitter, Foe Of Subtlety</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row4_col1" class="data row4 col1" >8</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row4_col2" class="data row4 col2" >$3.61</td> 
        <td id="T_3205fdfe_dc88_11e7_96ad_7c04d0bccd24row4_col3" class="data row4 col3" >$28.88</td> 
    </tr></tbody> 
</table> 


