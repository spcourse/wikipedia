# Visualize GDP Per Capita

## Specification

Create a program named `visualize_gdp.py` that generates a histogram showing the distribution of GDP per capita from data collected in the previous assignment. Execute the program with the following command:

    python visualize_gdp.py gdp_per_capita.csv

The program should load the data into a Pandas DataFrame and use [Matplotlib](https://matplotlib.org/) to create the histogram plot. The resulting plot should resemble the example below, but feel free to design it as simple or detailed as you prefer, as long as it clearly presents the histogram data.

![](histo.png)

Remember to exclude countries containing missing values (marked as `-1`) from your plot.

## Using ChatGPT Effectively

Normally, using ChatGPT to generate your code is not advised because it can prevent you from learning and might introduce errors. However, once you understand the basics of programming, ChatGPT can be a valuable tool to enhance your knowledge. For example, it can be useful in learning more about Matplotlib. Ensure you phrase your prompts carefully to maximize learning.

Start with a configuration prompt like this:

    You're a tutor teaching Python and I'm a student. I know all essential concepts in Python, but I'm still learning more advanced uses. Let me ask you questions, and answer them in such a way that I can learn from it.

Followed by a question like this:

    Using Matplotlib, how do I align the x-ticks with the bins of a histogram?

Here is the response that chatGPT gave:

### Response

Aligning the x-ticks with the bins of a histogram in Matplotlib can make the visualization clearer and more informative. Here's how you can do it:

### Steps to Align x-ticks with Histogram Bins

1. **Create the histogram**: Use the `plt.hist()` function to create the histogram. Capture the output of the histogram to get the bin edges and other details.

2. **Adjust x-ticks**: After plotting the histogram, use the `plt.xticks()` function to align the x-ticks with the center of each bin.

### Example Code

Here's a full example:

    import matplotlib.pyplot as plt
    import numpy as np

    # Sample data
    data = np.random.randn(1000)

    # Create a histogram
    n, bins, patches = plt.hist(data, bins=10, edgecolor='black', alpha=0.7)

    # Calculate the bin centers
    bin_centers = 0.5 * (bins[:-1] + bins[1:])

    # Set the x-ticks to be at the bin centers
    plt.xticks(bin_centers)

    # Show plot
    plt.show()

### Explanation

- **`n, bins, patches`**: This is the output of `plt.hist()`. Here:
  - `n` contains the counts for each bin.
  - `bins` contains the bin edges.
  - `patches` is a list of the individual bar rectangles.

- **`bin_centers`**: This calculates the center of each bin by averaging the left and right edges of each bin.

- **`plt.xticks(bin_centers)`**: This sets the x-ticks to the positions specified by `bin_centers`, aligning them with the bins.

- **Labels for x-ticks**: You can customize the labels on the x-ticks using the `labels` parameter in `plt.xticks()`, as shown in the example.

### Why This Works

By aligning the x-ticks with the bin centers, you make it easier for viewers to interpret the histogram, as each tick directly corresponds to a bin. This is especially helpful when dealing with continuous data, where understanding the range represented by each bin is crucial.
