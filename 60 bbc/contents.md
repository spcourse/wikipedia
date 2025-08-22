# Get Data from a Live Website

The goal of this assignment is simple: write a script that scrapes information from a website of your choosing.

Keep in mind that scraping data from a live website can be challenging for several reasons:

* The site may require a login.
* It may have active protections against bots (web scraping).
* It may use dynamic elements (e.g., JavaScript) that only load after specific user interactions.

Make sure to choose a website that is not too difficult to scrape.

## Example

For instance, the [BBC News](https://www.bbc.com/) website is relatively straightforward to scrape. You could write a script of only a few lines that retrieves all the headlines and descriptions of news articles from the main page (no crawling required) and saves them in a two-column `.csv` file:

```
headline,description
"The race for the two-mile-a-second super...","Russia and China are leading the..."
"Appeals court throws out Trump's $500m...","In its 323-page ruling, the New York..."
...
```

## Specifications

* Write your own scraper that retrieves information from a website of your choosing and saves it in a `.csv` file.
* Choose a website that is easy to scrape.
* You may use the BBC example, but for full marks, choose a different website.
