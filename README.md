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

To collect more proof on homogeneity of variables, Bartlett k-squared test done on data set. It shows that p-value is less than 0.05 and hence Null Hypotheses rejected. This indicates reduction in independent data variables should be done before performing regression analysis.

```
> bartlett.test(hair)

	Bartlett test of homogeneity of variances

data:  hair
Bartlett's K-squared = 146.4, df = 10, p-value < 2.2e-16
```

Now, before reducing variable we will perform PCA to identity right number of factors based on Scree plot and Eigen values. For PCA we installed ‘nFactors’ package to perform factor analysis. 

As shown in following table Eigen values are in decreasing trend and Eigen vectors are principle components. Eigen value represents decomposition matrix, in other words, largest variance summarized or reduction. 

Kaiser rule for PCA interprets factors based on Eigen values. Normally, Kaiser Rule takes value greater than 1, as factors.  Hence, in the following table there are 4 considerable factors, which are greater than 1. 

```
> library(nFactors)

> haireigen = eigen(cor(hair))

> haireigen

```
<p align="center"><img width=79% src=https://user-images.githubusercontent.com/44467789/65815807-b6296b80-e211-11e9-82f5-16e1c63452bf.jpg>
	
```	
> eigenvalue = haireigen$values
> factor = c(1:11)

> Scree = data.frame(factor,eigenvalue)

> plot(Scree, main = 'Scree Plot', xlab = 'Factors', ylab = 'Eigen Values', col = 'Blue')

      >lines(Scree, col = 'maroon')
```

<p align="center"><img width=69% src=https://user-images.githubusercontent.com/44467789/65815891-921a5a00-e212-11e9-90ca-3f7d513b232a.jpg>

Now, after identifying number of factors based on Eigen values and Scree plot, we need to fill scores for these 4 components and should be given subjective names to the factors to perform regression analysis. Now, these 4 factors will become independent variables for dependent variable – Customer Satisfaction in our regression model. 

Factor analysis is hallmark for minimizing variables with maximum information. With the help of library psych we have studied Rotational approach to solve the multicollinearity problem. 

For Orthogonal rotation we are using varimax rotate. Varimax rotation is rotation in which we assume that there is no correlation between components [RC1 to RC4].

```
> library(psych)

> rotate = principal(hair, nfactors = 4, rotate = 'varimax')
> print(rotate, digits = 4)
```

<p align="center"><img width=69% src=https://user-images.githubusercontent.com/44467789/65833772-6b7f2080-e2f1-11e9-83c0-74d55c1e6e51.jpg>

As shown in Principal Component Analysis table, there are 4 components RC1, RC2 RC3, and RC4. In PCA table, ss loading values indicates Eigen values. However, eigen values in PCA table varies from ‘haireigen’, because here we are doing orthogonal rotation with varimax rotation. If we do un-rotation then ss loading will be exactly same as ‘haireigen’. To get a better picture on what factors means we prefer to study orthogonal rotation. 

Before we start analyzing PCA table, we should know what RC1, RC2, RC3 and RC4 loading means to variables. In factor analysis our objective is to collapse variables, and in cluster analysis we collapse observations. As we see in PCA table, RC1 for ProdQual is 0.0015, which means correlation for RC1 and Product Quality variables is 0.15%. 

Similarly, RC2 and Product Quality are (-1.27%) negatively correlative. From the same table we have observed that Product Quality has strong positive correlation of 0.8757 with RC4 component. 

Similarly, loadings from PCA table represent correlation between individual variable with respective components. 

Now, as we do further analysis on PCA table, we came across proportion variance and cumulative variance. 
Here, proportion variance is derived based on Eigen value, % Proportion var = Eigen value / number of variables.

So, RC1 proportion var is 0.2630, where eigen value (ss loading) is 2.8927 and number of variables are 11. Similarly for RC2, RC3 and RC4, proportion var are 0.2031, 0.1687 and 0.1612 respectively. 

Next term, cumulative var represents percentage of information cumulatively distributed across components, where, RC1 and RC2 together carry 46.6% information of the data. And RC1, RC2, RC3 and RC4 all together carry 79.5% of information from the original data set. In other words, with components RC1, RC2, RC3 and RC4 together we are able to explain 79.5% of information from the original data set. 

As we further analyze PCA table, we also see h2 column. Column h2 represents communality. Communality means common variance captured by RC1, RC2, RC3, and RC4 components. 

For example, communality for Ecom variable is 0.7771. Ecom communality derived from RC1, RC2, RC3 and RC4 loadings, of 0.0568, 0.8706, 0.0475, and (-0.1175) respectively. Ecom communality is sum of squared of all the 4 components loadings. 

Now, as we closely analyze communality column we can see that DelSpeed variable’s communality is highest, 0.9144 among all the study variables. That means DelSpeed carry 91.4% information across all the 4-RC components.

Similarly, ProdQual and Ecom variables communality represents 76.8% and 77.7% information across all the components, and similarly explained for all the variables. 

As, we study further, we can assume that, we should have derived factor names based on Un-rotational PCA study also, but, due to direct loading we were not able to present the clear message for factors. And hence, for powerful analysis and higher confidence we performed orthogonal rotation. 


<p align="center"><img width=69% src=https://user-images.githubusercontent.com/44467789/65833790-b00abc00-e2f1-11e9-8400-74d480798eae.jpg>
	



<br>

### Acknowledge

Great Lakes Institute - BACP Project
