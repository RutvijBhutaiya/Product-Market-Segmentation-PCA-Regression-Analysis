In Progress...

## Product Market Segmentation: Customer Satisfaction  
## PCA (Principal Component Analysis) & Regression Analysis

<br>

<p align="center"><img width=59% src=https://user-images.githubusercontent.com/44467789/65239279-3f4aef00-dafc-11e9-95a8-d3784f2e85a7.png> 

<br>

### Table of Content

- [Objective](#objective)
- [Setting Up R Studio and Data Variables](#setting-up-r-studio-and-data-variables)
- [Descriptive Statistics](#descriptive-statistics)
- [Implement Principal Component Analysis and Factor Analysis](#implement-principal-component-analysis-and-factor-analysis)
- [Setting up Data Variables for Regression Analysis](#setting-up-data-variables-for-regression-analysis)
- [Regression Model](#regression-model)

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

<p align="center"><img width=72% src=https://user-images.githubusercontent.com/44467789/65833772-6b7f2080-e2f1-11e9-83c0-74d55c1e6e51.jpg>

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


<p align="center"><img width=90% src=https://user-images.githubusercontent.com/44467789/65833790-b00abc00-e2f1-11e9-8400-74d480798eae.jpg>
	
As shown in biplot for Principal Component Analysis, we can conclude that RC1 combines, CompRes, OrdBilling, and DelSpeed variables with higher correlation matrix of 0.9258, 0.8638 and 0.9382 respectively. 

RC2 combines, Ecom, Advertising, and SalesFImage with higher correlation matrix of 0.8706, 0.7415 and 0.9005 respectively.

RC3 combines, TechSup and WartyClaim with higher correlation matrix of 0.9392 and 0.9310 respectively.

RC4 combines, ComPricing, ProdQual and ProdLine with higher correlation of (-0.7226), 0.8757 and 0.6420 respectively. 

And hence based on subjective characteristic all the variables will collapse to 4 factors. These 4 factor names are, Sales Service Desk represents RC1, Brand Marketing Desk represents RC2, BackEnd Support Desk represents RC3 and Strategic Research Desk represents RC4. 

To elaborate more on factors, Sales Service Desk represents Complaint Resolution, Order & Billing and Delivery Speed variables. 

Brand Marketing Desk represents E-Commerce, Advertising, and Salseforce Image variables.

BackEnd Support Desk represents Technical Support and Warranty & Claims variables.

And Strategic Research Desk represents Competitive Pricing, Product Quality, and Product Line variables. 

To perform regression analysis we will consider these factors as independent variables and we will name the factors for simplicity, Sales Service Desk as ServDesk, Brand Marketing Desk as MktDesk, BackEnd Support Desk and SuppDesk and Strategic Research Desk as RechDesk. 

Based on ServDesk, MktDesk, SuppDesk and RechDesk variables we will perform regression analysis and predict how Customer Satisfaction as depended variable changes based on these 4 independent variables. 

Before we begin our regression analysis, we will create new .csv file based on factor scores. We have name this .csv file as newhair.csv. 

```
#Write newhair.csv file for Regression Analysis

      > rotate$scores

      > newhair = as.data.frame(rotate$scores)
      > write.csv(newhair, "newhair.csv")
```

<br>

### Setting up Data Variables for Regression Analysis

To perform regression analysis, we will load newhair.csv file and renamed the variables according to factor names.  We have also loaded original hair.csv file to observe Satisfaction variable. 

As we see in the following R studio code, we have used cbind.data.frame() function to present 18 variables and 100 observations from both the .csv files - [hair.csv](https://github.com/RutvijBhutaiya/Product-Market-Segmentation-PCA-Regression-Analysis/blob/master/hair.csv) & [newhair.csv](https://github.com/RutvijBhutaiya/Product-Market-Segmentation-PCA-Regression-Analysis/blob/master/newhair.csv)

```
> setwd("C:/Users/server/Desktop/New R")

	> newhair = read.csv('newhair.csv')
	> hair = read.csv('hair.csv')

     	> names(newhair) = c("x","ServDesk","MktDesk","SuppDesk","RechDesk")
	> newhair = cbind.data.frame(newhair,hair)
```

However, we require only independent variables ServDesk, MktDesk, SuppDesk, and RechDesk along with dependent variable Satisfaction. 
```

	> newhair = newhair[, -6:-17]
	> newhair = newhair [, -1]
```

<br>

### Regression Model

Before we begin regression analysis, we have plot scattered graph individually for independent and dependent variables. 

This graph is based on Ordinary Least Squared Method (OLS). With the scatter graphs we can analyze visual patterns or relationship between independent and dependent variables.

```
> library(ggplot2)
> par(mfrow=c(2,2))

> plot(ServDesk, Satisfaction, main = 'Sales Servive Desk', col = 'cornflowerblue', abline(lm(Satisfaction~ServDesk)) )

> plot(MktDesk, Satisfaction, main = 'Brand Marketing Desk', col = 'orange', abline(lm(Satisfaction~MktDesk)) )

> plot(SuppDesk, Satisfaction, main = 'BackEnd Support Desk', col = 'firebrick2', abline(lm(Satisfaction~SuppDesk)) )

> plot(RechDesk, Satisfaction, main = 'Strategic Research Desk', col = 'darkseagreen', abline(lm(Satisfaction~RechDesk)) )
```
<p align="center"><img width=72% src=https://user-images.githubusercontent.com/44467789/67869571-a0320200-fb53-11e9-962c-70bc2773a399.gif>
	
As we see in the scattered graph ServDesk, MktDesk and RechDesk has linear relationship with dependent variable Customer Satisfaction.  However, SuppDesk shows no correlation with dependent variable. Bases on the scattered graph we can also present the data tightness of fit to the line is normal for all the four variables. 

Normal tightness of fit also indicates that there would be low to normal R-square value in the regression model. Based on the SuppDesk scattered plot we can assume SuppDesk’s p-value should be more than 0.05, which indicates low significant to the model. 

Now, based on our date set for newhair customer satisfaction study, let us analyze and built multi linear regression model. 

Y hat = B0 + B1*x1 + B2*x2 + B3*x3 + B4*x4

Where,
Y hat is dependent variable [Satisfaction]
B0 is intercept or constant value
B1 is slop for correlation to x1 variable [ServDesk]
B2 is slop for correlation to x2 variable [MktDesk]
B3 is slop for correlation to x3 variable [SuppDesk]
B4 is slop for correlation to x4 variable [RechDesk]

 As we study statistical summary model for multi linear regression, 
B0 = 6.9180
B1 = 0.6180
B2 = 0.5097
B3 = 0.0671
B4 = 0.5403

```
> Model1 = lm(Satisfaction~ServDesk+MktDesk+SuppDesk+RechDesk)

> summary(Model1)
```
 <p align="center"><img width=72% src=https://user-images.githubusercontent.com/44467789/67869791-edae6f00-fb53-11e9-9097-a511f7940a8b.gif>
	
And hence Regression model for newhair data set is 

Satisfaction = 6.9180 + 0.6180*ServDesk + 0.5097*MktDesk + 0.067*SuppDesk + 0.5403*RechDesk

Based on the model we can interpret that ServDesk has highest significant impact on customer satisfaction. After that MktDesk and RechDesk has good significant impact on customer satisfaction score. However, as we see, SuppDesk is lagging behind to add its significance in customer satisfaction. 

Here, B0 value is constant, which indicates that 6.9180 is by default customer satisfaction due respect to independent variables. 

Also, 0.6180*ServDesk indicates, one unit increase in ServDesk variable leads to customer satisfaction increase of 0.618. Similarly, one unit increase in MktDesk and RechDesk leads to increase in customer satisfaction by 0.509 and 0.540 respectively. 

In the multi regression model, all the independent variables are positively correlated to dependent variables. As we see, SuppDesk does not significantly impact customer satisfaction, but still increase in one unit of SuppDesk leads to increase satisfaction by 0.067. 


Now, as we move further in our study for multi linear regression for newhair data set, we came across R-square in our model. 

R-square plays important role in the regression model. R-square value represents data fit to the model. 

Based on regression model summary, R-square is 0.6605 and adjusted R-square is 0.6462. R-square is always between 0 to 1, most statistics suggest – normal R-square range between 0.70 to 0.90 (may vary according to model to model).

In our study R-square is 0.6605, which is normal, not very high and not very low. Extreme low R-square value indicates that model is under fitted and has noisy variables. On the other side extreme high R-square value indicated that model is over fitted. However, higher the R-square the better the model (again depends on model to model). 


As we analyzed scatter graphs for individual variables, based on relationship pattern we can clearly visualize normal R-square value. 

Now, based on R-square value and scatter graphs analysis, how to confirm whether model fits best for regression analysis or not!

For this we have performed Analysis of Variance or ANOVA on newhair data set,

<p align="center"><img width=72% src=https://user-images.githubusercontent.com/44467789/67869936-29493900-fb54-11e9-92c1-233a322e9afa.png>
	
Based on ANOVA summary results, we can interpret that p-value is less than 0.05 alpha level. P-value from the regression model summary is 2.2e-16, which means we are almost 100% right. And hence, we reject the Null Hypotheses and conclude that regression model is valid and regression model exists in population. 

This means customer satisfaction is linearly dependent on Sales Service Desk, Brand Marketing Desk, BackEnd Support Desk, and Strategic and Research Desk.  

However, individual p-value analysis on independent variables presents us that ServDesk, MktDesk and RechDesk are highly significant with p-value less than alpha 0.05 levels. And hence, has significant impact on dependent variable satisfaction.  But, SuppDesk is not significant and p-value is greater than 0.05 alpha levels. 

Do we need to omit SuppDesk from our regression model?

Let us study the new regression model without SuppDesk variable. 

```
> M2 = lm(Satisfaction~ServDesk+MktDesk+RechDesk)

> summary(M2)

```
Statistically, based on p-value SuppDesk in not significant, but, in the new model summary, omitting SuppDesk does not make value addition in the original model quality.  And, without SuppDesk we are losing quality of our model. Because, R-square value [0.657] in new model is less than the R-square value [0.660] from the original model. In numerical terms omitting SuppDesk does not significantly improves the model quality. Hence we decide to include SuppDesk variable in the regression model. 

Before we move further, there are few notes to revise, Why SuppDesk does not play significant role in customers’ satisfaction?

For this, we went back in our study at Principal Component Analysis table, where we see that SuppDesk represents Technical Support and Warranty & Claims variables from the original data set. 

Hence, vaguely we can conclude that generally in consumer goods product, people do not make purchases or does not create satisfaction index in their minds based on technical support or warranty & claims. However, based on product categories like investment products or software tools or any service related product could have more significance to technical support and warranty & claims. 

Adding to this we cannot deny that in our study SuppDesk is 100% irreverent & not significant. Because we still have weak positive correlation of 0.0671 with dependent variable.

In this Multi Linear Regression model for customer satisfaction analysis, we built regression model, we tested regression model, and now we will see the prediction accuracy on the regression model. 

It is better to analyze actual customer satisfaction and predicted customer satisfaction in visuals. 

As we see in the graph, red mark line represents actual customer satisfaction and blue mark line represents predicted customer satisfaction. 

```
> Actual = newhair$Satisfaction

> Predict = predict(Model1)

> BackTrack = data.frame(Actual, Predict)
> View(BackTrack)

> plot(Actual, col = 'red', main = 'Customer Satisfaction - Actual vs. Predicted', ylab = 'Customer Satisfaction points', xlab = 'Observations')

> plot(Predict, col = 'blue', main = 'Customer Satisfaction - Actual vs. Predicted', ylab = 'Customer Satisfaction points', xlab = 'Observations')

> lines(Predict, col = 'blue')

> lines(Actual, col = 'red')
```
<p align="center"><img width=72% src=https://user-images.githubusercontent.com/44467789/67870250-8b09a300-fb54-11e9-9b0e-440f9c873fc0.gif>

Red Line = Actual Customer Satisfaction 
Blue Line = Predicted Customer Satisfaction 

Now, based on the plot we can see the pattern between Actual and Predicted lines, which are much similar.  

Further, we can also calculate confidence interval at 95% of the slop for customer satisfaction. 

```
> confint(Model1, "ServDesk")
             2.5 %    97.5 %
ServDesk 0.4766059 0.7594879

> confint(Model1, "MktDesk")
            2.5 %    97.5 %
MktDesk 0.3682931 0.6511751

> confint(Model1, "SuppDesk")
               2.5 %    97.5 %
SuppDesk -0.07430515 0.2085769

> confint(Model1, "RechDesk")
            2.5 %    97.5 %
RechDesk 0.398878 0.6817601
```
ServDesk variable range presents population slop of satisfaction variable will remain either lower end at 0.4766 or upper end at 0.7594 with 95% confidence level. 

Similarly, goes with MktDesk and RechDesk variables with 95% confidence level. 

Interestingly, with SuppDesk the lower end is (-0.0743), which means in worst case customer rating for Technical Support and Warranty & Claims could impact regression model adversely. However, due to very low multiplier impact could be very small and can be ignored.  

<br>

### Conclusion 

Based on the consumer goods product – Hair – market segmentation data set, we can conclude that, due to multicollinearity within independent variables, we cannot apply regression model directly on the date set. 

So, we created new data set – New hair – based on Principal Component Analysis. We have also recommended subjective new variable names as ServDesk, MktDesk, SuppDesk and RechDesk to the components. And then, based on Factor Analysis study we performed multi linear regressing.   

Based on the regression model we have concluded that Sales Service Desk plays – the most significant role in customer satisfaction. That means company should be extra cautious in Complain Resolution, Order & Billing, and Delivery Speed fronts. If Delivery is late or complaint is not resolved in time may leads to decline in company’s revenue. However, Brand Marketing Desk and Strategic Research Desk also plays important role with 0.509 and 0.540 weighted respectively in the regression model. 

From the study, we have also concluded that due to consumer goods product type customer do not give significance to Technical Support and Warranty & Claims, And hence SuppDesk variable does not play significance role in customer satisfaction index.

In overall study, we removed multicollinearity from the data, we built regression model, we tested regression model and based on BackTrack data we also predicted Actual vs. Predicted customer satisfaction score in line chart. 

In product or service based companies, if customer/prospect is satisfied with product, he will make purchase again and again for that particular product, and that works as revenue multiplier for the company. High customer satisfaction can also leads to cross selling of products. 

Hence, we suggest management to conduct customer survey on regular bases to identify trends and relationship for higher customer satisfaction experience. 



<br>



### Acknowledge

Great Lakes Institute - BACP Project
