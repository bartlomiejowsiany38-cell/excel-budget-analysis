# Budget Analysis in Excel

Links:

[Excel Spreadsheet](https://excel.cloud.microsoft/open/onedrive/?docId=28A42F26836D515B%21s3742a70ec826425da86af30f7682ed96&driveId=28A42F26836D515B )
[Tableau Dashboard](https://public.tableau.com/app/profile/bartek.owsiany/viz/p_17810222593120/BudgetAnalysis )


Main Question: 
Given 6 months of personal transactions, which spending categories exceed budget, and where is there room to save?

## 1. Cleaning data

### 1.1 Fixing inconsistent categories

First, we can normalize the categories.After a quick look, we can notice that there are positions like ’Dining Out’ and ‘dining out’, or ‘Food’ and ‘FOOD’. For this I will use Ctrl + H to change the mistakes. In order to quickly find a list of positions that need to be normalized we can also use Python:

```python
category = pd.Series(data['Category'].unique())
category.sort_values()
```

3          Dining Out
1       Entertainment
0                Food
4    Health & Fitness
5            Shopping
2           Transport
6           Utilities

Here we clearly see all the mistakes that need to be found and normalized. Thanks to this we don’t need to search for every variance of ‘Food’ – we just know that there are two ‘FOOD’ and ’food’, and we also know that ‘Health & Fitness’ category has no issues, so we don’t have to change anything there. Applying the same procedure to Payment Method column also shows that there are no problems there:

0        Card
1        Cash
2    Transfer

Now, we can move on to handling duplicates.

### 1.2 Duplicates

Here we can just use Remove Duplicates option. Before deleting it also shows the amount of duplicates it found. Alternatively we could use data.duplicated().sum() and  data.drop_duplicates()
in Python. The results show 2 duplicate rows, which is not a lot, so we can go ahead and delete them.

### 1.3 Missing values

Here we can just select the row and then apply filter with only ‘Blanks’ box checked. This shows us one row with blank description. The row lacks description, but since it contains the category its still useful information in the analysis. So, instead of deleting the row I can just fill it. I used following python code:  

```python
data[(data['Amount'] >= 40) & (data['Amount'] <= 80) & (data['Category']=='Food')]
```


to check what other descriptions appear in similar amount range and food category. All descriptions in the outcome say ‘Lidl groceries’, so I will fill the blank with that.

## 2. Analysis

### 2.1 Spending in each category

Now we can check total spending in each category by inserting pivot table in the new sheet. We just need to put Category in rows and Amount in values and set them to sum. We can see here that over 6 months period the most is being spend in food category – 4438.3; and least on entertainment – 897.94. This makes food spending 29.17% of total spending and entertainment only 5.90%.

### 2.2 Spending in each month

Here we can also look closer at monthly spending by adding grouping by Month and adding sum of Amount in new pivot table. Monthly spending does not surpass 3000 and we can see that the actual average spending is 2536.13. We can also see that the most spending was in march – 2831.67, and least in april 2287.66.

### 2.3 Budget limits analysis

Now we can finally check the overspending in each category based on monthly budget table. We can see now that there is overspending in 4 categories:

Food -538.3
Dining Out -428
Transport -217
Shopping -290.96

and underspending in 3:

Entertainment +2.06
Health & Fitness +583.3
Utilities +1017.12

## 3. Conclusion

The most overspending appears in the Food category being 538.3 above the budget and there is huge underspending of 1017.12 below the budget. Even though there are more categories where overspending occurs, overall spending in 6 months time period is 83.22 below the budget, leaving rather small room for saving. It is most likely due to overestimation of spending amount in Utilities and Health & Fitness category, which can be redistributed to other areas.
