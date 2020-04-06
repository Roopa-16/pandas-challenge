### Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).  
-----

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
```

## Player Count

* Display the total number of players



```python
playerDemographics = purchase_data.loc[:, ["Gender", "SN", "Age"]]
playerDemographics = playerDemographics.drop_duplicates()
numberOfPlayers = playerDemographics.count()[0]
pd.DataFrame({"Total Players": [numberOfPlayers]})
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
uniqueItems = len(purchase_data["Item ID"].unique())
averagePrice = purchase_data["Price"].mean()
numberOfPurchases = purchase_data["Price"].count()
totalRevenue = purchase_data["Price"].sum()

summaryTable = pd.DataFrame({"Number of Unique Items": uniqueItems,
                              "Total Revenue": [totalRevenue],
                              "Number of Purchases": [numberOfPurchases],
                              "Average Price": [averagePrice]})

summaryTable = summaryTable.round(2)
summaryTable ["Average Price"] = summaryTable["Average Price"].map("${:,.2f}".format)
summaryTable ["Number of Purchases"] = summaryTable["Number of Purchases"].map("{:,}".format)
summaryTable ["Total Revenue"] = summaryTable["Total Revenue"].map("${:,.2f}".format)
summaryTable = summaryTable.loc[:,["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]

summaryTable
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
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed





```python
totalCount = playerDemographics["Gender"].value_counts()
percentageOfPlayers = totalCount / numberOfPlayers * 100
genderDemographics = pd.DataFrame({ "Percentage of Players": percentageOfPlayers, "Total Count": totalCount})
genderDemographics = genderDemographics.round(2)
genderDemographics["Percentage of Players"] = genderDemographics["Percentage of Players"].map("{:.2f}%".format)
genderDemographics
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
      <th>Male</th>
      <td>84.03%</td>
      <td>484</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>14.06%</td>
      <td>81</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.91%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
totalPurchaseValue = purchase_data.groupby(["Gender"]).sum()["Price"].rename("Total Purchase Value")
averagePurchasePrice = purchase_data.groupby(["Gender"]).mean()["Price"].rename("Average Purchase Price")
purchaseCount = purchase_data.groupby(["Gender"]).count()["Price"].rename("Purchase Count")
averagePurchaseTotalPerPersonByGender = totalPurchaseValue / genderDemographics["Total Count"]


genderPurchasingAnalysis = pd.DataFrame({"Purchase Count": purchaseCount, "Average Purchase Price": averagePurchasePrice, "Total Purchase Value": totalPurchaseValue, "Average Purchase Total per Person by Gender": averagePurchaseTotalPerPersonByGender})


genderPurchasingAnalysis["Average Purchase Price"] = genderPurchasingAnalysis["Average Purchase Price"].map("${:,.2f}".format)
genderPurchasingAnalysis["Total Purchase Value"] = genderPurchasingAnalysis["Total Purchase Value"].map("${:,.2f}".format)
genderPurchasingAnalysis ["Purchase Count"] = genderPurchasingAnalysis["Purchase Count"].map("{:,}".format)
genderPurchasingAnalysis["Average Purchase Total per Person by Gender"] = genderPurchasingAnalysis["Average Purchase Total per Person by Gender"].map("${:,.2f}".format)
genderPurchasingAnalysis = genderPurchasingAnalysis.loc[:, ["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Average Purchase Total per Person by Gender"]]

genderPurchasingAnalysis
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
      <th>Average Purchase Total per Person by Gender</th>
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
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
ageBins = [0, 9.99, 14.99, 19.99, 24.99, 29.99, 34.99, 39.99, 99999]
groupNames = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

playerDemographics["Age Ranges"] = pd.cut(playerDemographics["Age"], ageBins, labels=groupNames)

totalCountAges= playerDemographics["Age Ranges"].value_counts()
PercentageOfPlayersAges = totalCountAges / numberOfPlayers * 100

ageDemographics = pd.DataFrame({"Total Count": totalCountAges, "Percentage of Players": PercentageOfPlayersAges})
ageDemographics = ageDemographics.round(2)
ageDemographics["Percentage of Players"] = ageDemographics["Percentage of Players"].map("{:.2f}%".format)

ageDemographics.sort_index()
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
purchase_data["Age Ranges"] = pd.cut(purchase_data["Age"], ageBins, labels=groupNames)
totalPurchaseValueAges = purchase_data.groupby(["Age Ranges"]).sum()["Price"].rename("Total Purchase Value")
averagePurchasePriceAges = purchase_data.groupby(["Age Ranges"]).mean()["Price"].rename("Average Purchase Price")
purchaseCountAges = purchase_data.groupby(["Age Ranges"]).count()["Price"].rename("Purchase Count")
averageTotalPurchasePerPersonAges = totalPurchaseValueAges / ageDemographics["Total Count"]

purchasingAnalysisAges = pd.DataFrame({"Purchase Count": purchaseCountAges, "Average Purchase Price": averagePurchasePriceAges, "Total Purchase Value": totalPurchaseValueAges, "Average Total Purchase per Person": averageTotalPurchasePerPersonAges})

purchasingAnalysisAges["Average Purchase Price"] = purchasingAnalysisAges["Average Purchase Price"].map("${:,.2f}".format)
purchasingAnalysisAges["Total Purchase Value"] = purchasingAnalysisAges["Total Purchase Value"].map("${:,.2f}".format)
purchasingAnalysisAges ["Purchase Count"] = purchasingAnalysisAges["Purchase Count"].map("{:,}".format)
purchasingAnalysisAges["Average Total Purchase per Person"] = purchasingAnalysisAges["Average Total Purchase per Person"].map("${:,.2f}".format)
purchasingAnalysisAges = purchasingAnalysisAges.loc[:, ["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Average Total Purchase per Person"]]

purchasingAnalysisAges
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
      <th>Average Total Purchase per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
groupBySN = purchase_data.groupby(["SN"])
purchaseCountSN = groupBySN["Price"].count()
averagePurchasePriceSN = groupBySN["Price"].mean()
totalPurchaseValueSN =groupBySN["Price"].sum()

topSpenders = pd.DataFrame({"Purchase Count": purchaseCountSN, "Average Purchase Price": averagePurchasePriceSN, "Total Purchase Value": totalPurchaseValueSN},
columns=["Purchase Count", "Average Purchase Price", "Total Purchase Value"])

topSpenders = topSpenders.sort_values(["Total Purchase Value"], ascending=False).head().round(2)
topSpenders["Average Purchase Price"] = topSpenders["Average Purchase Price"].map("${:.2f}".format)
topSpenders["Total Purchase Value"] = topSpenders["Total Purchase Value"].map("${:.2f}".format)

topSpenders
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
groupByItem = purchase_data.groupby(["Item ID", "Item Name"])
purchaseCountPopular = groupByItem["Price"].count()
itemPricePopular = groupByItem["Price"].mean()
totalPurchaseValuePopular = groupByItem["Price"].sum()

mostPopularItems = pd.DataFrame({"Purchase Count": purchaseCountPopular, "Item Price":itemPricePopular, "Total Purchase Value": totalPurchaseValuePopular},
columns=["Purchase Count", "Item Price", "Total Purchase Value"])

mostPopularItems = mostPopularItems.sort_values(["Purchase Count"], ascending=False)
mostPopularItems["Item Price"] = mostPopularItems["Item Price"].map("${:.2f}".format)
mostPopularItems["Total Purchase Value"] = mostPopularItems["Total Purchase Value"].map("${:.2f}".format)

mostPopularItems.head()
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>132</th>
      <th>Persuasion</th>
      <td>9</td>
      <td>$3.22</td>
      <td>$28.99</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
mostProfitableItems = pd.DataFrame({"Purchase Count": purchaseCountPopular, "Item Price":itemPricePopular, "Total Purchase Value": totalPurchaseValuePopular},
columns=["Purchase Count", "Item Price", "Total Purchase Value"])

mostProfitableItems = mostProfitableItems.sort_values(["Total Purchase Value"], ascending=False)

mostProfitableItems["Item Price"] = mostProfitableItems["Item Price"].map("${:.2f}".format)
mostProfitableItems["Total Purchase Value"] = mostProfitableItems["Total Purchase Value"].map("${:.2f}".format)

mostProfitableItems.head()

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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
