# Crawling

We began with the question: **Is there a relationship between income inequality, population size, and wealth?** So far, we have focused only on wealth (GDP per capita). But what about the other two factors: population size and income inequality (Gini coefficient)?

The challenge is that the webpage we scraped in the previous assignment doesn’t provide this information directly. However, the links in the first column of the table on that page redirect to pages holding more economic information about individual countries. For most countries, though not all of them, this page contains the population size and Gini coefficient.

So for each country in the list we need to follow the relevant link, and scrape the each webpage for information on the Gini coefficient and populations size. This process of navigating through multiple pages and extracting their content is known as *web crawling*.

It's always good practice to break up big problems into smaller ones, so we're going to break up this assignment into two steps:

1. First, we’ll collect all the relevant links for each country and save them into a `.csv` file.
2. Using the `.csv` file, we’ll visit each country’s page to extract the population size and Gini coefficient.

## Step 1, collecting the links

Make a copy of `scraper.py` and call it `scraper2.py`.

You should modify `scraper2.py` to do the following:

1. Collect the links to the country-pages.
2. Store the information in a Pandas dataframe.
3. Write the result to a `.csv` file.

Just as in the previous assignment, make sure that the output `.csv` file can be specified as a command line argument. So you should be able to call the script as follows:

    python scraper2.py gdp_per_capita2.csv

The output `.csv` file should be formatted as shown below, but include all 222 countries:

    country-name,gdp-per-capita,country-link
    Monaco,240862,https://spcourse.github.io/wiki/29/index.html
    Liechtenstein,187267,https://spcourse.github.io/wiki/30/index.html
    Luxembourg,128259,https://spcourse.github.io/wiki/31/index.html
    ...
    Afghanistan,353,https://spcourse.github.io/wiki/270/index.html
    Syria,421,https://spcourse.github.io/wiki/271/index.html
    Burundi,200,https://spcourse.github.io/wiki/272/index.html

Bear in mind that the links in the webpage are relative links (so just `29/index.html` and not `https://spcourse.github.io/wiki/29/index.html`). For the next step you will really need the full URL, so make sure to add the base URL to the relative links!

### Important notes

- **Relative Links:** The links in the scraped webpage are relative (e.g., `29/index.html`). You will need to prepend the base URL (`https://spcourse.github.io/wiki/`) to generate the full URL for each country.
- **Code Reuse:** Where possible, consider importing and reusing functions from `scraper.py` instead of copying and pasting all your code. This ensures consistency and avoids duplicating code unnecessarily. You might need to refactor some of your code in your original `scraper.py` to work in both scenarios, but ultimately this should be worth it!

## Step 2, follow the links and collect data

Write a program called `crawler.py`. This program will follow all the links from the `.csv` you created above and create a new `.csv` containing the population and Gini coefficient for each country. You should be able to call the script as follows:

    python crawler.py gdp_per_capita2.csv gdp_per_capita3.csv

The program should do the following:

1. Read the input `.csv` file into a Pandas DataFrame.
2. Open each country page linked to in the `country-link` column.
3. Get the population and Gini coefficient from the page. Think about what datatypes you need to convert them to. If the data is missing, set them to `-1`.
4. Add the population and Gini coefficient data to the DataFrame
5. Write the result to the output `.csv` file.

The output `.csv` file should be formatted as follows (but with all 222 countries listed):

    country-name,gdp-per-capita,country-link,population,gini
    Monaco,240862,https://spcourse.github.io/wiki/29/index.html,39150,-1.0
    Liechtenstein,187267,https://spcourse.github.io/wiki/30/index.html,38748,-1.0
    Luxembourg,128259,https://spcourse.github.io/wiki/31/index.html,650000,32.3
    ...
    Afghanistan,353,https://spcourse.github.io/wiki/270/index.html,41480304,-1.0
    Syria,421,https://spcourse.github.io/wiki/271/index.html,18604031,-1.0
    Burundi,200,https://spcourse.github.io/wiki/272/index.html,-1,-1.0

## Important notes

Before writing all the code for scraping, take some time to analyze how the data is most commonly displayed. Open a few pages, observe their structure, and think through your approach. This approach will save you time and help you design a more reliable scraper.

Keep in mind that some countries present exceptions to the most common layout. Some examples are:

- **Unusual Notation**: Numbers written in formats like `xxx.xxx.xxx` (like Iran) or `x.x million` (like Cyprus, Armenia, and more).  
- **Lists of Values**: Some countries provide multiple population figures.
- **Additional Information**: Some entries include extra details, such as years, before (Zambia: `(2021 est)x`) or after (Madagascar: `x(2020)`) the population value.  
- **Missing Data**: Entries marked as `N/A` or `NA` for countries like Liechtenstein, Libya, and North Korea.

It’s your task to find solutions in handling these inconsistencies. One approach is to ignore all countries for which your code might crash (*by using `try` and `except`*) and entering `-1` for these countries. Your implementation doesn’t need to handle every edge case perfectly, but it’s important to think critically about how to address such inconsistencies effectively and design a solution that works for the majority of unexpected scenarios.
