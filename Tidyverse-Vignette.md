1. Purpose of this project
--------------------------

To create a programming sample vignette that demonstrates how to use one
or more of the capabilities of selected TidyVerse packages using a
dataset selected either from Kaggle or Fivethirtyeight.

2. About Tidyverse
------------------

\[\] (tidy.jpeg)

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

3. Dataset
----------

The objective of this exercise is to compare the defense spending of the
US, the European Union and India over the years. A such, the dataset
selected for this vignette details defense spending by country from 1960
through 2018. This data was sourced from kaggle and can be found
[here](https://www.kaggle.com/nitinsss/military-expenditure-of-countries-19602019).
This data was first published by the World Bank and a more expanded
dataset with relevant infromation can be found at the [World Bank
Website](https://data.worldbank.org/indicator/MS.MIL.XPND.CD)

    #importing the tidyverse library
    library(tidyverse)

    ## ── Attaching packages ─────────────────────────

    ## ✔ ggplot2 3.2.1          ✔ purrr   0.3.3     
    ## ✔ tibble  2.1.3          ✔ dplyr   0.8.3     
    ## ✔ tidyr   1.0.0.9000     ✔ stringr 1.4.0     
    ## ✔ readr   1.3.1          ✔ forcats 0.4.0

    ## ── Conflicts ───────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

4. Readr (read\_csv)
--------------------

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

<!-- -->

    #creating a dataframe called military by importing the Military Expenditure data

    military <- read_csv('Military Expenditure.csv',col_names = TRUE, col_types = NULL ,skip = 0 , n_max =Inf, skip_empty_rows = TRUE)

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   Name = col_character(),
    ##   Code = col_character(),
    ##   Type = col_character(),
    ##   `Indicator Name` = col_character()
    ## )

    ## See spec(...) for full column specifications.

5. Tidyr (pivot\_longer)
------------------------

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

<!-- -->

    # creating a datframe called mil_long by pivoting all of the columns that contain years using the regex matches('^[0-9]')
    mil_long <- pivot_longer(military, cols=matches('^[0-9]'),names_to="year", values_to='expenditure')

    # reviewing the first 5 lines of the dataframe mil_long
    head(mil_long)

    ## # A tibble: 6 x 6
    ##   Name  Code  Type    `Indicator Name`                   year  expenditure
    ##   <chr> <chr> <chr>   <chr>                              <chr>       <dbl>
    ## 1 Aruba ABW   Country Military expenditure (current USD) 1960           NA
    ## 2 Aruba ABW   Country Military expenditure (current USD) 1961           NA
    ## 3 Aruba ABW   Country Military expenditure (current USD) 1962           NA
    ## 4 Aruba ABW   Country Military expenditure (current USD) 1963           NA
    ## 5 Aruba ABW   Country Military expenditure (current USD) 1964           NA
    ## 6 Aruba ABW   Country Military expenditure (current USD) 1965           NA

6. dplyr (mutate, filter)
-------------------------

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

    # creating a new column using mutate and dividing the expenditure column by 1 billion
    mil_long <- mutate(mil_long, spend_in_billions = expenditure/1000000000)

    # reviewing the first 5 lines of the dataframe mil_long
    head(mil_long)

    ## # A tibble: 6 x 7
    ##   Name  Code  Type   `Indicator Name`         year  expenditure spend_in_billio…
    ##   <chr> <chr> <chr>  <chr>                    <chr>       <dbl>            <dbl>
    ## 1 Aruba ABW   Count… Military expenditure (c… 1960           NA               NA
    ## 2 Aruba ABW   Count… Military expenditure (c… 1961           NA               NA
    ## 3 Aruba ABW   Count… Military expenditure (c… 1962           NA               NA
    ## 4 Aruba ABW   Count… Military expenditure (c… 1963           NA               NA
    ## 5 Aruba ABW   Count… Military expenditure (c… 1964           NA               NA
    ## 6 Aruba ABW   Count… Military expenditure (c… 1965           NA               NA

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

<!-- -->

    # cFilter out all values for USA, European union and India
     mil_short<- filter(mil_long, Code %in% c('USA', 'EUU', 'IND'))

    # reviewing the first 5 lines of the dataframe mil_short
    head(mil_short)

    ## # A tibble: 6 x 7
    ##   Name    Code  Type      `Indicator Name`    year  expenditure spend_in_billio…
    ##   <chr>   <chr> <chr>     <chr>               <chr>       <dbl>            <dbl>
    ## 1 Europe… EUU   Regions … Military expenditu… 1960  18900106694             18.9
    ## 2 Europe… EUU   Regions … Military expenditu… 1961  20513847216             20.5
    ## 3 Europe… EUU   Regions … Military expenditu… 1962  23162484939             23.2
    ## 4 Europe… EUU   Regions … Military expenditu… 1963  25129718765             25.1
    ## 5 Europe… EUU   Regions … Military expenditu… 1964  26446753894             26.4
    ## 6 Europe… EUU   Regions … Military expenditu… 1965  27776682702             27.8

7. ggplot2 (geom\_line)
-----------------------

Finally we are going to plot the three expenditures on the same graph
using the ggplot2 package. The steps to create a line chart using ggplot
is as follows:

#### Step 1:

ggplot() initialize a ggplot object which declares input data frame for
a graphic and sets aesthetics intended to be common throughout all
subsequent layers. Syntax for creating a ggplot object is ggplot(data =
NULL, mapping = aes(), …)

Where: - data- dataset to used for plotting.

mapping - list of aesthetic mappings to use for plotting

#### Step 2

geom\_line() connects all the observations within the data frame. Note
in order to plot lines we have to specify the number of groups within a
dataframe , or the column whose discrete values will determine the
number of groups. In the example below the column which contrains
country codes (‘Code’) will determine grouping since each line is for a
single value of ‘Code’

#### Step 3

Other arguments to determine the overall look of the visualiation

    # ggplot- defining x and y axis along with grouping criteria
    # geom_line- specifiying that each 'Code' should get its own color
    # theme- reducing the size of the x axis label and rotating it 90 degrees to avoid overlapping labels)
    ggplot(mil_short, aes(x=year,y=spend_in_billions, group =Code))  + geom_line(aes(color=Code)) + theme(axis.text.x = element_text(size = 7, angle = 90, hjust = 1)) 

![](Tidyverse-Vignette_files/figure-markdown_strict/ggplot-1.png)
