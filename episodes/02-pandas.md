---
title: Loading and Analyzing Argo Float Data
teaching: 20
exercises: 10
---

::::::::::::::::::::::::::::::::::::::::::  objectives

- "Explain what a library is and what libraries are used for."
- "Import a Python library and use the functions it contains."
- "Select individual values from data."
- "Perform operations on tables of data."

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::  questions



::::::::::::::::::::::::::::::::::::::::::::::::::


Words are useful, but what's more useful are the sentences and stories we build with them.
Similarly, while a lot of powerful, general tools are built into Python,
specialized tools built up from these basic units live in
[libraries](learners/reference.md#library)
that can be called upon when needed.

## Loading data with ArgoPy

All of the data recorded by Argo floats is sent to a 
Data Assembly Centre (DAC). After some checks of the data have been made it is sent to a Global Data Assembly Centre (GDAC).
There are two of these, one in the USA and one in France, but they both hold a copy of all of the Argo data ever received. 
To make accessing the data easy from Python a special library called `argopy` has been developed. This can load data directly
from one of the GDACs. This saves us having to search through the GDAC, picking the data we want and downloading it to a file on our computer. 

To tell Python that we'd like to start using `argopy`,
we need to [import](learners/reference.md#import) it:

```python
import argopy
```
Importing a library is like getting a piece of lab equipment out of a storage locker and setting it
up on the bench. Libraries provide additional functionality to the basic Python package, much like
a new piece of equipment adds functionality to a lab space. Just like in the lab, importing too
many libraries can sometimes complicate and slow down your programs - so we only import what we
need for each program.


The `argopy` library has a lot of different features, but we want to use the `DataFetcher` function which gets data from a GDAC. 
The `ArgoDataFetcher` will return something called a class that has more functions we can call. One of these is called `profile` and that 
gets an individual profile given a float number and a profile number. We're going to look at data from profile 12 of float number 6902746.

```python
argopy.DataFetcher().profile(6902746, 12)
```

The expression `argopy.DataFetcher().profile(....)` is a
[function call](learners/reference.md#function-call)
that asks Python to run the [function](learners/reference.md#function) `profile` which
belongs to the `DataFetcher` class which, in turn, belongs to the `argopy` library.
The dot notation in Python is used most of all as an object attribute/property specifier or for invoking its method. `object.property` will give you the object.property value, `object_name.method()` will invoke an object_name method.

::::::::::::::::::::::::::::::::::::::::::  callout

## Functions, Parameters and Return Values

 * In the last episode we looked at using the `print` and `type` functions which are built into Python.
 * We "call" a function by writing its name followed by a `(`, then we can give the values of any
parameters that the function might need. If there is more than one of these we separate each of them 
with a comma. Finally we write a closing `)` to end the function call. 
```python
function_name(first_parameter, second_parameter)
```
 * Parameters have to be given in the order the function expects them. 
   Alternatively we can put a name in front of each paraemter followed by an `=` sign and the parameter 
   value or the name of the variable we are sending.
```python
function_name(parameter_name=first_parameter_value)
```   
 * Functions can also send data back to the code which called them, this is known as "returning" data
from a function. 
 * We can save this return data into a variable to use it again later. If we 
don't save it into a variable then its value is displayed on the screen. 
```python
my_variable = function_name(first_parameter)
```
 * When we import a library like `argopy` more functions become available to us. 

::::::::::::::::::::::::::::::::::::::::::::::::::

`argopy.DataFetcher().profile` has two [parameters](learners/reference.md#parameter): a float number and a profile number.

If we run the profile function with the float number and profile number we get back a `datafetcher.erddap` object.


```output
<datafetcher.erddap>
Name: Ifremer erddap Argo data fetcher for floats
API: https://erddap.ifremer.fr/erddap/
Domain: phy;WMO6902746
Performances: cache=False, parallel=False
User mode: standard
Dataset: phy
```

This doesn't contain much useful data, although it does tell us which GDAC supplied the data. To get the actual data we need to call
yet another function that the `datafetcher.erdapp` object provides called `to_dataframe`. This gets the data ready for processing using another
library called [Pandas](https://pandas.pydata.org/), which is a data analysis library that is particularly good at handling tabular data. 

```python
argopy.DataFetcher().profile(6902746, 12).to_dataframe()
```

Now we get a lot more information including a dataframe (a table of data) for this float. These data correspond to Argo float data. Each row represents one reading and the columns are the different data values.

::::::::::::::::::::::::::::::::::::::::::  callout

## Not All Functions Have Input
Generally, a function uses inputs to produce outputs.
However, some functions produce outputs without
needing any input. These functions don't need any parameters, so we just write `()` after the function name. 
```python
function_name()
```
For example, checking the current time
doesn't require any input.
```python
import time
print(time.ctime())
```
```output
Sat Mar 26 13:07:33 2016
```
We still need parentheses (`()`)
to tell Python to go and do something for us.

::::::::::::::::::::::::::::::::::::::::::::::::::

Since we haven't told it to do anything else with the function's output,
the [notebook](learners/reference.md#notebook) displays it.
In this case,
that output is the data we just loaded. (It is worth noting that only part of the table is automatically displayed.)

Our call to `argopy.DataFetcher().profile` loads our data but didn't save the data in memory.
To do that,
we need to assign the table to a variable. In a similar manner to how we assign a single
value to a variable, we can also assign a table of values to a variable using the same syntax.
Let's capture this into a variable called `data_table`.

```python
data_table=argopy.DataFetcher().profile(6902746, 12).to_dataframe()
```

This statement doesn't produce any output because we've assigned the output to the variable `data_table`.
If we want to check that the data have been loaded,
we can print the variable's value:

```python
print(data_table)
```

Now we have a table with our data. 

Let's check its type.

```python
print(type(data_table))
```

```output
<class 'pandas.core.frame.DataFrame'>
```

The output tells us that `data_table` currently refers to a Pandas dataframe (as we expect), the functionality for which is provided by the Pandas library.

## Exploring the data

We can use this dataframe to access information about the data contained inside the dataframe. For example, we can find how many rows and columns the table has by using `shape`:

```python
print(data_table.shape)
```

```output
(108, 18)
```

This shape matches the information we saw when we printed the dataframe earlier. It tells us that the `data_table` dataframe variable contains 108 rows and 18 columns. 

However, we can see from when we printed the dataframe earlier that the last row is number 107. There are 108 rows - the first row is row 0.
Programming languages like Fortran, MATLAB and R start counting at 1
because that's what human beings have done for thousands of years.
Languages in the C family (including C++, Java, Perl, and Python) count from 0
because it represents an offset from the first value in an array or list (the second
value is offset by one index from the first value). This is closer to the way
that computers represent arrays (if you are interested in the historical
reasons behind counting indices from zero, you can read
[Mike Hoye's blog post](https://exple.tive.org/blarg/2013/10/22/citation-needed/)).

We can also use the `info` function to get a summary of the dataframe:

```python
print(data_table.info())
```

As well as telling us how many columns and rows there are, this tells us more information, like the labels of all the columns and the data types of the values inside the table. We have already used the `type` function; however this will only tell you that
a variable is a dataframe but won't tell you the type of
data inside the dataframe. The column called `Dtype` tells you the data type. For example, the data in the pressure (`PRES`) column are
[floating-point numbers](learners/reference.md#floating-point-number). 

If we are interested in one of those columns and want to access it, we do that by adding its label to the 
end of the command; for example, to get temperature we add `["TEMP"]`. TEMP is in quotes because the column labels are strings and we use square brackets around the column label, just as we
do in math when referring to an element of a matrix.

```python
temp_data = data_table['TEMP']
print(temp_data)
```

This no longer looks like a dataframe. Let us check what type `temp_data` is.

```python
print(type(temp_data))
```

```output
<class 'pandas.core.series.Series'>
```

This is a series, which is a one dimensional table (unlike a dataframe, which is a two dimensional table). It looks like a two dimensional table because it is labelled - the first column (in this case called `N_POINTS` contains the labels).

If we are interested in a particular temperature value, say, the first, we can pick this out by using the label of the value we want. In this case, the label is 0. We are going to use the `at` function Pandas provides to do this:

```python
print('first temperature value:', temp_data.at[0])
```

```output
first temperature value: 28.898
```

:::::::::::::::::::::::::::::::::::::::  challenge
## Explore salinity

If you haven't already, write some Python code to load in the data from profile 12 of float number 6902746. 
What is the last salinity value? (Salinity is called PSAL, as temperature was called TEMP.)

:::::::::::::::  solution

```python
import argopy
sal_data = argopy.DataFetcher().profile(6902746, 12).to_dataframe()["PSAL"]
print('last salinity value:', sal_data.at[107])
```

```output
34.988
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::


## Analyzing data

Pandas has several useful functions that take a series or dataframe as input to perform operations on its values. We are going to use some of these functions on a series first.
If we want to find the average of all our Argo float temperature data, for example, we can ask for `temp_data`'s mean value:

```python
print(temp_data.mean())
```

```output
13.058639
```

Let's use two other Pandas functions to get some descriptive values about the temperature range.

```python
maxval = temp_data.max()
minval = temp_data.min()

print('Max temperature:', maxval)
print('Min temperature:', minval)
```

Here we've assigned the return value from `temp_data.max()` to the variable `maxval` and the value
from `temp_data.min()` to `minval`. Note that we used `maxval`, rather than just `max` - it's
not good practice to use variable names that are the same as [Python keywords](https://docs.python.org/3/reference/lexical_analysis.html#keywords)
or fuction names.

```output
Max temperature: 28.906999588012695
Min temperature: 3.693000078201294
```

We can also use these functions directly on the dataframe, rather than on a series.

```python
print(data_table.mean(numeric_only=True))
```

```output
CYCLE_NUMBER       1.200000e+01
PLATFORM_NUMBER    6.902746e+06
POSITION_QC        1.000000e+00
PRES               7.757407e+02
PRES_ERROR         2.400000e+00
PRES_QC            1.000000e+00
PSAL               3.559740e+01
PSAL_ERROR         9.999998e-03
PSAL_QC            1.000000e+00
TEMP               1.305864e+01
TEMP_ERROR         2.000000e-03
TEMP_QC            1.000000e+00
TIME_QC            1.000000e+00
LATITUDE           1.880400e+01
LONGITUDE         -5.923800e+01
dtype: float64
```

Here, we have had to provide the `mean` function with an argument to tell it to only calculate the mean of columns which are integers, floats or booleans. Otherwise we will get an error when it tries to calculate the mean of the `Data Mode` and `Direction` columns.

::::::::::::::::::::::::::::::::::::::::::  callout

## Getting help on functions

How did we know what functions Pandas has and how to use them?
If you are working in IPython or in a Jupyter Notebook, there is an easy way to find out.
If you type the name of something followed by a dot, then you can use
[tab completion](learners/reference.md#tab-completion)
(e.g. after typing `import pandas` type `pandas.Dataframe.` and then press <kbd>Tab</kbd>)
to see a list of all functions and attributes that you can use. After selecting one, you
can also add a question mark (e.g. `pandas.Dataframe.mean?`), and IPython will return an
explanation of the method! This is the same as doing `help(pandas.Dataframe.mean)`.
::::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::  challenge
## Find the temperature range for an Arctic float

The float 5906983 has been deployed in the Arctic by NOC for the MetOffice. You can see a map of where it's been at
https://fleetmonitoring.euro-argo.eu/float/5906983.

Adapt the code above to load profile number 33 from float 5906983. 
Calculate it's minimum, maximum, mean and median temperature. 

We haven't calculated median before, search on the internet or look at the Pandas documentation 
(https://pandas.pydata.org/pandas-docs/stable/reference/series.html) to find out how to calculate this.

:::::::::::::::  solution

```python
temperatures = argopy.DataFetcher().profile(5906983, 33).to_dataframe()['TEMP']

maxval = temperatures.max()
minval = temperatures.min()
meanval = temperatures.mean()
medianval = temperatures.median()

print('Max temperature:', maxval)
print('Min temperature:', minval)
print('Mean Temperature:', meanval)
print('Median Temperature:', medianval)
```

```output
Max temperature: 7.464000225067139
Min temperature: -0.6669999957084656
Mean Temperature: 2.997521
Median Temperature: 3.930999994277954
```

:::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::::::::  keypoints

- "Import a library into a program using `import libraryname`."
- "The `argopy` library can load Argo float data over the internet from the GDAC"
- "Use the `pandas` library to work with tables of data in Python."

::::::::::::::::::::::::::::::::::::::::::::::::::
