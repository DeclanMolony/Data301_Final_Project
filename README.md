# ***The Influence of Taxes on State Migration***


## **I.	Intro**

  US states govern themselves on a number of issues such as tax rates. We wanted to know, _to what extent do state tax rates have an impact on inter-state migration_? For example, is there a relationship between a state having relatively high tax rates and people leaving the state?

An event dubbed “[The Great California Exodus](https://www.wsj.com/video/series/journal-editorial-report/wsj-opinion-the-great-migration-out-of-california/AD5A0538-8173-450D-8893-93C5B02580DD)” piqued our interest because it describes the hundreds of thousands of Californians leaving the state each year. One of the reasons cited for leaving the state is high taxes. So which tax rates account for the majority of state revenue?

By far, state income and sales tax rates account for most of the state’s revenue. A third category we considered was property tax, but property tax was mostly a source of revenue for local governments (ie: counties) rather than state governments. We are going to use historical income and sales tax data from the last 15 years to predict the state migration patterns going into and out of states.


## **II. II.	Data Collection and Cleaning**

### **Income Tax Brackets Data**

Income tax brackets were web scraped from https://www.tax-brackets.org/ using Python’s Requests and BeautifulSoup libraries. In total, we web scraped 765 website url’s (51 * 15, we included the District of Columbia #DC should be a state). There was an issue with a few states and years. All of South Dakota was missing from https://www.tax-brackets.org/, but fortunately it is a no income tax state, and the data was easily entered. There were five other state and year combinations (e.g. California 2009) that the website was missing. We had to go find that state and year’s income tax data from their respective state government websites.

Cleaning the income tax data was relatively simple. It involved removing non-numeric characters to convert a few variables to a numeric type (ex: “$14,500.00+” to 14500).






