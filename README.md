
# Heroes Of Pymoli Data Analysis
Of the 573 active players, the vast majority are male (81.2%). There also exists, a smaller, but notable proportion of female players (17.4%).

Our peak age demographic falls between 20-24 (58.6%), with secondary groups falling between 15-19 (23.2%), and 25-29 (21.2%).


```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
```


```python
# Import json into data frame
purchase_data_df = pd.read_json("purchase_data.json")
purchase_data_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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



## Player Count


```python
# Count the total number of players by grouping the unique SN's
players_df = purchase_data_df.groupby("SN")["SN"].unique()
number_of_players = players_df.count() 

# Display number of players in dataframe 
players_df = pd.DataFrame([{ "Number of Players": number_of_players}])
players_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
# Number of Unique Items
item_count = len(purchase_data_df["Item ID"].unique())

# Average Purchase Price: mean of all purchases
average_price_df = purchase_data_df ["Price"].mean()

# Total Number of Purchases: count of all purchases
total_purchases_df = purchase_data_df["Price"].count()
total_purchases_df

# Total Revenue: sum of all purchases
total_revenue_df = purchase_data_df["Price"].sum()

# Create data frame
purchasing_analysis_df = pd.DataFrame({ "Number of Unique Items" : [item_count],
                                        "Average Price" : [round(average_price_df, 2)],
                                        "Total Number of Purchases" : [total_purchases_df],
                                        "Total Revenue" : [total_revenue_df]})

# Presenting the data  
purchasing_analysis_df ["Average Price"] = purchasing_analysis_df["Average Price"].map("${:,.2f}".format)
purchasing_analysis_df ["Total Number of Purchases"] = purchasing_analysis_df["Total Number of Purchases"].map("{:,}".format)
purchasing_analysis_df ["Total Revenue"] = purchasing_analysis_df["Total Revenue"].map("${:,.2f}".format)
purchasing_analysis_df = purchasing_analysis_df.loc[:,["Number of Unique Items", "Average Price", "Total Number of Purchases", "Total Revenue"]]
purchasing_analysis_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
# Drop duplicate players names 
no_duplicate_players = purchase_data_df.drop_duplicates(['SN'], keep ='last')

# Count and percentage of Male Players
male_df = purchase_data_df.loc[purchase_data_df["Gender"] == "Male",:]
male_count = len(male_df["SN"].unique())
percent_male = round((len(male_data_df)/len(purchase_data_df)) * 100, 2)

# Count  and percentage of Female Players
female_df = purchase_data_df.loc[purchase_data_df["Gender"] == "Female",:]
female_count = len(female_df["SN"].unique())
percent_female = round((len(female_data_df)/len(purchase_data_df)) * 100, 2)

# Count and percentage of Other / Non-Disclosed
others_data_df = purchase_data_df.loc[purchase_data_df["Gender"] == "Other / Non-Disclosed",:]
others_count = len(others_data_df["SN"].unique())
percent_others = round((len(others_data_df)/len(purchase_data_df)) * 100, 2)

# Creating a total gender dataframe of counts and percentages
gender_demo_dict = {"Percentage Of Players" : [percent_male, percent_female, percent_others],
                    "Gender" : ["Male","Female","Other/Non-Disclosed"],
                    "Total Count" : [male_count, female_count, others_count]}

# Create as dataframe 
gender_demo_df = pd.DataFrame(gender_demo_dict)

# Present data frame 
gender_demo_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Percentage Of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>17.44</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other/Non-Disclosed</td>
      <td>1.41</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
# Purchase Count of Males, Females, and Others/ Non-disclosed
male_purchase = len (male_data_df)
female_purchase = len (female_data_df)
others_purchase = len (others_data_df)

# Average Purchase Price of Males, Females, and Others/ Non-disclosed
average_price_male = round((male_data_df["Price"].sum())/len(male_data_df["Price"]),2)
average_price_female =round((female_data_df["Price"].sum())/len(female_data_df["Price"]),2)
average_price_others = round((others_data_df["Price"].sum())/len(others_data_df["Price"]),2)

# Total Purchase Value of Males, Females, and Others/ Non-disclosed
total_value_male = round(male_data_df["Price"].sum(),2)
total_value_female = round(female_data_df["Price"].sum(),2)
total_value_others = round(others_data_df["Price"].sum(),2)

# Normalized Totals of Males, Females, and Others/ Non-disclosed
normalized_male = round((total_value_male/male_count), 2)
normalized_female = round((total_value_female/female_count), 2)
normalized_others = round((total_value_others/others_count), 2)

#Creating a total purchasing analysis (gender) dataframe
purchasing_gender = {"Purchase Count" : [male_purchase, female_purchase, others_purchase],
                     "Gender" : ["Male","Female","Other/Non-Disclosed"],
                     "Average Purchase Price" : [average_price_male, average_price_female, average_price_others],
                     "Total Purchase Value" : [total_value_male, total_value_female,total_value_others ],
                     "Normalized Totals" : [normalized_male, normalized_female, normalized_others ]}

purchasing_gender_df = pd.DataFrame(purchasing_gender)
purchasing_gender_df = purchasing_gender_df.set_index("Gender")

# Presenting the data
purchasing_gender_df["Average Purchase Price"] = purchasing_gender_df["Average Purchase Price"].map("${:,.2f}".format)
purchasing_gender_df["Total Purchase Vale"] = purchasing_gender_df["Total Purchase Value"].map("${:,.2f}".format)
purchasing_gender_df ["Purchase Count"] = purchasing_gender_df["Purchase Count"].map("{:,}".format)
purchasing_gender_df["Normalized Totals"] = purchasing_gender_df["Normalized Totals"].map("${:,.2f}".format)
purchasing_gender_df = purchasing_gender_df.loc[:, ["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]

# Display the Gender Table
purchasing_gender_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
# Establish the bins to put the ages into
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Categorize the existing players using the age bins
player_demographics["Age Ranges"] = pd.cut(player_demographics["Age"], age_bins, labels=group_names)

# Calculate the Numbers and Percentages by Age Group
age_demographics_totals = player_demographics["Age Ranges"].value_counts()
age_demographics_percents = age_demographics_totals / num_players * 100
age_demographics = pd.DataFrame({"Total Count": age_demographics_totals, "Percentage of Players": age_demographics_percents})

# Presenting the data
age_demographics = age_demographics.round(2)

# Display Age Demographics Table
age_demographics.sort_index()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>4.89</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>6.11</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>23.21</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>58.64</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>21.82</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>11.17</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>7.33</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.97</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
# Bin the Purchasing Data
purchase_data_df["Age Ranges"] = pd.cut(purchase_data_df["Age"], age_bins, labels=group_names)

# Run basic calculations
age_purchase_total = purchase_data_df.groupby(["Age Ranges"]).sum()["Price"].rename("Total Purchase Value")
age_average = purchase_data_df.groupby(["Age Ranges"]).mean()["Price"].rename("Average Purchase Price")
age_counts = purchase_data_df.groupby(["Age Ranges"]).count()["Price"].rename("Purchase Count")

# Calculate Normalized Purchasing
normalized_total = age_purchase_total / age_demographics["Total Count"]

# Convert to a Purchasing Analysis (age) DataFrame
age_data = pd.DataFrame({"Purchase Count": age_counts, "Average Purchase Price": age_average, "Total Purchase Value": age_purchase_total, "Normalized Totals": normalized_total})

# Presenting the Data
age_data["Average Purchase Price"] = age_data["Average Purchase Price"].map("${:,.2f}".format)
age_data["Total Purchase Value"] = age_data["Total Purchase Value"].map("${:,.2f}".format)
age_data ["Purchase Count"] = age_data["Purchase Count"].map("{:,}".format)
age_data["Normalized Totals"] = age_data["Normalized Totals"].map("${:,.2f}".format)
age_data = age_data.loc[:, ["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]

# Display the Age Table
age_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$2.77</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$3.08</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$2.84</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$3.16</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$2.98</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders: Identify the the top 5 spenders in the game by total purchase value, then list (in a table)


```python
# Calculate Total purchase value
user_total = purchase_data_df.groupby(["SN"]).sum()["Price"]

# Calculate average purchase price
user_average = purchase_data_df.groupby(["SN"]).mean()["Price"]

# Calculate puchase count 
user_count = purchase_data_df.groupby(["SN"]).count()["Price"]

# Creating a dictionary of top spenders
top_spenders_dict = {"SN" : spenders_list,
                     "Purchase Count" : user_count,
                     "Average Purchase Price" : user_average,
                     "Total Purchase Value": user_total}

# Create a dataframe for Top 5 Spenders
top_spenders_df = pd.DataFrame(top_spenders_dict)
top_spenders_df = top_spenders_df.set_index("SN")
top_spenders_df = top_spenders_df.sort_values("Total Purchase Value",ascending=False)

# Presenting the Data
top_spenders_df["Average Purchase Price"] = top_spenders_df["Average Purchase Price"].map("${:,.2f}".format)
top_spenders_df["Total Purchase Value"] = top_spenders_df["Total Purchase Value"].map("${:,.2f}".format)
top_spenders_df= top_spenders_df.loc[:,["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

#Display the head of the dataframe
top_spenders_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <th>[Undirrala66]</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>[Saedue76]</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>[Mindimnya67]</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>[Haellysu29]</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>[Eoda93]</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items: Identify the 5 most popular items by purchase count, then list (in a table)


```python
# Extract item Data
item_data = purchase_data_df.loc[:,["Item ID", "Item Name", "Price"]]

# Perform calculations 
total_item_purchase = item_data.groupby(["Item ID", "Item Name"]).sum()["Price"]
average_item_purchase = item_data.groupby(["Item ID", "Item Name"]).mean()["Price"]
item_count = item_data.groupby(["Item ID", "Item Name"]).count()["Price"]

# Create new DataFrame
item_data_pd = pd.DataFrame({"Total Purchase Value": total_item_purchase, 
                             "Item Price": average_item_purchase, 
                             "Purchase Count": item_count})

# Sort Values
item_data_count_sorted = item_data_pd.sort_values("Purchase Count", ascending = False)

# Present the data
item_data_count_sorted["Item Price"] = item_data_count_sorted["Item Price"].map("${:,.2f}".format)
item_data_count_sorted["Purchase Count"] = item_data_count_sorted["Purchase Count"].map("{:,}".format)
item_data_count_sorted["Total Purchase Value"] = item_data_count_sorted["Total Purchase Value"].map("${:,.2f}".format)
item_popularity = item_data_count_sorted.loc[:,["Purchase Count", "Item Price", "Total Purchase Value"]]

# Display the head of the data 
item_popularity.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
# Item Table (Sorted by Total Purchase Value)
item_total_purchase = item_data_pd.sort_values("Total Purchase Value", ascending = False)

# Presenting the data
item_total_purchase["Item Price"] = item_total_purchase["Item Price"].map("${:,.2f}".format)
item_total_purchase["Purchase Count"] = item_total_purchase["Purchase Count"].map("{:,}".format)
item_total_purchase["Total Purchase Value"] = item_total_purchase["Total Purchase Value"].map("${:,.2f}".format)
item_profitable = item_total_purchase.loc[:,["Purchase Count", "Item Price", "Total Purchase Value"]]

# Display the head of the data 
item_profitable.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


