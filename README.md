
# Heroes Of Pymoli Data Analysis
OBSERVED TREND 1: There are more males than females and other/nondisclosed genders that play Heroes of Pymoli.

OBSERVED TREND 2: The most common age range for individuals who play Heroes of Pymoli is 20-24. 

OBSERVED TREND 3: Among the three gender categories (male, female, others/nondisclosed), females had the highest purchase count and total purchase value. 


```python
#Dependencies 
import os
import pandas as pd
import numpy as np

#read json file
json_file = os.path.join("purchase_data.json")
purchase_df = pd.read_json(json_file)
```

# Player Count


```python
#Total Players
total_players = purchase_df["SN"].nunique()
total_players_df = pd.DataFrame([{"Total Players": total_players}])
total_players_df
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
      <th>Total Players</th>
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



# Purchasing Analysis (Total)


```python
#PURCHASING ANALYSIS (Total)
unique_items_count = purchase_df["Item ID"].nunique()
total_purchases = len(purchase_df)
total_revenue = purchase_df["Price"].sum()
average_purchase_price = total_revenue/total_purchases
purchasing_analysis = pd.DataFrame([{"Number of Unique Items":unique_items_count,
                                     "Average Price" : average_purchase_price,
                                     "Number of Purchases": total_purchases,
                                    "Total Revenue": total_revenue}])
purchasing_analysis = purchasing_analysis[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]
purchasing_analysis["Total Revenue"] = purchasing_analysis["Total Revenue"].map('${:,.2f}'.format)
purchasing_analysis["Average Price"] = purchasing_analysis["Average Price"].map('${:,.2f}'.format)

purchasing_analysis
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
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



# Gender Demographics


```python
#GENDER DEMOGRAPHICS 
#Creating a DataFrame
condensed_df= purchase_df[["Gender", "SN"]]
gender_sn_df = condensed_df.drop_duplicates("SN")
#Percentage & Count of Male Players
male_players = gender_sn_df.loc[purchase_df["Gender"] == "Male"]
male_players_count = len(male_players)
male_players_percentage = round((male_players_count/total_players) * 100, 2)
#Percentage & Count of Female Players
female_players = gender_sn_df.loc[purchase_df["Gender"] == "Female"]
female_players_count = len(female_players)
female_players_percentage = round((female_players_count/total_players) * 100, 2)
#Percentage & Count of Others/ Non-Disclosed
gender_sn_df["Gender"].unique()
other_players = gender_sn_df.loc[purchase_df["Gender"] == "Other / Non-Disclosed"]
other_players_count = len(other_players)
other_players_percentage = round((other_players_count/total_players) * 100, 2)
#Gender Demographics Final
gender_demographic_data = {"Percentage of Players": [male_players_percentage, female_players_percentage, other_players_percentage],
                                         "Total Count": [male_players_count, female_players_count, other_players_count]}
gender_demographic = pd.DataFrame(gender_demographic_data, index = ["Male", "Female", "Other / Non-Disclosed"])
gender_demographic
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Gender)


```python
#Purchasing Analysis(Gender)
#dataframes
female_df = purchase_df.loc[purchase_df["Gender"] == "Female"]
male_df = purchase_df.loc[purchase_df["Gender"] == "Male"]
other_df = purchase_df.loc[purchase_df["Gender"] == "Other / Non-Disclosed"]
#female stats
female_purchase_count = len(female_df)
female_total_purchase_value= female_df["Price"].sum()
female_average_purchase_price = female_total_purchase_value/female_purchase_count
female_normalized_totals = female_total_purchase_value/female_players_count
#male stats
male_purchase_count = len(male_df)
male_total_purchase_value= male_df["Price"].sum()
male_average_purchase_price = male_total_purchase_value/male_purchase_count
male_normalized_totals = male_total_purchase_value/male_players_count
#other stats
other_purchase_count = len(other_df)
other_total_purchase_value = other_df["Price"].sum()
other_average_purchase_price = other_total_purchase_value/other_purchase_count
other_normalized_totals = other_total_purchase_value/other_players_count
#Purchasing Analysis(Gender) Final
purchasing_analysis_gender = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"], 
                             "Purchase Count": [female_purchase_count, male_purchase_count, other_players_count], 
                             "Average Purchase Price":[female_average_purchase_price, male_average_purchase_price, other_average_purchase_price],
                             "Total Purchase Value":[female_total_purchase_value, male_total_purchase_value, other_total_purchase_value],
                            "Normalized Totals": [female_normalized_totals, male_normalized_totals, other_normalized_totals]
                             })
purchasing_analysis_gender = purchasing_analysis_gender[["Gender", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
purchasing_analysis_gender.set_index("Gender")
purchasing_analysis_gender["Total Purchase Value"] = purchasing_analysis_gender["Total Purchase Value"].map('${:,.2f}'.format)
purchasing_analysis_gender["Average Purchase Price"] = purchasing_analysis_gender["Average Purchase Price"].map('${:,.2f}'.format)
purchasing_analysis_gender["Normalized Totals"] = purchasing_analysis_gender["Normalized Totals"].map('${:,.2f}'.format)
purchasing_analysis_gender
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
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>8</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>



# Age Demographics


```python
#Age Binning
bins = [0, 10, 15, 20, 25, 30, 35, 40, 200]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
purchase_df["Age Groups"] = pd.cut(purchase_df["Age"], bins, labels=group_names)
#Age df
condensed_age_df= purchase_df[["SN", "Age Groups"]]
age_sn_df = condensed_age_df.drop_duplicates("SN")
#Age Counts
a = np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "<10"]))
b= np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "10-14"]))
c= np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "15-19"]))
d= np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "20-24"]))
e = np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "25-29"]))
f = np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "30-34"]))
g = np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "35-39"]))
h = np.float64(len(age_sn_df.loc[age_sn_df["Age Groups"] == "40+"]))
#Age Percentages
a_p =round((a/total_players)*100, 2)
b_p = round((b/total_players)*100, 2)
c_p = round((c/total_players)*100, 2)
d_p = round((d/total_players)*100, 2)
e_p = round((e/total_players)*100, 2)
f_p= round((f/total_players)*100, 2)
g_p = round((g/total_players)*100, 2)
h_p = round((h/total_players)*100, 2)
#Final Age Demographics 
age_data = {"Percentage of Players": [a_p, b_p, c_p, d_p, e_p, f_p, g_p, h_p],
            "Total Count": [a, b, c, d, e, f, g, h]}
age_demographic = pd.DataFrame(age_data, index = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"])
age_demographic["Total Count"] = age_demographic["Total Count"].astype(int)
age_demographic
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>9.42</td>
      <td>54</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.26</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.84</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.08</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Age)


```python
#Purchasing Analysis (Age)
#dataframes 
a_df = purchase_df.loc[purchase_df["Age Groups"] == "<10"]
b_df = purchase_df.loc[purchase_df["Age Groups"] == "10-14"]
c_df = purchase_df.loc[purchase_df["Age Groups"] == "15-19"]
d_df = purchase_df.loc[purchase_df["Age Groups"] == "20-24"]
e_df = purchase_df.loc[purchase_df["Age Groups"] == "25-29"]
f_df = purchase_df.loc[purchase_df["Age Groups"] == "30-34"]
g_df = purchase_df.loc[purchase_df["Age Groups"] == "35-39"]
h_df = purchase_df.loc[purchase_df["Age Groups"] == "40+"]

#Purchase Count
a_pc = np.float64(len(a_df))
b_pc = np.float64(len(b_df))
c_pc = np.float64(len(c_df))
d_pc = np.float64(len(d_df))
e_pc = np.float64(len(e_df))
f_pc = np.float64(len(f_df))
g_pc = np.float64(len(g_df))
h_pc = np.float64(len(h_df))

#Total Purchase Value 
a_pv = a_df["Price"].sum()
b_pv = b_df["Price"].sum()
c_pv = c_df["Price"].sum()
d_pv = d_df["Price"].sum()
e_pv = e_df["Price"].sum()
f_pv = f_df["Price"].sum()
g_pv = g_df["Price"].sum()
h_pv = h_df["Price"].sum()

#average purchase price
a_av = a_pv/a_pc
b_av = b_pv/b_pc
c_av = c_pv/c_pc
d_av = d_pv/d_pc
e_av = e_pv/e_pc
f_av = f_pv/f_pc
g_av = g_pv/g_pc
h_av = h_pv/h_pc


#Normalized Totals 
a_nt = a_pv/a
b_nt = b_pv/b
c_nt = c_pv/c
d_nt = d_pv/d
e_nt = e_pv/e
f_nt = f_pv/f
g_nt = g_pv/g
h_nt = h_pv/h

#Converting purchase counts to integers

#Final Purchasing Analysis(Age)
age_purchase_data = {"Purchase Count": [a_pc, b_pc, c_pc, d_pc, e_pc, f_pc, g_pc, h_pc],
                     "Average Purchase Price": [a_av, b_av, c_av, d_av, e_av, f_av, g_av, h_av],
                    "Total Purchase Value": [a_pv, b_pv, c_pv, d_pv, e_pv, f_pv, g_pv, h_pv],
                    "Normalized Totals":[a_nt, b_nt, c_nt, d_nt, e_nt, f_nt, g_nt, h_nt] }
age_purchase_analysis = pd.DataFrame(age_purchase_data, index = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"])

age_purchase_analysis = age_purchase_analysis[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
age_purchase_analysis["Purchase Count"] = age_purchase_analysis["Purchase Count"].astype(int)
age_purchase_analysis["Average Purchase Price"] = age_purchase_analysis["Average Purchase Price"].map('${:,.2f}'.format)
age_purchase_analysis["Total Purchase Value"] = age_purchase_analysis["Total Purchase Value"].map('${:,.2f}'.format)
age_purchase_analysis["Normalized Totals"] = age_purchase_analysis["Normalized Totals"].map('${:,.2f}'.format)
age_purchase_analysis
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
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>78</td>
      <td>$2.87</td>
      <td>$224.15</td>
      <td>$4.15</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
      <td>$3.80</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
      <td>$4.23</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
      <td>$4.05</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
      <td>$2.90</td>
      <td>$127.49</td>
      <td>$5.10</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
      <td>$2.88</td>
    </tr>
  </tbody>
</table>
</div>



# Top Spenders


```python
#Top Spenders
#creating purchase count
purchase_df["Purchase Count"] = purchase_df.groupby(["SN"])["SN"].transform("count")

#Gathering the top 5
top_spenders_data = purchase_df.groupby(by = ["SN", "Purchase Count"])["Price"].sum()
top_5 = top_spenders_data.nlargest(5)

#Renaming Total Purchase Value
top_5_df = pd.DataFrame(top_5).rename(columns={"Price": "Total Purchase Value"})

#Reseting index
top_5_df = top_5_df.reset_index().set_index(['SN'])

#Getting the Average Purchase Price
top_5_df["Average Purchase Price"] = top_5_df["Total Purchase Value"] / top_5_df["Purchase Count"]


#Formatting
top_5_df["Total Purchase Value"] = top_5_df["Total Purchase Value"].map('${:,.2f}'.format)
top_5_df["Average Purchase Price"] = top_5_df["Average Purchase Price"].map('${:,.2f}'.format)
top_5_df = top_5_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

#Final Result
top_5_df


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
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items 


```python
#Most Popular Items 
popular_items= purchase_df.groupby(["Item ID", "Item Name", "Price"])["Item ID"].count()

#Gathering top 5
top_items = popular_items.nlargest(5)

#Formatting
top_items_df = pd.DataFrame(top_items).rename(columns={"Item ID":"Purchase Count"})
top_items_df = top_items_df.reset_index().set_index(['Item ID', "Item Name"])
top_items_df = top_items_df.rename(columns = {"Price":"Item Price"})

#Calculating Total Purchase Value
top_items_df["Total Purchase Value"] = top_items_df["Item Price"] * top_items_df["Purchase Count"]

#Adding Dollar Signs & Formatting
top_items_df["Total Purchase Value"] = top_items_df["Total Purchase Value"].map('${:,.2f}'.format)
top_items_df["Item Price"] = top_items_df["Item Price"].map('${:,.2f}'.format)
top_items_df = top_items_df[["Purchase Count", "Item Price", "Total Purchase Value"]]

#Final Result
top_items_df
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
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>



# Most Profitable Items


```python
#Most Profitable Items 
purchase_df["Item Count"] = purchase_df.groupby(["Item ID"])["Item ID"].transform("count")
profitable_items = purchase_df.groupby(["Item ID", "Item Name", "Price", "Item Count"])["Price"].sum()

#Gathering top 5
top_profit_items = profitable_items.nlargest(5)

#Formatting
top_profit_df = pd.DataFrame(top_profit_items).rename(columns={"Price":"Total Purchase Value"})
top_profit_df = top_profit_df.reset_index().set_index(['Item ID', "Item Name"])
top_profit_df = top_profit_df.rename(columns = {"Price":"Item Price", "Item Count": "Purchase Count"})

top_profit_df["Total Purchase Value"] = top_profit_df["Total Purchase Value"].map('${:,.2f}'.format)
top_profit_df["Item Price"] = top_profit_df["Item Price"].map('${:,.2f}'.format)

#Final 
top_profit_df = top_profit_df[["Purchase Count", "Item Price", "Total Purchase Value"]]
top_profit_df

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


