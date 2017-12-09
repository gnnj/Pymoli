# Three observable trends based on the data
# 1)  Most players are males by a large amount.
# 2)  Ages 20 - 24 make the most purchases.
# 3)  The average item purchase price is $2.82 to $3.25.

# Import modules
import pandas as pd
import json
import os

# Load JSON File
file = os.path.join('purchase_data.json')

data = pd.read_json(file)

# Player Count
# Total Number of Players

player_count = len(data['SN'].unique())

players_df = pd.DataFrame([{'Total Players': player_count}])

players_df.set_index('Total Players', inplace = True)
players_df

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