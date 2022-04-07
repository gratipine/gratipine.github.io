---
title: "Simulating data in python"
date: 2022-04-04 19:59:00 -0000
categories: package-explanation
---
## Why we care
There are many reasons why you might care about simulating data in Python. You might need specific examples for a case in your unit tests or you are explaining how people seem to be adopting the company's products. There are several ways to get this example data. 

- For the math masters amongst us it would be a simple thing to make a gamma distribution with one set of params and a normal with another and get the graph they need.
- If you are not quite so comfortable with equations you could just write down some random numbers, plot them and see if they fit your purpose.

However, if you need very specific type of data it is easier to turn to drawdata, a Python package for visualizing data. 
It works from either a Jupyter notebook or an IPython window. 

## Package methods
The package has only 3 public methods - draw_scatter, draw_histogram and draw_line.

### draw_scatter
```python
import drawdata as dd
dd.draw_scatter()
```
The code above produces the following screen:

![Photo of scatter screen](/gratipine.github.io/assets/draw_scatter.png)

Not very interesting at first, but a quick drawing leads to the below:

![Photo of scatter screen with drawn data](./../../../../assets/draw_scatter_2.png)

If you need the data in table format you can simply click on the "copy csv" part and call pandas to the rescue as written below.

```python
import pandas as pd
data_in = pd.read_clipboard(sep=",")
```

### draw_histogram
Drawing a histogram and using the data from it is exactly the same as a scatter plot. Keep in mind that if your cursor is on the plot it is changing data. This is not a "draw once and it is set" kind of environment.
```python
import drawdata as dd
import pandas as pd

dd.draw_scatter()
data_in = pd.read_clipboard(sep=",")
```
An example plot can look like this:

![Photo of histogram screen with drawn data](./../../../../assets/draw_histogram.png)

### draw_line
Finally, drawing of a line works in the same way. For all three types of plots you can add up to 4 different categories. 

If you are trying to explain to someone different relationships between variables, you could do it with a plot like below. The blue line represents no relationship, the red one represents a linear relationship and yellow and purple represent different non-linear relationships. If you are teaching someone what you can model with a linear regression, a quick plot like this makes it clearer faster than trying to explain exponential relationship in maths terms (although that has value too)

![Photo of line screen with drawn data](./../../../../assets/draw_line.png)

## Sources
- [drawdata documentation](https://pypi.org/project/drawdata/)
- [The code in script form](https://github.com/gratipine/ci_example/blob/main/notebooks/draw_data.py)
