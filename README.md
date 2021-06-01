# ***The Influence of Taxes on State Migration***


## Abstract

**To what extent do state tax rates have an impact on inter-state migration?** For example, is there a relationship between a state having relatively high tax rates and people leaving the state?

In this project, we web scraped income & sales tax data from government websites in an attempt to build models predicting state migration patterns. In gerneral, our hypothesis proved true as places with the highest taxes like California and New York had the greatest outflow of people leaving those states, while places with relatively low taxes like Texas and Florida were receiving many new occupants in a given year. Although the states' tax data were significant predictors, they did not explain a large part of the variation of net migration (measured in the form of R-squared).


## **I.	Intro**

An event dubbed “[The Great California Exodus](https://www.wsj.com/video/series/journal-editorial-report/wsj-opinion-the-great-migration-out-of-california/AD5A0538-8173-450D-8893-93C5B02580DD)” piqued our interest because it describes the hundreds of thousands of Californians leaving the state each year. One of the reasons cited for leaving the state is high taxes. So which tax rates account for the majority of state revenue?

State income and sales tax rates account for most of the state’s revenue. A third category we considered was property tax, but property tax was mostly a source of revenue for local governments (ie: counties) rather than state governments. We are going to use historical income and sales tax data from the last 15 years to predict the state migration patterns of people entering and leaving states.


## **II. Data Collection and Cleaning**

### **Income Tax Brackets Data**

Income tax brackets were web scraped from https://www.tax-brackets.org/ using Python’s BeautifulSoup and Requests libraries. In total, we web scraped 765 website URL’s (51 * 15, we included the District of Columbia #DC should be a state). There was an issue with a few states and years. South Dakota data was missing from https://www.tax-brackets.org/. Fortunately it's a no income tax state and its data was easily entered. There were a few other state and year combinations the website was missing. We had to go find that state and year’s income tax data from their respective state government websites.

Cleaning the income tax data involved removing non-numeric characters to convert a few variables to numeric type (ex: “$14,500.00+” to 14500).

### **Sales Tax Data**

We were able to find a multi-tabbed excel file [here](https://www.taxpolicycenter.org/statistics/state-individual-income-tax-rates-2000-2020) that contained the sales tax rates for states by year. Additionally, it was missing the years of 2005 and 2009. In order to find those years, we had to look at government websites for [2005](https://govinfo.library.unt.edu/taxreformpanel/meetings/pdf/Salestax_04182005.pdf) and [2009](https://taxfoundation.org/updated-state-and-local-option-sales-tax/).

Year-to-year formatting was inconsistent. Several helper functions were created to aid in consolidating formatting across years. (Original data has inconsistent merged cells, lack of structure, and other details like superscripts in the excel file.)

### **Migration Data**

In order to get state-by-state migration data, we went to the United States Census Bureau [here](https://www.census.gov/data/tables/time-series/demo/geographic-mobility/state-to-state-migration.html). This site has access to state-by-state migration data between 2005-2019 contained in an excel file.

The data was formatted poorly with merged cells across columns and it repeated the indexing column every 11th column. The formatting was slightly different for each year, so we had to find the few common patterns that were similar across all years to make sure we could clean all excel files into a common format. Then we combined the data frames for each year into one main data frame with the years included as an extra column.


## **III.	Exploratory Analysis**

The following two tables show the states that do _not_ have sales or income tax, respectively. Alaska is the one state that has neither, which made it difficult to predict migration patterns for.

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image12.png?raw=true) ![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image5.png?raw=true)


This following facet graph shows in the 2000's, sales tax has bimodal distribution. Going into the 2010's it slowly transitions to trimodal distribution. The small 'bump' on the left of each facet represents the group of states with no sales tax.

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image10_1.jpg?raw=true) ![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image10_2.jpg?raw=true)



The following choropleth illustrates how the sales tax changes each state by year. It is interactive and in the “Data Exploration” google collab, one can filter by year. 

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image9.png?raw=true)

There are two interesting observations from this graph. (1) Sales taxes from 2009-2011 seem to be particularly low compared to the rest of the years. This could be a response by state governments to the Great Recession during those years. (2) Sales tax tends to not vary greatly each year. It's mostly constant across each state, with increases or decreases every couple years.



The following choropleth illustrates how the median Income Tax changes in each state by year. It is interactive and in the “Data Exploration” google collab, one can filter by year. 

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image2.png?raw=true)

The observations based on this graph is that Income Tax median rates vary a great deal more than the sales tax rates. 


The following choropleth illustrates Migration Inflow of each state by year. It is interactive and in the “Data Exploration” google collab, one can filter by year. 

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image11.png?raw=true)

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image3.png?raw=true)

If you look at any year, the three states by far with the most migration inflow are California, Texas, and Florida. The table confirms those are in the top five States regarding mean inflow migration per year, along with North Carolina and Georgia.


The following choropleth illustrates Migration Outflow of each state by year. It is interactive and in the “Data Exploration” google collab, one can filter by year. 

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image4.png?raw=true)

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image1.png?raw=true)


In terms of mean outflow migration per year, California, Florida, and Texas are still in the top 5. But the other states people are leaving are New York and Illinois.

Now hold on, if California, Texas, and Florida are in the top 3 for both inflow and outflow migration, what does that really mean? It means we need to take a look at net migration data, by state.


![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image8.png?raw=true)


Here we see the top five states, on average, with the most net migration: Texas, Florida, North Carolina, Arizona, and Georgia. These states all have net positive migration. Compare those states to the bottom five, which all have net negative migration. Those states are Michigan, New Jersey, Illinois, California, and New York.

There is a huge disparity between the top and bottom five. About 111,000 people moved to Texas, on average, over the last 15 years. Meanwhile about 164,000 people moved out of New York, on average, over the same time period.


## **IV. Predicting Migration Outflow and Inflow using Linear Regression**



Our variables for each model:
-	Minimum State Income Tax Rate: 
-	Maximum State Income Tax Rate
-	Median State Income Tax Rate
-	State Sales Tax


### Overview of our Models

Before we start, we want to specifically mention why we did not include dummy variables corresponding to each state in our model. This is because just knowing the state tells us the inflow and outflow by itself. In preliminary tests, we found that having only the state dummy variables gives us R-squared of .98, which is extremely high. This is because the state dummy encapsulates specific things about the state, its weather, its housing prices, and of course its tax rates. Meaning that however much the tax of a state influences an individual to move/leave there is accounted for by that one dummy variable. 

Knowing this we ran two main categories of regressions to predict the outflow and inflow of migration per state:
1.	Predicting inflow/outflow using all the state migration/tax data
2.	Predicting inflow/outflow one state at a time, meaning we trained one model per state using only that specific states migration and tax data for prediction


For the first model, we got some interesting results depending on whether or not we added an intercept to the model.

| Model         | Inflow          | Outflow     |
|:-------------:|:----------------:|:-----------|
R-squared (without intercept) | 0.602    |     0.609  
R-squared (with intercept)    | 0.119    |     0.116


This shows us the model has *far* more predictive power when there's no intercept given. However, we chose to keep the intercept going forward despite it weakening the model’s predictive power for interpretability purposes.

For the second model, we tested its accuracy by the average R-squared model across all state models (including an intercept). Here are some graphs of the predictive power of each model by state:

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image7.png?raw=true)

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image6.png?raw=true)

We found that states that don’t have any taxes, like Texas, had no ability to predict what the rates were which resulted in R-squared values of 0. This is due to us trying to predict the migration of these states, using taxes that don’t exist. Conversely, states with higher tax rates are easier to predict migrations using tax rates, for example, California has an R-squared of .99 when predicting how many people will leave, and an R-squared of .688 to predict how many people will come to the California each year. 


### Models Using All Migration and Tax Data


The first model we created was trying to predict the inflow of a generic state using all the state’s tax data across all years collected (2005-2019). The output is below:

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image13.PNG?raw=true)


As you can see, the R-squared on models with the intercept are not high; however, the P-values for all the coefficients except for the minimum income tax rate are all below .05, providing statistical evidence of their importance when predicting the inflow of a given state per year.


Interpretation of coefficients:

1.	Intercept: If a state has no taxes, we predict that there will be 100,000 people trying to enter the state
2.	Minimum Income Tax Rate: If a state increases its tax rate for the lowest tax bracket by 100%, then we expect 700,000 new people to enter the state)
3.	Maximum Income Tax Rate: If a state increases its tax rate by 100% for the highest tax bracket by 100%, then we predict 1,326,000 people to enter the state 
4.	Median Income Tax Rate: If the income tax rate increases by 100% for the median tax bracket, then we predict for 3,270,000 to leave the state. 
5.	Sales Tax: If the sales tax rate increases by 1% for the state, then we predict for 1,656,000 people to enter the state. 



The second model is the same as the first but attempting to predict the migration out of the state rather than into the state. The output is below:

![](https://github.com/DeclanMolony/Data301_Final_Project/blob/main/Images/image14.PNG?raw=true)

From the output above we can see that R-squared is very similar to that of the inflow model, but the main difference between the two is that all the P-values for all coefficients for this model are below .05, meaning they have statistical significance in this model.

Interpretation of coefficients:

1.	Intercept: If a state has no taxes, we predict that there will be 60,000 people to leave the state
2.	Minimum Income Tax Rate: If a state increases its tax rate for the lowest tax bracket by 100%, then we expect 1.487 million new people to leave the state
3.	Maximum Income Tax Rate: If a state increases its tax rate for the highest tax bracket by 100%, then we predict 2.9 million people to enter the state.
4.	Median Income Tax Rate: If the income tax rate increases by 100% for the median tax bracket, then we predict for 4.9 million people to enter the state. 
5.	Sales Tax: If the sales tax rate increases by 100% for the state, then we predict for 1.76 people to leave the state. 


### Conclusion about these models

The interpretation of the coefficient of these two models contradict each other. In the first model we are predicting inflow, and in the second we are predicting outflow. However, the signs on all the coefficients are the same for both models, meaning the interpretations of the two models directly contradict each other. For example, if the median tax rate increases by 100%, then the inflow model predicts that the state will LOSE over 3 million people, but in the exact same scenario, the outflow model predicts the state will GAIN nearly 5 million people. This shows a serious weakness in our model. If we were to try and improve it, we would look for more confounding variables that explain some of the movement between states; examples being weather, housing prices, and employment rates. Adding confounding variables would allow our model to further isolate what the main causes of state-by-state migration.


## **Conclusion**

"In this world nothing is certain but death and taxes" - Benjamin Franklin


No one likes paying taxes, that is tautology if ever there was one, but we wanted to find out to what extent people would go to not pay their taxes. Through our data exploration we found that states with the largest net migrations out of the state are California and New York, with the highest tax rates; furthermore, we found that Texas, a state with no income tax, has the highest net migration into the state. These simple statistics were evidence that tax rates play a big role in people choosing to move to or out of a given state. 


We had hoped that we could use linear regression to find further evidence of this phenomenon, but our model was simply too weak to make any meaningful conclusions on the matter. We believe that we would need to account for other reasons people might choose to move to, or, from a state: a state’s weather, housing prices, and employment rates being some examples. And if we were to add these to our model, we think we would find more definitive evidence of the role taxes play in people’s decision to move between states. 











