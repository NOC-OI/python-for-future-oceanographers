---
title: Loading and Analyzing Argo Float Data
teaching: 20
exercises: 10
---

::::::::::::::::::::::::::::::::::::::::::  objectives

- "Explain what a library is and what libraries are used for."
- "Import a Python library and use the functions it contains."
- "Read tabular data from a file into a program."
- "Select individual values and subsections from data."
- "Perform operations on arrays of data."

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::  questions

- "How can I process tabular data files in Python?"

::::::::::::::::::::::::::::::::::::::::::::::::::


Words are useful, but what's more useful are the sentences and stories we build with them.
Similarly, while a lot of powerful, general tools are built into Python,
specialized tools built up from these basic units live in
[libraries](learners/reference.md#library)
that can be called upon when needed.

## Loading data into Python

To begin processing the Argo data, we need to load it into Python.
We can do that using a library called
[NumPy](https://numpy.org/doc/stable "NumPy Documentation"), which stands for Numerical Python.
In general, you should use this library when you want to do fancy things with lots of numbers,
especially if you have matrices or arrays. To tell Python that we'd like to start using NumPy,
we need to [import](learners/reference.md#import) it:

```python
import numpy
```

Importing a library is like getting a piece of lab equipment out of a storage locker and setting it
up on the bench. Libraries provide additional functionality to the basic Python package, much like
a new piece of equipment adds functionality to a lab space. Just like in the lab, importing too
many libraries can sometimes complicate and slow down your programs - so we only import what we
need for each program.

Before we load any data it can be helpful to tell NumPy not to print all the lines in our data
since some of our data is quite big and we probably don't want to see every line of it. `NumPy` includes
a function called `set_printoptions` which we can use to tell `NumPy` how many lines of our data
to show. 

```python
numpy.set_printoptions(threshold=10)
```

Once we've imported the `NumpPy` library, we can ask it to read our data file for us:

```python
numpy.loadtxt(fname='argo_data.csv', delimiter=',', skiprows=1)
```

But this gives us a FileNotFoundError because we don't have a file called `argo_data.csv` yet.
```output
---------------------------------------------------------------------------
FileNotFoundError                         Traceback (most recent call last)
Cell In[3], line 1
----> 1 numpy.loadtxt(fname='argo_data2.csv', delimiter=',', skiprows=1)
...
```

This file is available from https://raw.githubusercontent.com/NOC-OI/python-for-future-oceanographers/refs/heads/main/data/argo_data.csv.

We can download this using the external command `wget`. This is not part of Python and we can tell
Jupyter to run it by starting the cell with an `!`. 

```python
!wget https://raw.githubusercontent.com/NOC-OI/python-for-future-oceanographers/refs/heads/main/data/argo_data.csv
```

Or we can change the filename to the full web address and Numpy will get the file from the internet for us.

```python
numpy.loadtxt(fname='https://raw.githubusercontent.com/NOC-OI/python-for-future-oceanographers/refs/heads/main/data/argo_data.csv', delimiter=',', skiprows=1)
```

```output
array([[0.0000000e+00, 3.5025002e+01, 2.8898001e+01, 3.0000000e+00],
       [1.0000000e+00, 3.5026001e+01, 2.8898001e+01, 4.0000000e+00],
       [2.0000000e+00, 3.5026001e+01, 2.8896000e+01, 5.0000000e+00],
       ...,
       [1.0500000e+02, 3.4988998e+01, 3.7710000e+00, 1.9380000e+03],
       [1.0600000e+02, 3.4987999e+01, 3.7340000e+00, 1.9630000e+03],
       [1.0700000e+02, 3.4987999e+01, 3.6930000e+00, 1.9890000e+03]])
```


The expression `numpy.loadtxt(...)` is a
[function call](learners/reference.md#function-call)
that asks Python to run the [function](learners/reference.md#function) `loadtxt` which
belongs to the `numpy` library.
The dot notation in Python is used most of all as an object attribute/property specifier or for invoking its method. `object.property` will give you the object.property value, `object_name.method()` will invoke an object_name method.

As an example, John Smith is the John that belongs to the Smith family.
We could use the dot notation to write his name `smith.john`,
just as `loadtxt` is a function that belongs to the `numpy` library.

`numpy.loadtxt` has two [parameters](learners/reference.md#parameter): the name of the file
we want to read and the [delimiter](learners/reference.md#delimiter) that separates values
on a line. These both need to be character strings
(or [strings](learners/reference.md#string) for short), so we put them in quotes. Notice
that we also had to tell NumPy to skip the first row, which contains the column titles.

Since we haven't told it to do anything else with the function's output,
the [notebook](learners/reference.md#notebook) displays it.
In this case,
that output is the data we just loaded.
By default,
only a few rows and columns are shown
(with `...` to omit elements when displaying big arrays).
Note that, to save space when displaying NumPy arrays, Python does not show us trailing zeros,
so `1.0` becomes `1.`.

Our call to `numpy.loadtxt` read our file
but didn't save the data in memory.
To do that,
we need to assign the array to a variable. In a similar manner to how we assign a single
value to a variable, we can also assign an array of values to a variable using the same syntax.
Let's re-run `numpy.loadtxt` and save the returned data:

```python
data = numpy.loadtxt(fname='argo_data.csv', delimiter=',', skiprows=1)
```


This statement doesn't produce any output because we've assigned the output to the variable `data`.
If we want to check that the data have been loaded,
we can print the variable's value:

```python
print(data)
```


```output
[[0.0000000e+00 3.5025002e+01 2.8898001e+01 3.0000000e+00]
 [1.0000000e+00 3.5026001e+01 2.8898001e+01 4.0000000e+00]
 [2.0000000e+00 3.5026001e+01 2.8896000e+01 5.0000000e+00]
 ...
 [1.0500000e+02 3.4988998e+01 3.7710000e+00 1.9380000e+03]
 [1.0600000e+02 3.4987999e+01 3.7340000e+00 1.9630000e+03]
 [1.0700000e+02 3.4987999e+01 3.6930000e+00 1.9890000e+03]]
```


Now that the data are in memory,
we can manipulate them.
First,
let's ask what [type](learners/reference.md#type) of thing `data` refers to:

```python
print(type(data))
```


```output
<class 'numpy.ndarray'>
```


The output tells us that `data` currently refers to a NumPy array, the functionality for which is provided by the NumPy library.
These data correspond to Argo float data. Each row represents one reading and the columns are the different data values.


::::::::::::::::::::::::::::::::::::::::::  callout

## Data Type
A Numpy array contains one or more elements
of the same type. The `type` function will only tell you that
a variable is a NumPy array but won't tell you the type of
thing inside the array.
We can find out the type
of the data contained in the NumPy array.
```python
print(data.dtype)
```
```output
float64
```
This tells us that the NumPy array's elements are
[floating-point numbers](learners/reference.md#floating-point-number).
::::::::::::::::::::::::::::::::::::::::::::::::::

With the following command, we can see the array's [shape](learners/reference.md#shape):

```python
print(data.shape)
```


```output
(108, 4)
```


The output tells us that the `data` array variable contains 108 rows and 4 columns
 (sequence number, conductivity/salinity, temperature and pressure/depth). 

If we want to get a single number from the array, we must provide an
[index](learners/reference.md#index) in square brackets after the variable name, just as we
do in math when referring to an element of a matrix.  Our data has two dimensions, so
we will need to use two indices to refer to one specific value:

```python
print('first temperature value in data:', data[0, 2])
```


```output
first value in data: 28.898001
```


```python
print('middle temperature value in data:', data[53, 2])
```


```output
middle value in data: 9.876
```


The expression `data[53, 2]` accesses the element at the 54th row and 3rd column not 
the 53rd row and 2nd column as you might think. 
Programming languages like Fortran, MATLAB and R start counting at 1
because that's what human beings have done for thousands of years.
Languages in the C family (including C++, Java, Perl, and Python) count from 0
because it represents an offset from the first value in the array (the second
value is offset by one index from the first value). This is closer to the way
that computers represent arrays (if you are interested in the historical
reasons behind counting indices from zero, you can read
[Mike Hoye's blog post](https://exple.tive.org/blarg/2013/10/22/citation-needed/)).
As a result,
if we have an M×N array in Python,
its indices go from 0 to M-1 on the first axis
and 0 to N-1 on the second.
It takes a bit of getting used to,
but one way to remember the rule is that
the index is how many steps we have to take from the start to get the item we want.

![](fig/python-zero-index.svg){alt="'data' is a 3 by 3 numpy array containing row 0: ['A', 'B', 'C'], row 1: ['D', 'E', 'F'], and
row 2: ['G', 'H', 'I']. Starting in the upper left hand corner, data[0, 0] = 'A', data[0, 1] = 'B',
data[0, 2] = 'C', data[1, 0] = 'D', data[1, 1] = 'E', data[1, 2] = 'F', data[2, 0] = 'G',
data[2, 1] = 'H', and data[2, 2] = 'I',
in the bottom right hand corner."}

::::::::::::::::::::::::::::::::::::::::::  callout

## In the Corner
What may also surprise you is that when Python displays an array,
it shows the element with index `[0, 0]` in the upper left corner
rather than the lower left.
This is consistent with the way mathematicians draw matrices
but different from the Cartesian coordinates.
The indices are (row, column) instead of (column, row) for the same reason,
which can be confusing when plotting data.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Slicing data
An index like `[53, 2]` selects a single element of an array,
but we can select whole sections as well.
For example,
we can select the Argo data for the first five readings like this:

```python
print(data[0:5, 0:4])
```


```output
[[ 0.       35.025002 28.898001  3.      ]
 [ 1.       35.026001 28.898001  4.      ]
 [ 2.       35.026001 28.896     5.      ]
 [ 3.       35.025002 28.893     6.      ]
 [ 4.       35.025002 28.892     7.      ]]
```


The [slice](learners/reference.md#slice) `0:5` means, "Start at index 0 and go up to,
but not including, index 5". Again, the up-to-but-not-including takes a bit of getting used to,
but the rule is that the difference between the upper and lower bounds is the number of values in
the slice.

We don't have to start slices at 0:

```python
print(data[5:10, 1:4])
```


```output
[[35.027    28.896     8.      ]
 [35.025002 28.902     9.      ]
 [35.026001 28.900999 10.      ]
 [35.027    28.907    16.      ]
 [35.549999 28.858999 26.      ]]
```


We also don't have to include the upper and lower bound on the slice.  If we don't include the lower
bound, Python uses 0 by default; if we don't include the upper, the slice runs to the end of the
axis, and if we don't include either (i.e., if we use ':' on its own), the slice includes
everything:

```python
first_five = data[:5, 1:]
print('data from first five readings is:')
print(first_five)
```

The above example selects rows 0 through 4 and columns 1 through to the end of the array (which gives us the salinity, temperature and depth).

```output
data from first five readings is:
[[35.025002 28.898001  3.      ]
 [35.026001 28.898001  4.      ]
 [35.026001 28.896     5.      ]
 [35.025002 28.893     6.      ]
 [35.025002 28.892     7.      ]]
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Slicing Strings

A section of an array is called a [slice](learners/reference.md#slice).
We can take slices of character strings as well:

```python
element = 'oxygen'
print('first three characters:', element[0:3])
print('last three characters:', element[3:6])
```

```output
first three characters: oxy
last three characters: gen
```


What is the value of `element[:4]`?
What about `element[4:]`?
Or `element[:]`?

:::::::::::::::  solution
```output
oxyg
en
oxygen
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::  callout

## Not All Functions Have Input
Generally, a function uses inputs to produce outputs.
However, some functions produce outputs without
needing any input. For example, checking the current time
doesn't require any input.
```python
import time
print(time.ctime())
```
```output
Sat Mar 26 13:07:33 2016
```
For functions that don't take in any arguments,
we still need parentheses (`()`)
to tell Python to go and do something for us.

::::::::::::::::::::::::::::::::::::::::::::::::::


## Loading data with ArgoPy

Instead of passing around spreadsheets or CSV files of data, all of the data recorded by Argo floats is sent to a 
Data Assembly Centre (DAC). After some checks of the data have been made it is sent to a Global Data Assembly Centre (GDAC).
There are two of these, one in the USA and one in France, but they both hold a copy of all of the Argo data ever received. 
To make accessing the data easy from Python a special library called `argopy` has been developed. This can load data directly
from one of the GDACs and turn it into a Numpy array. This saves us having to search through the GDAC, picking the data we want and downloading it to a file on our computer. 

```python
import argopy
```

The `argopy` library has a lot of different features, but we want to use the `DataFetcher` function which gets data from a GDAC. 
The `ArgoDataFetcher` will return something called a class that has more functions we can call. One of these is called `profile` and that 
gets an individual profile given a float number and a profile number. The data we've been using came from profile 12 of float number 6902746.

```python
argopy.DataFetcher().profile(6902746, 12)
```

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
yet another function that the `datafetcher.erdapp` object provides called `to_xarray`. This gets the data ready for processing using another
library called Xarray, which works well with Numpy data but is very good at working with really big datasets. 

```python
argopy.DataFetcher().profile(6902746, 12).to_xarray()
```

Now we get a lot more information including a list of what data variables this float has. To get one of those we add its name to the 
end of the command; for example, to get temperature we add `.TEMP`.

```python
argopy.DataFetcher().profile(6902746, 12).to_xarray().TEMP
```
Now we have something which just looks like real data. However one last thing, the type of this data is `xarray.DataArray` not `numpy.ndarray`.
To do that final conversion we add `.values` on the end (note that there's no brackets on this as this is a variable name not a function).

```python
argopy.DataFetcher().profile(6902746, 12).to_xarray().TEMP.values
```

Let's capture this into a variable called `temp_data` and check it's type.

```python
temp_data=argopy.DataFetcher().profile(6902746, 12).to_xarray().TEMP.values
type(temp_data)
```

and now we have a Numpy array with our temperature data. 

```output
numpy.ndarray
```

This should be the same as the 3rd (2nd if you count from zero!) column of our earlier data.
Let's do a basic check of this by comparing the mean values.

```
print(temp_data.mean())
print(data[:,2].mean())
```

```output
13.058639
13.058638888888888
```

The values are slightly differnent because when they got saved into the CSV file they got rounded a little bit. 

## Analyzing data

NumPy has several useful functions that take an array as input to perform operations on its values.
If we want to find the average of all our Argo float data, for example, we can ask NumPy to compute `data`'s mean value:

```python
print(numpy.mean(data))
```


```output
219.47419444212963
```


`mean` is a [function](learners/reference.md#function) that takes
an array as an [argument](learners/reference.md#argument). Given that our
array contains the sequence numbers and three different data variables taking the mean of the
whole array doesn't really make much sense.

We can use slicing to calculate the mean temperature from our dive:

```python
print(numpy.mean(data[:,2]))
```

```output
13.058638888888888
```

Let's use two other NumPy functions to get some descriptive values about the temperature range.

```python
maxval = numpy.max(data[:,2])
minval = numpy.min(data[:,2])

print('Max temperature:', maxval)
print('Min temperature:', minval)
```

Here we've assigned the return value from `numpy.max(data[:,2])` to the variable `maxval` and the value
from `numpy.min(data[:,2])` to `minval`. Note that we used `maxval`, rather than just `max` - it's
not good practice to use variable names that are the same as [Python keywords](https://docs.python.org/3/reference/lexical_analysis.html#keywords)
or fuction names.

```output
Max temperature: 28.907
Min temperature: 3.693
```

::::::::::::::::::::::::::::::::::::::::::  callout

## Getting help on functions

How did we know what functions NumPy has and how to use them?
If you are working in IPython or in a Jupyter Notebook, there is an easy way to find out.
If you type the name of something followed by a dot, then you can use
[tab completion](learners/reference.md#tab-completion)
(e.g. type `numpy.` and then press <kbd>Tab</kbd>)
to see a list of all functions and attributes that you can use. After selecting one, you
can also add a question mark (e.g. `numpy.abs?`), and IPython will return an
explanation of the method! This is the same as doing `help(numpy.abs)`.
::::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::  challenge
## Find the temperature range for an Arctic float

The float 5906983 has been deployed in the Arctic by NOC for the MetOffice. You can see a map of where it's been at
https://fleetmonitoring.euro-argo.eu/float/5906983.

Adapt the code above to load profile number 33 from float 5906983. 
Calculate it's minimum, maximum, mean and median temperature. 

We haven't calculated median before, search on the internet or look at the NumPy documentation 
(https://numpy.org/devdocs/reference/routines.statistics.html) to find out how to calculate this.

:::::::::::::::  solution

```python
temperatures = argopy.DataFetcher().profile(5906983, 33).to_xarray().TEMP.values

maxval = numpy.max(temperatures)
minval = numpy.min(temperatures)
meanval = numpy.mean(temperatures)
medianval = numpy.median(temperatures)

print('Max temperature:', maxval)
print('Min temperature:', minval)
print('Mean Temperature:', meanval)
print('Median Temperature:', medianval)
```

```output
Max temperature: 7.463699817657471
Min temperature: -0.6674000024795532
Mean Temperature: 2.9974963312872487
Median Temperature: 3.9305999279022217
```

:::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::::::::  keypoints

- "Import a library into a program using `import libraryname`."
- "Use the `numpy` library to work with arrays in Python."
- "The expression `array.shape` gives the shape of an array."
- "Use `array[x, y]` to select a single element from a 2D array."
- "Array indices start at 0, not 1."
- "Use `low:high` to specify a `slice` that includes the indices from `low` to `high-1`."
- "Use `numpy.mean(array)`, `numpy.max(array)`, and `numpy.min(array)` to calculate simple statistics."
- "The `argopy` library can load Argo float data over the internet from the GDAC"

::::::::::::::::::::::::::::::::::::::::::::::::::
