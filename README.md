# ***The Influence of Taxes on State Migration***


## **I.	Intro**

**To what extent do state tax rates have an impact on inter-state migration**? For example, is there a relationship between a state having relatively high tax rates and people leaving the state?

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

Before we start, we want to specifically mention why we did not include dummy variables corresponding to each state in our model. This is because just knowing the state tells us the inflow and outflow by itself. In preliminary tests, we found that having only the state dummy variables gives us R^2 of .98, which is extremely high. This is because the state dummy encapsulates specific things about the state, its weather, its housing prices, and of course its tax rates. Meaning that however much the tax of a state influences an individual to move/leave there is accounted for by that one dummy variable. 

Knowing this we ran two main categories of regressions to predict the outflow and inflow of migration per state:
1.	Predicting inflow/outflow using all the state migration/tax data
2.	Predicting inflow/outflow one state at a time, meaning we trained one model per state using only that specific states migration and tax data for prediction


For the first model, we got some interesting results depending on whether or not we added an intercept to the model.

| Model         | Inflow          | Outflow     |
|:-------------:|:----------------:|:-----------|
R^2 (without intercept) | 0.602    |     0.609  
R^2 (with intercept)    | 0.119    |     0.116


This shows us the model has *far* more predictive power when there's no intercept given. However, we chose to keep the intercept going forward despite it weakening the model’s predictive power for interpretability purposes.

For the second model, we tested its accuracy by the average R^2 model across all state models (including an intercept). Here are some graphs of the predictive power of each model by state:















