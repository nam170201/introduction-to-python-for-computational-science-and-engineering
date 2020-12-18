---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.12
    jupytext_version: 1.7.1
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Pandas - Data Science with Python

+++

Numpy and numpy arrays are our tool of choice for numeric data that resembles vectors, matrices (and higher dimensional tensors).

Where data is gathered from experiments, and in particular where we want to extract meaning from the combination of different data sources, and where data is often incomplete, the pandas library offers a number of useful tools (and has become a standard tool for data scientists).

In this section, we introduce the basics of Pandas.

In particular, we introduce the two key data types in Pandas: the ``Series`` and the ``DataFrame`` objects.

+++

By convention, the `pandas` library is imported under the name `pd` (the same way that `numpy` is imported under the name `np`:

```{code-cell} ipython3
import pandas as pd
```

## Motivational example (Series)

Imagine we are working on software for a greengrocer or supermarket, and need to track the number of apples (10), oranges(3) and bananas (22) that are available in the supermarket. 

We could use a python list (or a numpy array) to track these numbers:

```{code-cell} ipython3
stock = [10, 3, 22]
```

However, we would need to remember separately that the entries are in the order of apples, oranges, and bananas. This could be achieved through a second list: 

```{code-cell} ipython3
stocklabels = ['apple', 'orange', 'banana']
```

```{code-cell} ipython3
assert len(stocklabels) == len(stock)  # check labels and 
                                       # stock are consistent
for label, count in zip(stocklabels, stock):
    print(f'{label:10s} : {count:4d}')
```

The above 2-list solution is a little awkward in two ways: firstly, we have use two lists to describe one set of data (and thus need to be carefuly to update them simulatenously, for example), and secondly, the access to the data given a label is inconvenient: We need to find the index of the label with one list, then use this as the index to the other list, for example

```{code-cell} ipython3
index = stocklabels.index('banana')
bananas = stock[index]
print(f"There are {bananas} bananas [index={index}].")
```

We have come across similar examples in the section on dictionaries, and indeed a dictionary is a more convenient solution:

```{code-cell} ipython3
stock_dic = {'apple': 10, 
             'orange': 3,
             'banana': 22}
```

In a way, the keys of the dictionary contain the stock labels and the values contain the actual values:

```{code-cell} ipython3
stock_dic.keys()
```

```{code-cell} ipython3
stock_dic.values()
```

To retrieve (or change) the value for `apple`, we use `apple` as the key and retrieve the value through the dictionary's indexing notation:

```{code-cell} ipython3
stock_dic['apple']
```

And we can summarise the stock as follows:

```{code-cell} ipython3
for label in stock_dic:
    print(f'{label:10s} : {stock_dic[label]:4d}')
```

This is a vast improvement over the 2-lists solution: (i) we only maintain one structure, which contains a value for every key - so we don't need to check that the lists have the same length. (ii) we can access individual elements through the label (using it as a key for the dictionary). 

+++

The Pandas Series object address the requriments above. It is similar to a dictionary, but with improvements for the given problem:

* the order of the items is maintained
* the values have to have the same type (higher execution performance)
* a (large) number of convenience functionality, for example to deal with missing data, time series, sorting, plotting, and more 

+++

## Pandas `Series`

### Stock example - `Series`

We can create a `Series` object - for example - from a dictionary:

```{code-cell} ipython3
stock = pd.Series({'apple': 10, 
                   'orange': 3,
                   'banana': 22})
```

The default presentation shows the entries one per row, with the label on the left, and the value on the right. 

```{code-cell} ipython3
type(stock)
```

```{code-cell} ipython3
stock
```

The items on the left are referred to as the `index` of the Series, and are available as the `index` attribute of the `series` object:

```{code-cell} ipython3
stock.index
```

```{code-cell} ipython3
type(stock.index)
```

We can also access the list of values for each item, using the `values` attribute:

```{code-cell} ipython3
stock.values
```

Regarding data access, the `Series` object behaves like a dictionary:

```{code-cell} ipython3
stock['apple']
```

```{code-cell} ipython3
stock['potato'] = 101    # adding more values
stock['cucumber'] = 1
```

```{code-cell} ipython3
print(stock)
```

```{code-cell} ipython3
stock
```

We can plot the data as a bar chart:

```{code-cell} ipython3
%matplotlib inline
# settings for jupyter book: svg for html version, high-resolution png for pdf
from IPython.display import set_matplotlib_formats
set_matplotlib_formats('svg', 'png')
import matplotlib as mpl
mpl.rcParams['figure.dpi'] = 400
```

```{code-cell} ipython3
stock.plot(kind='bar')
```

We can sort the data according to the values in the Series (and then plot to visualise):

```{code-cell} ipython3
stock.sort_values().plot(kind='bar')
```

Or sort the index to get alphabetical order of our fruit and vegetables:

```{code-cell} ipython3
stock.sort_index().plot(kind='bar')
```

The `Series` object has a number of numerical methods available, including `mean` and `sum`:

```{code-cell} ipython3
stock.sum()
```

```{code-cell} ipython3
stock.mean()
```

It also behaves like a sequence in that the `len` function returns the number of data points in the Series object:

```{code-cell} ipython3
len(stock)
```

### memory usage

For larger data sets, it might be important to know how many bytes storing the Series costs. The bytes required to store the actual series data are available as

```{code-cell} ipython3
stock.nbytes
```

or from the underlying numpy array directly:

```{code-cell} ipython3
stock.values.nbytes
```

It is 40 bytes, because we have 5 elements stored as int64 (each needing 8 bytes):

```{code-cell} ipython3
stock.dtype
```

The Series object needs additional memory. This can be queried using:

```{code-cell} ipython3
stock.memory_usage()
```

### Statistics

A number of statistical descriptors of the data in the `stock` Series object is available using `describe()`:

```{code-cell} ipython3
stock.describe()
```

As usual, the documentation strings provide documentation (`help(stock.describe)`), and the pandas home page (`https://pandas.pydata.org`) provides links to the Pandas documentation.

+++

## Create Series from list

In the example above, we showed how to create a Series from a dictionary where the keys of the dictionary entries served as the index for the Series object. 

We can also create a Series from a list, an provide an additional index:

```{code-cell} ipython3
stock = pd.Series([10, 3, 22], index=['apple', 'orange', 'banana'])
```

```{code-cell} ipython3
stock
```

If we omit the `index` argument, the Series will assume an integer index:

```{code-cell} ipython3
stock = pd.Series([10, 3, 22])
```

```{code-cell} ipython3
stock
```

However, an index can be added subsequently:

```{code-cell} ipython3
stock.index = ['apple', 'orange', 'banana']
```

```{code-cell} ipython3
stock
```

## Plotting data

Commonly used plots are easily accessible via the `plot()` method of the Series object. We have seen a bar plot above already. The `Series.plot()` method accepts an argument `kind` such as `kind="bar"`, but there is an equivalent method `Series.plot.bar()` avaialble.

Further examples:

```{code-cell} ipython3
stock.plot.pie()
```

To tailor the plot, we can either get the axis object and modify it subsequently:

```{code-cell} ipython3
ax = stock.plot.pie()
ax.set_aspect(1)
ax.set_ylabel(None);
ax.set_title(None);
```

```{code-cell} ipython3
import matplotlib.pyplot as plt
fig, ax = plt.subplots(1, 1, figsize=(9, 3))
stock.plot.bar(ax=ax)
ax.set_title("Current stock");
```

We can also fetch the data from the series and drive the plotting "manually" ourselves:

```{code-cell} ipython3
import matplotlib.pyplot as plt

names = list(stock.index)
values = list(stock.values)

fig, ax = plt.subplots(1, 1, figsize=(9, 3))
ax.bar(names, values)
ax.set_title('Stock');
```

## Missing values

"Real" data sets tend to be incomplete. Dealing with missing values is an important topic in data science. The agreement in Pandas is that the special floating point value "NaN" (standing for `N`ot `a` `N`umber) represents missing data points. For example, if we have a table for the stock, but we don't know the value for `apple`, we would replace it with `NaN`. 

The special `Nan` value in Python can be created using `float('nan')` or using `numpy.nan` if the `numpy` module is imported.

```{code-cell} ipython3
stock['apple'] = float('nan')
```

```{code-cell} ipython3
stock
```

Note that the `dtype` of the `stock` Series object has changed from `int64` to `float64` when we assigned `NaN` to `apple`: the whole series has been converted to float, because `NaN` is only defined for floating point numbers. 

(There is a proposal to create a `NaN` object as part of pandas - this would overcome the above limitation.)

Assume we need to calculate how many items of stock we have in total using the `sum` function:

```{code-cell} ipython3
stock.values
```

A common situation is that we have an incomplete Series or DataFrame (which are multiple Series with the same index) and we want to process with our analysis, but treat the missing values in a special way.

```{code-cell} ipython3
stock.sum()
```

The above example `sum` shows that `NaN` values are simply ignored, which can be convenient.

+++

We can also 'tidy up' the Series object, by removing all entries that have a `NaN` value:

```{code-cell} ipython3
stock.dropna()
```

## Series data access: explicit and implicit (`loc` and `iloc`)

```{code-cell} ipython3
stock = pd.Series({'apple': 10, 
                   'orange': 3,
                   'banana': 22,
                   'cucumber' : 1,
                   'potato' : 110})
```

```{code-cell} ipython3
stock
```

### Indexing

We can access single values through their index as if the stock Series object would be a dictionary:

```{code-cell} ipython3
stock['banana']
```

There is an equivalent and recommended way of using this retrieval using the `loc` (for LOCation?) attribute:

```{code-cell} ipython3
stock.loc['banana']
```

For convenience, pandas also (!) allows us to use integer indexing into the Series object. This is called *implicit* indexing as the series Object doesn't use integers as the index, but the name of the fruits.

For example, we can also retrieve the value for `banana` through its implicit index 2, because it is in row 3 of the Series object (which would need index 2 as we start counting from 0):

```{code-cell} ipython3
stock[2]
```

In this example, this works fine and seems convenient, but can become very confusing if the actual index of the object consists of integers. For that reason, the explicit (and recommended way) of using the indirect indexing is through the `iloc` (ImplicitLOCation) attribute:

```{code-cell} ipython3
stock.iloc[2]
```

### Slicing

```{code-cell} ipython3
stock
```

We can also slice the Series:

```{code-cell} ipython3
stock['orange':'potato']
```

Or skip every second entry:


```{code-cell} ipython3
stock['orange':'potato':2]
```

### Data manipulation

+++

Numerical operations on the series object can be carried for all data values at the same time inthe same way that numpy arrays are processed:

```{code-cell} ipython3
stock - stock.mean()
```

```{code-cell} ipython3
import numpy as np
```

```{code-cell} ipython3
np.sqrt(stock)
```

Where preferred, we can extract the numpy array and work with that:

```{code-cell} ipython3
data = stock.values
```

```{code-cell} ipython3
type(data)
```

```{code-cell} ipython3
data - data.mean()
```

### Import and Export

Pandas (and its objects `Series` and `DataFrame`) support export to and import from a number of useful formats.

For example, we can write a `Series` object into a comma separated value file:

```{code-cell} ipython3
stock.to_csv('stock.csv', header=False)
```

```{code-cell} ipython3
#NBVAL_IGNORE_OUTPUT
!cat stock.csv
```

We can also create a LaTeX representation of the table:

```{code-cell} ipython3
stock.to_latex()
```

We'll come back to reading from files in the `DataFrame` section.

+++

## Data Frame

### Stock Example - `DataFrame`

After having introduced the `Series` object above, we will focus on the second important type in pandas: the `DataFrame`.

As a first description, we could say that the `DataFrame` is similar to a (2d) spreadsheet: it contains rows and columns.

The series object we have studied above is a special case of the `DataFrame`, where the `DataFrame` has only one column.

We'll continue with our stock example:

```{code-cell} ipython3
stock
```

In addition to tracking how many objects of each type we have stocked, we have a second Series object that provides the price per item at which the item is sold:

```{code-cell} ipython3
price = pd.Series({'apple': 0.55, 'banana': 0.50, 'cucumber' : 0.99, 'potato' : 0.17, 'orange': 1.76})
price
```

The `DataFrame` object allows us to treat the two series together. In fact, a convenient way to create the `DataFrame` object is to combine a number of series as follows:

```{code-cell} ipython3
shop = pd.DataFrame({'stock' : stock, 'price' : price})
shop
```

Because both `Series` objects had the same `index` elements, our data is nicely aligned in the `DataFrame` with name `shop`, even though the data was stored in different order in the `price` and `stock`.

+++

If one Series is missing a data point, pandas will insert a `NaN` entry into that field:

```{code-cell} ipython3
price2 = price.copy()
```

```{code-cell} ipython3
price2['grapefruit'] = 1.99
price2
```

```{code-cell} ipython3
pd.DataFrame({'stock' : stock, 'price' : price2})
```

### Accessing data in a DataFramea

```{code-cell} ipython3
shop
```

The data frame has an *index* which is the same for all columns, and shown in bold in the left most column. We can also ask for it:

```{code-cell} ipython3
shop.index
```

Each column has name (here `stock` and `price`):

```{code-cell} ipython3
shop.columns
```

### Extracting columns of data

Using the column names, we can extract one column into a Series object using the index operator (`[]`):

```{code-cell} ipython3
shop['stock']
```

```{code-cell} ipython3
shop['price']
```

### Extracting rows of data

We have two options of extracting a row of data. 

First, explicit indexing using the label of the index in that row:

```{code-cell} ipython3
shop.loc['apple']             # single row is returned as series
```

```{code-cell} ipython3
shop.loc['banana':'cucumber']  # multiple rows are returned as DataFrame
```

Second, we can use the implicit indexing (as for Series objects):

```{code-cell} ipython3
shop.iloc[0]
```

```{code-cell} ipython3
shop.iloc[1:3]
```

**Warning**

Note that there are some inconsistencies here: the explicit slicing with index labels (such as `.loc['banana':'cucumber']`) is inclusive of `cucumber`, whereas in the implicit slicing (such as `.iloc[1:3]`) the row with index `3` is *not* included.

The behaviour of `.loc` is convenient and a good design choice if labels such as strings in our `stock` example are used. The behaviour of `.iloc` is reflecting the normal Python behaviour.

It is thus understandable how we have arrived at the situation.


+++

### Data manipulation with `shop`

The real strength of the DataFrames is that we can continue to process the data conveniently. 

For example, we could work out the financial value of the items we have in stock, and add this as an extra column:

```{code-cell} ipython3
shop['value'] = shop['price'] * shop['stock']
shop
```

Of course we can compute the sum, for example, to estimate the value of the total stock:

```{code-cell} ipython3
shop['value'].sum()
```

If, for whatever reason, we want to swap columns with rows, we can `transpose` the data frame like a numpy array:

```{code-cell} ipython3
shop.transpose()
```

## Example: European population 2017

Here is a second example to demonstrate some use cases of pandas DataFrames.

+++

First, we get the data. It is originally from EUROSTAT (reference "demo_gind")

```{code-cell} ipython3
#NBVAL_IGNORE_OUTPUT
!wget https://fangohr.github.io/data/eurostat/population2017/eu-pop-2017.csv
```

The data source is a comma-separated-value file (CSV), which looks like this:

```{code-cell} ipython3
#NBVAL_IGNORE_OUTPUT
!head eu-pop-2017.csv
```

Pandas has very strong support of reading files from different formats, including MS Excel, CSV, HDF5 and others. Each reading routine has a number of options to tailor the process.

Many data science projects leave the data in their original files, and use a few lines of Python code to import it.

```{code-cell} ipython3
df = pd.read_csv('eu-pop-2017.csv')
```

```{code-cell} ipython3
df
```

We look at the dataframe as it is, and use the 'head()' command which will only show the first 5 lines of data:

```{code-cell} ipython3
df.head()
```

The meaning of the colums, we have to get from metada information. In this case, we have the following description of the data:

- **geo**: the country in question
- **pop17**: the population count of that country as of 1 January 2017
- **pop18**: the population count of that country as of 1 January 2018
- **births**: the number of (live) births in the country during the year 2017
- **deaths**: the number of deaths in that country during the year 2017

The data is provided for all of the 28 European Union members (as of 2017).

+++

We want to use the country as the country name as the index. We can achieve this either with

```{code-cell} ipython3
df2 = df.set_index('geo')
```

```{code-cell} ipython3
df2.head()
```

Note that we cannot change the index in a given DataFrame, so the `set_index()` method returns a new DataFrame. (This happens for many operations.)

An as alternative, we can also modify the import statement to already indicate which column we want to use as the index:

```{code-cell} ipython3
df = pd.read_csv('eu-pop-2017.csv', index_col="geo")
```

```{code-cell} ipython3
df.head()
```

We explore the data by plotting some of it:

```{code-cell} ipython3
df.plot(kind='bar', y='pop17')
```

The above shows the population as of 1 Jan 2017. 

We'll try to improve this in two ways:

- we want to count population in millions. We can do this by dividing all the data by $10^6$.

- it would be interesting to sort the countries in order of size for this plot.


```{code-cell} ipython3
df_millions = df / 1e6
```

```{code-cell} ipython3
df_millions['pop17'].sort_values(ascending=False).plot(kind='bar')
```

The example above selects one column from the data frame (`['pop17'`) and that returns a `Series` object. Then we sort this `Series` object using `sort_values()` according to the values (that's the number of poeple in each country), then we plot this.

+++

Alternatively, we could also create a plot for the whole data frame, but say that the `pop17` is the column for sorting, and that we want to plot only the column with `pop17`:

```{code-cell} ipython3
df_millions.sort_values(by='pop17').plot(kind='bar', y='pop17')
```

We can also plot more than one column at the same time:

```{code-cell} ipython3
ax = df_millions.sort_values(by='pop17').plot(kind='bar', y=['pop17', 'pop18'])
```

We can also fine tune the plot with the usual `matplotlib` commands:

```{code-cell} ipython3
ax = df_millions.sort_values(by='pop17').plot(kind='bar', y='pop17', figsize=(10, 4))
ax.set_ylabel("population 2017 [in millions]")
ax.grid()
ax.set_xlabel(None);  # get rid of default label for x-axis ('geo')
```

Based on the number of births and deaths, we can compute change in population for each country for 2017. This is sometimes called the "natural-change":

```{code-cell} ipython3
df['natural-change'] = df['births'] - df['deaths']
```

```{code-cell} ipython3
df['natural-change'].sort_values()
```

From this, we can see that the population change due to births and deaths in Italy and Germany is decreasing most in absolute terms.

+++

To relate this to the overall size of the population, one often uses rates per year and per 1000 people in the country, such as the birth rate per 1000 inhabitants [1] (and death rate accordingly):

[1] https://en.wikipedia.org/wiki/Birth_rate

```{code-cell} ipython3
df['birth-rate'] = df['births'] / df['pop17'] * 1000
df['death-rate'] = df['deaths'] / df['pop17'] * 1000
df['natural-change-rate'] = df['natural-change'] / df['pop17'] * 1000
```

```{code-cell} ipython3
df.head()
```

We can now look at the natural rate of change of population for each country, which is normalised by the population in that country.

```{code-cell} ipython3
ax = df.sort_values(by='natural-change-rate').plot(kind='bar', y='natural-change-rate', figsize=(10, 4))
ax.set_title("Natural change due to births and deaths per 1000 in 2017");
```

We can show the data together with the underlying birth and death rate data:

```{code-cell} ipython3
tmp = df.sort_values(by='natural-change-rate')

fig, axes = plt.subplots(2, 1, figsize=(12, 6))

tmp.plot(kind='bar', y=['natural-change-rate'], sharex=True, ax=axes[0])
axes[0].set_title("Population change per 1000 in 2017")
tmp.plot(kind='bar', y=['death-rate', 'birth-rate'], sharex=True, ax=axes[1])
```

We haven't used the information we have about the population on 1 January 2018 yet. 

Let's first look at the absolute changes in the population based on the (census?) data from 1 Jan 2017 and 1 Jan 2018:

```{code-cell} ipython3
df['change'] = df['pop18'] - df['pop17']
```

```{code-cell} ipython3
ax = df.sort_values(by='change').plot(y='change', kind='bar')
ax.set_title("Total change in population per country in 2017");
```

With that information, we can estimate migration. (It is important to note that this estimated number will also absorb all inaccuracies or changes of the data gathering method, in the original data described as "statistical adjustment".)

```{code-cell} ipython3
df['migration'] = df['change'] - df['natural-change']
```

```{code-cell} ipython3
df.head()
```

Let's plot the total change of the population per country in the top subfigure, and the contribution from natural changes and migration in the lower subfigure:

```{code-cell} ipython3
tmp = df.sort_values(by='change')
fig, axes = plt.subplots(2, 1, figsize=(12, 6))

tmp.plot(kind='bar', y=['change'], sharex=True, ax=axes[0])
axes[0].set_title("Population changes in 2017")
axes[0].legend(['total change of population (migration + natural change due to deaths and births'])
tmp.plot(kind='bar', y=['migration', 'natural-change'], sharex=True, ax=axes[1])
axes[1].legend(['Migration', "natural change due to deaths and births"])
axes[1].set_xlabel(None);
```

## Further reading

There is a lot more to say about Pandas. The following resources may be useful but there are countless others available:

- Further reading on `[]`, `.loc[]` and `.iloc[]` from Ted Petrou as a [Jupyter Notebook](https://github.com/tdpetrou/Learn-Pandas/blob/master/Selecting%20Subsets/01%20Selecting%20Subsets%20with%20%5B%20%5D%2C%20.loc%20and%20.iloc.ipynb) and [blog entry](https://medium.com/dunder-data/selecting-subsets-of-data-in-pandas-6fcd0170be9c).

- Jake VanderPlas: Python Data Science Handbook [online](https://jakevdp.github.io/PythonDataScienceHandbook/)
