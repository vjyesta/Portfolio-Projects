**Inventory Control Management with SQL: Analyzing Walmart Sales Data**

![](media/image1.png){width="6.5in" height="6.5in"}

Recently, I took on a project to explore the role of **inventory control
management** in retail, a crucial aspect of ensuring smooth operations
and profitability for any product-based business.

Think about it: running out of a popular product during a peak season or
overstocking items that aren't selling well can significantly impact a
business's bottom line. That's where inventory control comes in, helping
businesses make smarter decisions about stocking, ordering, and sales
strategies.

For this project, I chose the Walmart sales dataset, which offered a
treasure trove of information: sales data spanning from 2010 to 2012,
macroeconomic indicators like CPI, weather conditions, and even holiday
data. With SQL as my tool of choice, I aimed to answer intriguing
questions like:

-   Which year had the highest sales?

-   Did weather influence sales patterns?

-   Do sales always spike during holidays?

-   Identify top-performing stores and products

-   Analyse for underperforming stores or products.

What started as a simple exercise quickly turned into a journey of
discovery, uncovering patterns and insights that could help any retailer
optimize their inventory planning. Let me walk you through how I
approached this project and the fascinating insights I found along the
way.

**Dataset Overview**

The dataset I used for this project is a historical record of Walmart
sales, spanning from **February 5, 2010, to November 1, 2012**,
contained in the file **Walmart\_Store\_sales**. This dataset is compact
but rich in insights, making it a fantastic resource for analyzing
retail trends and inventory management.

Here's what the dataset includes:

-   **Store**: The store number, identifying individual Walmart
    > locations.

-   **Date**: The week of sales, giving a time dimension for trend
    > analysis.

-   **Weekly\_Sales**: Sales figures for each store during the specified
    > week.

-   **Holiday\_Flag**: A binary flag indicating whether the week was a
    > holiday (1 for holiday, 0 for non-holiday).

-   **Temperature**: The temperature on the day of sales, providing
    > insights into how weather might influence shopping habits.

-   **Fuel\_Price**: The cost of fuel in the region, potentially
    > affecting transportation costs and customer behavior.

-   **CPI (Consumer Price Index)**: The prevailing CPI, reflecting
    > inflation levels during the time.

-   **Unemployment**: The prevailing unemployment rate, offering an
    > economic context for sales trends.

The dataset also identifies specific **holiday events**, such as:

-   **Super Bowl**

-   **Labour Day**

-   **Thanksgiving**

-   **Christmas**

What makes this dataset particularly interesting is its breadth: it
combines sales figures with external factors like **weather, fuel
prices, and economic indicators**. This allows for a multi-dimensional
analysis, where we can explore not only *what* happened but also *why*.
For example:

-   Did higher unemployment rates correlate with lower sales?

-   Were holiday weeks consistently stronger in sales across all stores?

-   How did fuel prices and weather conditions influence shopping
    > patterns?

**Getting Started with the Data**

Before diving into the project objectives and solving problems, I
started by downloading the Walmart sales dataset onto my machine. To
work with the data effectively, I imported it into **MySQL Workbench**,
which provided an intuitive interface for running queries and exploring
the data.

Importing the dataset into MySQL Workbench was a straightforward
process:

1.  I saved the dataset as a .csv file.

2.  Using the **Import Wizard** in MySQL Workbench, I created a new
    > schema and loaded the data into a table called walmart.

3.  After ensuring the data was imported correctly, I reviewed the table
    > structure and confirmed that all fields were properly formatted
    > for analysis.

**Project Objectives**

List the key questions or problems you aim to solve:

1.  Identify the year with the highest sales.

select year(STR\_TO\_DATE((DATE), \'%d-%b-%y\')) as
year,sum(weekly\_sales) as total\_sales\
,dense\_rank()over(order by sum(weekly\_sales) desc) as rnk\
from\
WalmartDataProject.walmart\
group by year(STR\_TO\_DATE((DATE), \'%d-%b-%y\'))\
order by sum(weekly\_sales) desc;

![A screenshot of a phone AI-generated content may be
incorrect.](media/image2.png){width="6.5in"
height="2.3854166666666665in"}

2024 year has highest sales volume.

2\. Evaluate the impact of holidays on sales trends.

SELECT\
case when holiday\_flag = 1 then \"Holiday\" else \"Not a Holiday\" end
as Holiday\_Info,\
CONCAT(\'\$\',ROUND(AVG(Weekly\_Sales)/1000000, 2), \'M\') AS
Avg\_Weekly\_Sales,\
CONCAT(\'\$\',ROUND(SUM(Weekly\_Sales)/1000000000,2), \'B\') AS
Total\_Sales,\
COUNT(\*) AS Week\_Count\
FROM WalmartDataProject.walmart\
GROUP BY case when holiday\_flag = 1 then \"Holiday\" else \"Not a
Holiday\" end;

![A screenshot of a sales report AI-generated content may be
incorrect.](media/image3.png){width="6.5in"
height="1.3715277777777777in"}

**Holiday weeks sales are 7.69% more than non-holiday weeks.**

Though holiday weeks accounted for only 450** weeks**, they
contributed **nearly 8%** of total sales, highlighting their importance.

3\. How did fuel prices and weather conditions influence shopping
patterns?

SELECT\
year(STR\_TO\_DATE((DATE), \'%d-%b-%y\')) AS \"Year\",\
ROUND(AVG(Fuel\_Price), 2) AS \"Avg Fuel Price (\$)\",\
CONCAT(\'\$\', ROUND(AVG(Weekly\_Sales) / 1000000, 2), \'M\') AS \"Avg
Weekly Sales (Millions)\"\
FROM WalmartDataProject.walmart\
GROUP BY year(STR\_TO\_DATE((DATE), \'%d-%b-%y\'))\
ORDER BY year(STR\_TO\_DATE((DATE), \'%d-%b-%y\'));\
\
\
SELECT\
year(STR\_TO\_DATE((DATE), \'%d-%b-%y\')) AS \"Year\",\
ROUND(AVG(Temperature), 1) AS \"Avg Temperature (°F)\",\
CONCAT(\'\$\', ROUND(AVG(Weekly\_Sales) / 1000000, 2), \'M\') AS \"Avg
Weekly Sales (Millions)\"\
FROM WalmartDataProject.walmart\
GROUP BY year(STR\_TO\_DATE((DATE), \'%d-%b-%y\'))\
ORDER BY year(STR\_TO\_DATE((DATE), \'%d-%b-%y\'));

![A screenshot of a phone AI-generated content may be
incorrect.](media/image4.png){width="6.5in"
height="1.8652777777777778in"}

![A screenshot of a phone AI-generated content may be
incorrect.](media/image5.png){width="6.5in"
height="1.6770833333333333in"}

Though the difference is small, we can see a correlation with fuel and
weather to the sales.

In 2023, as the fuel price and temperature decreased, we could see
slight increase in sales.
