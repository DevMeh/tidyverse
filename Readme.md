# DATA 607 Tidyverse Assignment

--------------------------

## 1. Purpose of this project

To create a programming sample vignette that demonstrates how to use one
or more of the capabilities of selected TidyVerse packages using a
dataset selected either from Kaggle or Fivethirtyeight.

## 2. About Tidyverse
![](https://github.com/DevMeh/tidyverse/blob/master/tidy.jpeg)


Tidyverse is a collection of packages designed for datascience
applications. Currently it consists of the following packages:

-   ggplot2 - for creating graphics
-   dplyr - for data manipulation
-   tidyr - to get data into a consistent and ‘tidy’ format
-   readr- to read rectangular data
-   purr - tools for working with functions and vectors
-   tibble - modified data framing tool
-   stringr - for working with strings
-   forcats - tools to solve commong problemsn with factors. For more
    information please visit [Tidyverse](https://www.tidyverse.org/)

## 3. Dataset


The objective of this exercise is to compare the defense spending of the
US, the European Union and India over the years. A such, the dataset
selected for this vignette details defense spending by country from 1960
through 2018. This data was sourced from kaggle and can be found
[here](https://www.kaggle.com/nitinsss/military-expenditure-of-countries-19602019).
This data was first published by the World Bank and a more expanded
dataset with relevant infromation can be found at the [World Bank
Website](https://data.worldbank.org/indicator/MS.MIL.XPND.CD)


## 4. Readr (read\_csv)


Since this data can be considered rectangular and is in a csv format, we
are going to demonstrate the capabilities of the `read_csv` function
from the readr package.

As detailed in the documentation for read\_csv(), the read\_csv()
function is a special case of read\_delim() which is useful for reading
common types of flat file data which is comma separated values. The
syntax and a few limited arguments are as follows:

`read_csv(file, col_names = TRUE, col_types = NULL, skip = 0, n_max = Inf, guess_max = min(1000,n_max), skip_empty_rows = TRUE)`

where:

-   file - path to a file, a connection, or literal data

-   col\_names - If TRUE, the first row of the input will be used as the
    column names, If FALSE, column names will be generated
    automatically: X1, X2, X3 etc.

-   col\_types - If NULL, all column types will be imputed from the
    first 1000 rows on the input.If a column specification created by
    cols(), it must contain one column specification for each column.

-   skip - Number of lines to skip before reading data.

-   n\_max - Maximum number of records to read.

-   skip\_empty\_rows - Should blank rows be ignored



## 5. Tidyr (pivot\_longer)


According to the tidyverse webset, the goal of of tidyr is to create
‘tidy data’ where:

1.  Every column is variable.
2.  Every row is an observation..
3.  Every cell is a single value. When reviewing the dataframe created
    above, we note that the original dataset is in a ‘wide’ format. In
    order to effectively work this dataset we would have to tansform
    this dataset into long form. This is where the pivotting functions
    of the tidyr package comes into play. The specific pivotting fuction
    we will be using is pivot\_longer() which is replacing gather().
    Note that the pivot\_longer() still in development hence this
    exercise uses the devtools version of tidyverse.

As detailed in the [documentation for
pivot\_longer()](https://tidyr.tidyverse.org/reference/pivot_longer.html),
the `pivot_longer()` function " ‘lengthens’ data, increasing the number
of rows and decreasing the number of columns. The syntax and a few
limited arguments are as follows:

`pivot_longer(data, cols, names_to = "name",values_drop_na = FALSE`

where:

-   data - data frame to pivot.

-   cols - columns to pivot into longer format

-   names\_to - string specifying the name of the column to create

-   values\_to - name of the column to create from the data stored in
    cell values

-   values\_drop\_na - If TRUE, will drop rows that contain only NAs.


## 6. dplyr (mutate, filter)


The dplyr library is considered to be the “grammar of data
manipulation”. The five ‘verbs’ of manipulating data with dplyr are:

1.  mutate() - adds new variables that are functions of existing
    variables
2.  select() - picks variables based on their names.
3.  filter() - picks cases based on their values.
4.  summarise() - reduces multiple values down to a single summary.
5.  arrange() - changes the ordering of the rows. For our purposes of
    this exercise we are going to use the mutate and filter functions:

`mutate()` - Currently the expenditure column is in dollars. For
convenience we will create a new column to indicate expenditure in
billions. We will use the mutate() function in dplyr to achieve this.
This functions lets us add new variables to the dataframe while
preserving the old ones. The syntax for mutate is:

`mutate(data, calculation  )`

`filter()`- As stated earlier we selected this data to compare the
defense expenditures of USA, EU and India over the years. We will use
the filter() function to extract the data for just these three entities
from the larger dataframe. The filter function syntax is as follows:

`filter(.data, ogical condition, .preserve = FALSE)`

where:

-   data - data table

-   logical condition - Multiple conditions are combined with &. Only
    rows where the condition evaluates to TRUE are kept.

-   preserve- when FALSE, the grouping structure is recalculated based
    on the resulting data, otherwise it is kept as is.



## 7. ggplot2 (geom_line)
Finally we are going to plot the three expenditures on the same graph using the ggplot2 package. The steps to create a line chart using ggplot is as follows:

#### Step 1:

ggplot() initialize a ggplot object which declares input data frame for a graphic and sets aesthetics intended to be common throughout all subsequent layers. Syntax for creating a ggplot object is
ggplot(data = NULL, mapping = aes(), ...)

Where: - data- dataset to used for plotting.

mapping - list of aesthetic mappings to use for plotting

#### Step 2

geom_line() connects all the observations within the data frame. Note in order to plot lines we have to specify the number of groups within a dataframe , or the column whose discrete values will determine the number of groups. In the example below the column which contrains country codes (‘Code’) will determine grouping since each line is for a single value of ‘Code’

#### Step 3

Other arguments to determine the overall look of the visualiation
