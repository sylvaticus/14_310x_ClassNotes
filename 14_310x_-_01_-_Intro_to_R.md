
# 14_310x_Intro_to_R Tutorial #

Markdown version of the swirl() based tutorial.


## Lesson 0 - Starting the tutorial ##

- To updayte all packages after upgrading R: `update.packages(ask = FALSE, checkBuilt=TRUE)` In Linux, do it from root first (sudo -i ...) and then as user
- To install a package: `install.packages("pkgName")`
- To load a package: `library("pkgName")`
- To check the installed version of a package: `packageVersion("pkgName")`
- To clear the workspace: `rm(list=ls())` 

Pachages needed for the tutorial:

- tidyverse
- swirl

To start Swirl Tutorial:

- `library("swirl")`
- `install_course_url("https://lobianco.org/temp/14_310x_Intro_to_R_correctedLesson12.zip",multi=FALSE)`  14_310x_Advanced_R__5_.zip
- `swirl()`
- `uninstall_course("14 310x Intro to R")`  If you wish to uninstall the course


## Lesson 1 - Welcome ##

`help.start()`  Main R help

`ESC`  Return to R prompt

Swirl functions: `skip()`, `play()`, `nxt()`, `bye()`, `main()`, `info()`

## Lesson 2 - Basic building blocks ##
logical operators: `<`, `>`, `<=`, `>=`, `==`, `!=`

`a <- b` To assign b to a (`a = b` also works but is not liked by R purists)

`c(ELEMENTS)` to make a vector. Note that v2 = `c(v1,1,2)` will NOT make a multidimensional array but just append `1` and `2` to `v1` elements

`args(FUNCNAME)` To know the arguments of a function

## Lesson 3 - Workspace and files ##

- `ls()` show the workspace (variables acailable in the current scope)
- `list.files()` or `dir()` to list the files in the current directory
- `getwd()` get the current directory
- `dir.create(DIRNAME, recursive=FALSE)`
- `setwd(DIRNAME)`
- `file.create(FILENAME)`
- `file.exists(FILENAME)`
- `file.info(FILENAME)`
- `file.rename(OLD,NEW)`
- `file.copy(SOURCE,DEST)`
- `file.path(FILENAME)`
- `unlink(DIR or FILNAME, recursive=True)`  Delete directories or files
- ?`FNAME`
- ?`OPERATOR`

## Lesson 4 - Sequences ##

- `seq(from,to,by,length,along.with)`  the first three arguent also by position
- `from:to` (but not `from:range:to` operator. Use `seq`)
- `length(VECTOR)`
- `rep(NUMBER or SEQ, times, each)` Replicate elements

## Lesson 5 - Vectors ##

- `paste(elements, collapse = " ", sep = " ")` To concatenate strings. Elements can be scalars or vectors, `collapse` è il separatore tra gli elementi (if elements are vectors), `sep` è il separatore tra gli argomenti, e.g.: `paste("Hello", "world!", sep = " ")` or `paste(c(1,2,3),c("a","b","c"), sep="q",collapse="w")` # "1qaw2qbw3qc"
- `c(my_char, "your_name_here")` Push elements
- `LETTERS` is a predefined variable in R containing a character vector of all 26 letters in the English alphabet

## Lesson 6 - NA ##
- `NA`,  which stands for "Not Available"",  propagates (i.e. operations with `NA` returns `NA`)
- `NaN`, stands for "not a number""
`Inf` stands for "infinity""
`NULL` is the "engineering NULL". Most functions operating on a `NULL` would raise a run-time error.
`rnorm(1000)` Generatea vector containing 1000 draws from a standard normal distribution
`rep(NA, 1000)` Generate a vector with replication of `NA` for 1000 times
`sample(myvector, 100)` Uniformply sample elements from a vector
`is.na(my_data)` Returns a vector of boolean values relative on each elements of the vector being a na value (don't use  == NA)
R represents `TRUE` as the number `1` and `FALSE` as the number `0`
`sum(myvect)` 

## Lesson 7 - Subsetting vectors ##

- `x[1:10]`
- `x[c(3,5,7)]`  Select positions 3, 5 and 7
- `x[c(-2, -10)]` or `x[-c(2, 10)]` Negative indices indicate except values, but only 0's may be mixed with negative subscripts
- `x[!is.na(x)]` Boolean selection to remove NAs
- R uses "one-based indexing""
- R doesn't report overflow errors (!!!!)
- `vect = c(foo = 11, bar = 2, norf = NA)` Named vectors (associative array)
- `names(vect)` The vector keys
- `names(vect) <- c("foo2", "bar2", "norf2")` What it is doing here ? R has a syntax where, for some functions, you can pass the result of the RHS to the function on the LHS. The same function can hence acts as a "getX()" or "setX()"" function
- `identical(vect, vect2)` To check if items are identical
- `vect["bar"]` (but you can still use position index in named vectors as well)

## Lesson 8 - Matrices and DataFrames ##

In this lesson, we'll cover matrices and data frames. Both represent "rectangular"" data types, meaning that they are used to store tabular data, with rows and columns.

The main difference is that matrices can only contain a single class of data, while data frames can consist of many different classes of data. The difference is NOT in dataframe having named dimensions, as in R also matrix may have names across dimensions.

A matrix is simply an atomic vector with a dimension attribute. In other way, it is a vector plus the information on how this data is shaped across the dimensions.

- `length(my_vector)`
- `dim(my_vector)` returns `NULL`
- `dim(my_vector) <- c(4, 5)` Shapes my-vector as a 4x5 matrix
  - Now `my_vector` has a `dim` attribute and acts as a matrix
- `as.vector(m)` or `as.vector(t(m)` Does the opposite and flatten a matrix to a 1-D array
- `attributes(my_vector)` Return all the set attributes of an object. In this case that dim of my_vector is [4,5] 
- `class(my_vector)` Returns the type of object. `matrix` here
- Create a matrix: `m = matrix(data = c(1:20), nrow = 5, ncol = 4, byrow = FALSE, dimnames = list(list("a","b","c","d","e"),list("A","B","C","D")))`
- `cbind(patients, my_matrix)` Merge matrices (or vectors) horizontally (side by side)
- `df <- data.frame(patients, m)` Creates a dataframe from a list of vectors (and in such case take the name of the variable the data is stored in, in this case "patients", as column header) or matrices (and in such case takes the matrix column header as df column header, but if the names are integers it converts them in x1,x2...)
- `df <- read.csv("myfile.csv")` Import a csv to a df
- `colnames(my_data) <- cnames` Set DataFrame column names

## Lesson 9 - Looking at data ##

- `dim(df)` df rows and cols. Also available: `nrow(df)` and `ncol(df)`
- `object.size(obj)` Returns how much space the obj is occupying in memory
- `names(df)` Column names
- Select rows by name (vectors accepted):
  - `m["a",]` 
  - `df["a",]`
- Select column(s) (vectors accepted):
  - `m[,c("A","C")]` 
  - `df[,c("A","C")]`
  - also: `df$A`
- Select by position (both rows, cols):
  - `m[2:3,3:4]`
  - `df[2:3,3:4]`
- `head(df,nrows=6)` Select first 6 rows
- `tail(df,nrows=6)` Select last 6 rows
- `summary(df)`
- `str(df)` Structure (and preview) of a dataframe. Also for other kind of objects 
- `table(df$colname)` To get all number of values of a categorical variable

## Lesson 10 - Base Graphics ##

- `data(df)` "loads" some data
- `plot(df)`
- `plot(x=df$colx,y=df$coly,xlab="X Label",ylab="Y label", main="Title", sub="Subtitle", col=IntegerIdx, pch=IntegerIdx, xlim = c(MIN, MAX))` `col` and `pch` are the symbols' colour and shape respectivelly, and expect an integer as parameter, e.g. 2 is red colour
- `boxplot(formula, dfsource, and all the other arguments as plots)` 
- "formula" argument: an expression with a tilde ("~") which indicates the relationship between the input variables. E.g. `mpg ~ cyl` plots the relationship between cyl (number of cylinders) on the x-axis and mpg (miles per gallon) on the y-axis.
- `hist(vector)` Computes already the frequency

## Lesson 11 - Manipulating Data with dplyr ##

- dplyr: A consistent and concise grammar for manipulating tabular data. One of the packages of the tidyverse collection.
- Works with data frames, data tables, databases and multidimensional arrays.
- dplyr supplies five 'verbs' that cover most fundamental data manipulation tasks: `select()`, `filter()`, `arrange()`, `mutate()`, and `summarize()`.
- tidyverse is built around tidy data stored in "tibbles", which are enhanced data frames (e.g. with improved printing appareances)
- `my_tbldf <- tbl_df(mydf)` Converts a df to a tibble (but it is deprecate!: use instead `my_tbldf <- tibble::as_tibble(mydf)` )

### select() ###

Select columns

- `select(my_tbldf, col1, col2, ...)`
- `select(my_tbldf, colStart:colEnd)`
- `select(my_tbldf, -col(s)ToOmit)`
- `select(my_tbldf, contains(match, ignore.case=TRUE))` 

### filter() ###

Filter rows

 - AND logic: commas or `&`
 - OR logic: `|`
 - NOT: `!`
 - `filter(my_tbldf, col1=="something", col2<="somethingElse", grepl("matchToFind",col3))` (AND logic is applied).

### arrange() ###

Sort the rows based on value on certain column(s)

- `arrange(my_tbldf,sortCol1, sortCol2, desc(sortCol3))`  sortCol3 is sorted in descending order

### mutate() ###

Create new variable(s) based on the values of other variables.

- `mutate(my_tbldf, newCol = [some logic based on other columns' values], newCol2 = [here we can already use newCol])`

Other approaches to have a new variable depending on other variables:

- mydf$varC <- 0
- mydf$varC[(!is.na(mydf$varB)) & (mydf$varA == mydf$varB)] <- 1
- mydf$varC <- ifelse(mydf$varA == mydf$varC, 1, 0)


### summarize ###

Collapses the dataset to a single row.

- Returns a new table with the variable(s) we want.
- `summarize(my_tbldf, avg_col1 = mean(col1))`
- Useful in particular together with grouped data, where it can give the requested summarised values for each group. 
- Useful functions used inside it:
  - `mean(col1)` Returns the average
  - `n()` To count all items
  - `n_distinct(col1)` To count all distinct items
  - `sum()` To sum all items of a field

## Lesson 12 - Grouping and Chaining with dplyr ##

- `grouped_df <- group_by(my_tbldf, catVar1, catVar2)` Breaks up the dataset into groups of rows based on the values of one or more variables.
- It returns a new "grouped" tibble that looks the smae aside a "groups" header printed on top.
- Now any operation applied to the grouped data will take place on a per group basis.

- `quantile(df$col, probs = 0.99)` Returns the number that split the data in 1 % (above the value) and 99% (below the value)
- `View(df)` opens a spreadsheet-like view of the data

- Chaining (or "piping") allows to string together multiple function calls in a way that is compact and readable.
- It avoids saving intermediate results without having to embedd function calls within one another.
- With the chain operator `%>%` instead, the code to the right of %>% operates on the result from the code to the left of it.
- In practice, what is on the left becomes the first argument of the function call that is on the right.
- `my_tbldf %>% group_by(catVar) %>% summarize(count = n(),unique = n_distinct(ip_id),...)`

## Lesson 13 - Tidying Data with tidyr ##

- Main paper on tidying data concepts: http://vita.had.co.nz/papers/tidy-data.pdf
- "Standard" layout for data to be defined "tydy" and work seamlessly with other tidy data tools:
  1) Each variable forms a column
  2) Each observation forms a row
  3) Each type of observational unit forms a table

Example of non-tidyed ("messy") data:

1. Column headers are values, not variable names
2. Variables are stored in both rows and columns
3. A single observational unit is stored in multiple tables
4. Multiple types of observational units are stored in the same table
5. Multiple variables are stored in one column

Problems of solving messy data:

### Column headers are values, not variable names ###

From:
```
grade male  female
A     1     5
B     5     0
```
To:
```
grade sex     count
A     male    1
A     female  5
B     male    5
B     female  0
```

- `gather(my_tbldf, nameToGiveToNewKeyCol, nameToGiveToNewValueCol, namesOldColumnsFromWhichToTakeTheData)` Takes multiple columns and collapses into key-value pairs (the key is the header of the column where the value is. I.e. "From columns to rows")
- namesOldColumnsFromWhichToTakeTheData also accepts negative names to say "every columns except these"
- Code for this example: `gather(students, sex, count, male, female)` (or `gather(students, sex, count, -grade)`)

### Multiple variables stored in one column ###

From:
```
  grade male_1 female_1 male_2 female_2
1     A      3        4      3        4
2     B      6        4      3        5
```
To:
```
   grade    sex class count
1      A   male     1     3
2      B   male     1     6
3      A female     1     4
4      B female     1     4
5      A   male     2     3
6      B   male     2     3
7      A female     2     4
8      B female     2     5
```

- `separate(my_tbldf, colToSeparate, namesToGiveToSeparatedCols, separator)` Separates one column into multiple columns. separator defaults to any nonalphanumerical values.
- Code for this example:

```
students2 %>%
  gather( sex_class,count ,-grade ) %>%
  separate(sex_class , c("sex", "class"))
```

### Variables stored in both rows and columns ###

From:
```
    name    test class1 class2 class3 class4 class5
1  Sally midterm      A   <NA>      B   <NA>   <NA>
2  Sally   final      C   <NA>      C   <NA>   <NA>
3   Jeff midterm   <NA>      D   <NA>      A   <NA>
4   Jeff   final   <NA>      E   <NA>      C   <NA>
```
To:
```
    name class final midterm
1   Jeff     2     E       D
2   Jeff     4     C       A
3  Sally     1     C       A
4  Sally     3     C       B
```

- `spread(my_tbldf, colWhoseUniqueValuesWillBecomeCols, colStoringTheValue )` Spreads a key-value pair across multiple columns (i.e. "From rows to cols")
- Code for this example:

```
students3 %>%
  gather(class, grade, class1:class5, na.rm = TRUE) %>%
  spread(test, grade) %>%
  mutate(class=parse_number(class)) 
```

### Multiple observational units stored in the same table ###

From:
```
    id  name sex class midterm final
1  168 Brian   F     1       B     B
2  168 Brian   F     5       A     C
3  588 Sally   M     1       A     C
4  588 Sally   M     3       B     C
```
To:
```
   id  name sex
1 168 Brian   F
2 588 Sally   M
```
And:
```
    id class midterm final
1  168     1       B     B
2  168     5       A     C
3  588     1       A     C
4  588     3       B     C
```

- `unique(my_tbldf)` Reurns a tibble with removed duplicate rows (in all fields)

- Code for this example:

```
student_info <- students4 %>%
  select(id, name, sex) %>%
  unique 
gradebook <- students4 %>%
  select(id,class,midterm,final)
```
  
### Single observational unit stored in multiple tables ###

Opposite of the previous problem

From ("passed""):
```
   name class final
1 Brian     1     B
2 Roger     2     A
3 Roger     5     A
```
And ("failed"):
```
  name class final
1 Brian     5     C
```
To:
```
    name class final status
1  Brian     1     B passed
2  Roger     2     A passed
3  Roger     5     A passed
4  Brian     5     C failed
```

- `bind_rows(my_tbldf1,my_tbldf2,...)` Vertical stacking of tables (by column name)
- `bind_cols(my_tbldf1,my_tbldf2,...)` Horizontal stacking of tables (by position. Use `join` to use common values - like ids -instead.)
- Code for this example:

```
mutate(passed, status = "passed")
mutate(failed, status = "failed")
bind_rows(passed,failed)
```

### The SAT college-readiness exam ###

Database with exams aggregated resutls in 2012 by 3 exam sections (critical reading, mathematics, writing), sex, and six score ranges

From:
```
  score_range read_male read_fem read_total math_male math_fem math_total write_male write_fem write_total
1 700-800         40151    38898      79049     74461    46040     120501      31574     39101       70675
2 600-690        121950   126084     248034    162564   133954     296518     100963    125368      226331
3 500-590        227141   259553     486694    233141   257678     490819     202326    247239      449565
4 400-490        242554   296793     539347    204670   288696     493366     262623    302933      565556
5 300-390        113568   133473     247041     82468   131025     213493     146106    144381      290487
6 200-290         30728    29154      59882     18788    26562      45350      32500     24933       57433
```
To:
 ```
    score_range part  sex    count  total   prop
 1 700-800     read  male   40151 776092 0.0517
 2 600-690     read  male  121950 776092 0.157 
 3 500-590     read  male  227141 776092 0.293 
 4 400-490     read  male  242554 776092 0.313 
 5 300-390     read  male  113568 776092 0.146 
 6 200-290     read  male   30728 776092 0.0396
 7 700-800     read  fem    38898 883955 0.0440
 8 600-690     read  fem   126084 883955 0.143 
 9 500-590     read  fem   259553 883955 0.294 
10 400-490     read  fem   296793 883955 0.336 
etc.
 ```
 
- `contains(match, ignore.case = TRUE)` As a condition inside a `select` statement, filter to columns names containing the `match` string.
- Code for this example:
```
sat %>%
  select(-contains("total")) %>%
  gather(part_sex, count, -score_range) %>%
  separate(part_sex, c("part", "sex")) %>%
  group_by(part,sex) %>%
  mutate(total = sum(count),
         prop = count/total
  )
```

- Note that the last group_by doesn't really shrink the db, it is needed only to compute the total (that remains constant in each item of the group), in turn used to compute the prob field.


## OLS with R ##

- `single <- lm(dep_var ~ a_var + other_var, mydf)`  This performs the OLS
- `fit1 <- lm(weeksm ~ threeKids+blackm+hispm+othracem , data=subset(mydf,workedm==1))` Here we are running the OLS on a subset of data
- `summary(single)`  This prints the OLS results in a nice format
- `coefficients(single)` This returns the coefficients
- `ci_single <- confint(single, level=0.9)` This returns the coefficient intervals


