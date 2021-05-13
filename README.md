# ***The Influence of Taxes on State Migration***


## **I.	Intro**

 US states govern themselves on a number of issues such as tax rates. We wanted to know, _to what extent do state tax rates have an impact on inter-state migration_? For example, is there a relationship between a state having relatively high tax rates and people leaving the state?

An event dubbed “[The Great California Exodus](https://www.wsj.com/video/series/journal-editorial-report/wsj-opinion-the-great-migration-out-of-california/AD5A0538-8173-450D-8893-93C5B02580DD)” piqued our interest because it describes the hundreds of thousands of Californians leaving the state each year. One of the reasons cited for leaving the state is high taxes. So which tax rates account for the majority of state revenue?

By far, state income and sales tax rates account for most of the state’s revenue. A third category we considered was property tax, but property tax was mostly a source of revenue for local governments (ie: counties) rather than state governments. We are going to use historical income and sales tax data from the last 15 years to predict the state migration patterns going into and out of states.


## **II. Data Collection and Cleaning**

### **Income Tax Brackets Data**

Income tax brackets were web scraped from https://www.tax-brackets.org/ using Python’s BeautifulSoup and Requests libraries. In total, we web scraped 765 website url’s (51 * 15, we included the District of Columbia #DC should be a state). There was an issue with a few states and years. South Dakota data was missing from https://www.tax-brackets.org/. Fortunately it's a no income tax state and its data was easily entered. There were a few other state and year combinations the website was missing. We had to go find that state and year’s income tax data from their respective state government websites.

Cleaning the income tax data involved removing non-numeric characters to convert a few variables to numeric type (ex: “$14,500.00+” to 14500).

### **Sales Tax Data**

We were able to find a multi-tabbed excel file [here](https://www.taxpolicycenter.org/statistics/state-individual-income-tax-rates-2000-2020) that contained the sales tax rates for states by year. Additionally, it was missing the years of 2005 and 2009. In order to find those years, we had to look at government websites for [2005](https://govinfo.library.unt.edu/taxreformpanel/meetings/pdf/Salestax_04182005.pdf) and [2009](https://taxfoundation.org/updated-state-and-local-option-sales-tax/).








