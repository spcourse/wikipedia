# Scraping

**Is there a relationship between income inequality, population size, and wealth?**

We often encounter scenarios where the data we need isn't readily available in machine-readable formats like CSV or JSON files, but rather embedded within websites. For this exercise, even though machine-readable data for our question might exist elsewhere, let’s imagine we’re only allowed to use Wikipedia. This way, we can practice the skills needed to extract and work with data retrieved directly from web pages.

For instance, Wikipedia has a [lists of countries by GDP per capita](https://en.wikipedia.org/wiki/List_of_countries_by_GDP_(nominal)_per_capita), which we can use as a proxy for a country's wealth. Linked within these entries is also information on the [Gini coefficient](https://en.wikipedia.org/wiki/Gini_coefficient), which indicates income inequality. (If you're an economist, I apologize for the oversimplification).

Our goal is to use Wikipedia data to create a plot similar to this:

![](final.png)

To achieve this, you'll need to write a script that can extract data from a HTML source; a process known as scraping. It's important to note that many companies dislike of scraping, and take measures against it.

Wikipedia doesn't specifically prohibit scraping, but to prevent any issues and ensure data consistency during your project, we've provided a bare-bones copy of the relevant webpages for you to practice on: [Fake Wiki](https://spcourse.github.io/wiki/).

You'll develop the script in two phases. In the first phase, you focus only on GDP per capita data. In later phases you'll also include data on the Gini coefficient and population.

## Getting started

### Pipeline

We'll divide the process into two main steps:

1. **Data Acquisition**: Create a script to scrape the Fake Wiki, extract data into a Pandas dataframe, and save it as a CSV file.
2. **Data Visualization**: Develop another script to read the CSV file into a Pandas dataframe and generate a plot.

Using separate scripts for scraping and visualization simplifies maintenance and debugging.

### Libraries

This assignment requires three libraries:

- **Requests** to retrieve webpages from the internet. We provide some functions that use this library.
- **BeautifulSoup** for parsing the Document Object Model (DOM). We provide some examples that should help you understand how to use it, but it can be wise to look at the [BeautifulSoup documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- **Pandas** for managing data in Python. [Pandas documentation](https://pandas.pydata.org/docs/).

These libraries should all already be installed if you are using our environment.

### Downloading Webpages

First, you need to download the relevant webpage using the `requests` package. To get you started we have provided you with some code which you can find in [`helpers.py`](../downloads/helpers.py)

The file helpers.py contains the function `simple_get(url)` that can be used to retrieve the HTML of a webpage by giving it's `url` as a string. It also contains a brief example of how to use the `simple_get` function.

Check out the code and run the example:

    python helpers.py

### BeautifulSoup

To scrape data from webpages, we use BeautifulSoup — a Python library for extracting data from HTML and XML files. It simplifies the process of navigating, searching, and modifying the parse tree.

Begin with watching the following tutorial on HTML and the DOM. Ignore any reference to JavaScript (you may replace it in your mind with BeautifulSoup), as it will not be needed in our exercises.

![embed](https://www.youtube.com/embed/ng2o98k983k)

## Building `scraper.py`

Start by writing a script in a file named (`scraper.py`) that loads the correct Wiki address, uses the `simple_get` function to download the webpage, parses the HTML with BeautifulSoup, and extracts the header text:

    from bs4 import BeautifulSoup
    from helpers import simple_get

    WIKI_URL = 'https://spcourse.github.io/wiki/'

    html = simple_get(WIKI_URL)
    dom = BeautifulSoup(html, 'html.parser')
    title = dom.find('header', {'class': 'mw-body-header'})
    print(title.text)

#### Find DOM Objects

You will frequently need to use the `find()` and `find_all()` methods. `find()` always returns the first occurrence of the element you are searching and `find_all()` always returns a list (even if there is only one match). For example, to locate and print the row for Iceland in the GDP table:

    # Get all tables in the website
    all_tables = dom.find_all('tbody')  

    # Select the second table
    gdp_table = all_tables[1]

    # Get all table-rows
    all_rows = gdp_table.find_all('tr')

    # Show the 12th row (Iceland)
    print(all_rows[12])

#### Extracting Text

To extract the text from a DOM element you can use the `text`-field. For instance, the following piece of code prints all the texts of the cells in row 12.

    cells_row_12 = all_rows[12].find_all('td')
    for cell in cells_row_12:
        print(cell.text)

## Specification

- Extend `scraper.py` to locate and extract GDP per capita information (**using the World Bank source**) in the DOM
- Create a Pandas DataFrame with two columns: `'country-name'` and `'gdp-per-capita'`.
- The DataFrame should be saved to a CSV file, formatted as follows (but with all 222 countries listed):

      country-name,gdp-per-capita
      Monaco,240862
      Liechtenstein,187267
      Luxembourg,128259
      ...
      Afghanistan,353
      Syria,421
      Burundi,200

- If the GDP information is missing you should set the value to `-1`.
- Make sure that the output csv file can be specified as a command line argument by using `argparse`. You should be able to call the script as follows:

      python scraper.py name-of-output-file.csv

> This exercise ... design TODO

## Hints

### DOM inspector

The DOM inspector of your web browser is your biggest ally! In Chrome and Firefox you can right-click an element and click 'inspect', or you can use F12 (fn+F12 on most laptops) to toggle the inspector. All popular browsers have similar functionality.

### Potential Challenges

- Converting the GDP information to an integer or float is not trivial; consider how you might accomplish this.
- Not every row contains the same number of cells due to missing data, which may complicate finding the cell that contains the World Bank GDP figure.

### Come to class

If you encounter difficulties, attending class can often help to find quick solutions to common issues. This is generally good advice, but specifically so for this assignment.

### Use comments!

Keep in mind that the use of BeautifulSoup will not necessarily result in beautiful code. In fact, data collection through scraping (as well as taking ingredients out of a soup) is known to be a very messy practice. You might need to heavily comment code to make sure that it is understandable!
