In Progress...

## Product Market Segmentation:  PCA & Regression Analysis

<br>

<p align="center"><img width=59% src=https://user-images.githubusercontent.com/44467789/65239279-3f4aef00-dafc-11e9-95a8-d3784f2e85a7.png> 

<br>

### Table of Content

- [Objective](#objective)
- [Setting Up R Studio and Data Variables](#setting-up-r-studio-and-data-variables)
- [Descriptive Statistics](#descriptive-statistics)
-

<br>

### Objective

The objective of the project is to perform Factor Analysis to reduce variables in context of market segmentation for hair product service and implement regression model to predict customer satisfaction based on independent variables.

<br>

### Setting Up R Studio and Data Variables

Out of 13 variables, “Satisfaction” is dependent variable and following 11 are independent variables. 

```
> dim(hair)
	[1] 100  13
  

> names(hair)
 [1] "ID"           "ProdQual"     "Ecom"         "TechSup"      "CompRes"      "Advertising"  "ProdLine"    
 [8] "SalesFImage"  "ComPricing"   "WartyClaim"   "OrdBilling"   "DelSpeed"     "Satisfaction"
```

Variable expansion is as follow,

1.	ProdQual =  Product Quality
2.	Ecom = E-Commerce
3.	TechSup = Technical Support
4.	CompRes = Complaint Resolution
5.	Advertising = Advertising
6.	ProdLine = Product Line
7.	SalesFImage = Salseforce Image
8.	ComPricing = Competitive Pricing
9.	WartyClaim = Warranty & Claims 
10.	OrdBilling = Order Billing
11.	DelSpeed = Delivery Speed
12.	Satisfaction = Customer Satisfaction 

In hari.csv file, categorical data represents customers’ votes ranging between 0 and 10. In the date ‘0’ represents least score and ‘10’ represents highest score. In data set, Satisfaction variable is dependent on ProdQual, Ecom, TechSup, CompRes, Advertising, ProdLine, SalesFImage, ComPricing, WartyClaim, OrdBilling and DelSpeed. 


### Descriptive Statistics

As we discussed 13 variables, Satisfaction variable is dependent on 11 independent variables, let’s explore descriptive statistics. 

As we see variable data type in hair.csv file, dependent and independent variables are in numerical format. 

```
> str(hair)
'data.frame':	100 obs. of  13 variables:
 $ ID          : int  1 2 3 4 5 6 7 8 9 10 ...
 $ ProdQual    : num  8.5 8.2 9.2 6.4 9 6.5 6.9 6.2 5.8 6.4 ...
 $ Ecom        : num  3.9 2.7 3.4 3.3 3.4 2.8 3.7 3.3 3.6 4.5 ...
 $ TechSup     : num  2.5 5.1 5.6 7 5.2 3.1 5 3.9 5.1 5.1 ...
 $ CompRes     : num  5.9 7.2 5.6 3.7 4.6 4.1 2.6 4.8 6.7 6.1 ...
 $ Advertising : num  4.8 3.4 5.4 4.7 2.2 4 2.1 4.6 3.7 4.7 ...
 $ ProdLine    : num  4.9 7.9 7.4 4.7 6 4.3 2.3 3.6 5.9 5.7 ...
 $ SalesFImage : num  6 3.1 5.8 4.5 4.5 3.7 5.4 5.1 5.8 5.7 ...
 $ ComPricing  : num  6.8 5.3 4.5 8.8 6.8 8.5 8.9 6.9 9.3 8.4 ...
 $ WartyClaim  : num  4.7 5.5 6.2 7 6.1 5.1 4.8 5.4 5.9 5.4 ...
 $ OrdBilling  : num  5 3.9 5.4 4.3 4.5 3.6 2.1 4.3 4.4 4.1 ...
 $ DelSpeed    : num  3.7 4.9 4.5 3 3.5 3.3 2 3.7 4.6 4.4 ...
 $ Satisfaction: num  8.2 5.7 8.9 4.8 7.1 4.7 5.7 6.3 7 5.5 ...
```

Also, hair.csv file does not carry any missing value. Not having missing value in date set actually improves data quality and results for regression analysis. To check missing value we applied is.na() in R Studio. 

```
> any(is.na(hair))
 [1] FALSE


# Summary for data variables

	> summary(hair)
```
Now, before we start analyzing hair.csv date file, we need to check for the normalization data by scaling the variables. This exercise will give equal weighted to independent variable data and variable data will scale between 0 and 1. 

As shown in Annexure 1, variables’ w-statistics is more than 95% [except ProdQual variable]. For data normality, higher the w-statistics means normal data.  However, we also observed that p-value for ProdQual, Ecom, SalesFImage, ComPricing and OrdBilling are less than 0.05. That concludes deviation in normality for selected variables. 

<br>

### Implement Principal Component Analysis and Factor Analysis

Now, before we apply factor analysis and principal component analysis, we present the correlation between variables. In this case, our objective should be to find high correlation between independent variables.  However, in data set it could also be possible that independent variables are correlated with each other. In statistics, correlations within independent variables are called multicollinearity. 

As we see in scattered plot, due to 11 variables we are not able to get clear visibility. In the scattered plot formation we have dropped ID and Satisfaction variable.

To perform PCA and Factor Analysis we consider only independent variables and check whether there are any correlations within them or not. Hence we are not considering ID and Satisfaction variables for implementing PCA and Factor Analysis study. 

```

#Scattered Plot from hair.csv date
> pairs(hair[,2:12])
```
<p align="center"><img width=80% src=https://user-images.githubusercontent.com/44467789/65815708-b5dca080-e210-11e9-9ee2-c5152cf9c6d5.jpg>

Due to low visibility of scattered plot from hair.csv date we are not able to represent clear picture of correlations within variables. Hence, we have created correlation table for independent variables.  

For better visualization we have presented correlation matrix with heat maps. 

```
> library(ggcorrplot)
> ggcorrplot(cor(hair), type = 'lower')
```
<p align="center"><img width=72% src=https://user-images.githubusercontent.com/44467789/65815718-e7ee0280-e210-11e9-82c3-967c3d52e32a.jpg>

As we see in following correlation table, correlation between SalesFImage and Ecom is 0.7915, WartyClaim and TechSup is 0.7971, CompRes and DelSpeed is 0.865, DelSpeed and ProdLine is 0.602, OrdBilling and CompRes is 0.76, OrdBilling and DelSpeed is 0.75,  We can also see negative correlation between ProdLine and ComPricing is (- 0.495), ProdQual and ComPricing is (- 0.401) etc.  

These correlations within independent variables are multicollinearity. And, before performing regression analysis, multicollinearity should be removed by factor analysis and PCA. So, all correlated independent variables could be transferred in to non-correlated variables and then we would perform regression analysis. 

```
> print(cor(hair), digits = 2)
```
<p align="center"><img width=79% src=https://user-images.githubusercontent.com/44467789/65815837-fab50700-e211-11e9-99be-e2a754571a06.jpg>


<br>

### Acknowledge

Great Lakes Institute - BACP Project
