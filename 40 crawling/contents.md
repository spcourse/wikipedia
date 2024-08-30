# Crawling

We started with the following question: Is there a relationship between income inequality, population size, and wealth? So far we only looked at wealth (GDP per capita).

What about the other two factors, population size and income inequality (Gini coefficient)?

The problem is that the webpage that you have scraped in the previous assignment did not have this information. However if we follow the links of the countries in the first column of the website, we're redirected to information on the economy of the respective country. For most countries (but not all) this page contains the information we're looking for.

So for each country in the list we need to follow the relevant link, and scrape the each webpage for information on the Gini coefficient and populations size.

This process of following the links to multiple pages and scraping their information is called *crawling*.

It's alway good practice to break up big problems into smaller ones, so we're going to break up this assignment into two steps: In the first step we're going to collect the links that we need to follow into a `.csv` file, in the second step, we're going to scrape each page in the file.\

## Step 1, collect links

Make a copy of `scraper.py` and call it `scraper2.py`.

You should modify `scraper2.py` to do the following:

1. Collect the links to the country-pages.
2. Store the information in a Pandas dataframe.
3. Write the result to a `.csv` file.

Just as in the previous assignment, make sure that the output `.csv` file can be specified as a command line argument. So you should be able to call the script as follows:

    python scraper2.py name-of-output-file2.csv

The output `.csv` file should be formatted as follows (but with all 222 countries listed):

    country-name,gdp-per-capita,country-link
    Monaco,240862,https://spcourse.github.io/wiki/29/index.html
    Liechtenstein,187267,https://spcourse.github.io/wiki/30/index.html
    Luxembourg,128259,https://spcourse.github.io/wiki/31/index.html
    Bermuda,123091,https://spcourse.github.io/wiki/32/index.html
    Ireland,103685,https://spcourse.github.io/wiki/33/index.html
    Switzerland,99995,https://spcourse.github.io/wiki/34/index.html
    ...

Tip bear in mind that the links in the webpage are relative links (so just `29/index.html`. and not `https://spcourse.github.io/wiki/29/index.html`). For the next step you really need the full URL, so make sure to add the base URL to the relative links.

## Step 2, follow the links and collect data

Write a program called `crawler.py`. This program will follow all the links from the `.csv` you created above and create a new `.csv` containing the population and Gini coefficient figures for each countries. You should be able to call the script as follows:

    python crawler.py gdp_per_capita2.csv gdp_per_capita3.csv

The program should do the following:

1. Read the input `.csv` file into a Pandas DataFrame.
2. Open each country page linked to in the `country-link` column.
3. Get the population and Gini coefficient from the page. Think about what datatypes you need to convert them to. If the data is missing, set them to `-1`.
4. Add the population and and Gini coefficient data to the DataFrame
5. Write the result to the output `.csv` file.

The output `.csv` file should be formatted as follows (but with all 222 countries listed):

    country-name,gdp-per-capita,country-link,population,gini
    Monaco,240862,https://spcourse.github.io/wiki/29/index.html,39150,-1.0
    Liechtenstein,187267,https://spcourse.github.io/wiki/30/index.html,38748,-1.0
    Luxembourg,128259,https://spcourse.github.io/wiki/31/index.html,650000,32.3
    Bermuda,123091,https://spcourse.github.io/wiki/32/index.html,-1,-1.0
    Ireland,103685,https://spcourse.github.io/wiki/33/index.html,5149139,28.9
    Switzerland,99995,https://spcourse.github.io/wiki/34/index.html,8981565,31.1
    ...
