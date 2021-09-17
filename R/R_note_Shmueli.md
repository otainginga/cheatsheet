# read data



```R
housing.df <- read.csv("R_data/WestRoxbury.csv", header = TRUE)  # load data

dim(housing.df)  # find the dimension of data frame
head(housing.df)  # show the first six rows
View(housing.df)  # show all the data in a new tab

names(housing.df)  # print a list of variables to the screen.
t(t(names(housing.df)))  # print the list in a useful column format

class(housing.df[, 1]) # the class of 1st column is ?

```



## Data type

```R

vec_updated <- as.factor(vec)  # a simple vector

housing.df$REMODEL = as.factor(housing.df$REMODEL)  # a simple column

data3 <- as.data.frame(unclass(data3),                     # Convert all columns (in dataframe)to factor
                       stringsAsFactors = TRUE)
```



# Data Preparation

```R
# use set.seed() to get the same partitions when re-running the R code.
set.seed(1)
```



## Subset Selection

### loc method

```R
# Practice showing different subsets of the data
housing.df[1:10, 1]  # show the first 10 rows of the first column only
housing.df[1:10, ]  # show the first 10 rows of each of the columns 
housing.df[5, 1:10]  # show the fifth row of the first 10 columns
housing.df[5, c(1:2, 4, 8:10)]  # show the fifth row of some columns
housing.df[, 1]  # show the whole first column
housing.df$TOTAL.VALUE  # a different way to show the whole first column
housing.df$TOTAL.VALUE[1:10]  # show the first 10 rows of the first column
length(housing.df$TOTAL.VALUE)  # find the length of the first column
mean(housing.df$TOTAL.VALUE)  # find the mean of the first column
summary(housing.df)  # find summary statistics for each column
```



### random sample in %

including training set and validation set 

```R
# random sample of 5 observations
s <- sample(row.names(housing.df), 5)
housing.df[s,]

# oversample houses with over 10 rooms
s <- sample(row.names(housing.df), 5, prob = ifelse(housing.df$ROOMS>10, 0.9, 0.01))
housing.df[s,]



# use set.seed() to get the same partitions when re-running the R code.
set.seed(1)

## partitioning into training (60%) and validation (40%)
train.rows <- sample(rownames(housing.df), dim(housing.df)[1]*0.6)
train.data <- housing.df[train.rows, ]
# assign row IDs that are not already in the training set, into validation 
valid.rows <- setdiff(rownames(housing.df), train.rows) 
valid.data <- housing.df[valid.rows, ]

## partitioning into training (50%), validation (30%), test (20%)
# randomly sample 50% of the row IDs for training
train.rows <- sample(rownames(housing.df), dim(housing.df)[1]*0.5)

# sample 30% of the row IDs into the validation set, drawing only from records
# not already in the training set
# use setdiff() to find records not already in the training set
valid.rows <- sample(setdiff(rownames(housing.df), train.rows), 
                     dim(housing.df)[1]*0.3)

# assign the remaining 20% row IDs serve as test
test.rows <- setdiff(rownames(housing.df), union(train.rows, valid.rows))

# create the 3 data frames by collecting all columns from the appropriate rows 
train.data <- housing.df[train.rows, ]
valid.data <- housing.df[valid.rows, ]
test.data <- housing.df[test.rows, ]

```



## Dummy

change all factor levels to extra columns

```
install.packages('dummies')
library(dummies)
housing.df <- dummy.data.frame(housing.df, sep = ".")
names(housing.df)
```



## NaNs

```R
# create NaNs
rows.to.missing <- sample(row.names(housing.df), 10)
housing.df[rows.to.missing,]$BEDROOMS <- NA
summary(housing.df$BEDROOMS)  # Now we have 10 NA's and the median of the 
# remaining values is 3.


# Fill NaNs
housing.df[rows.to.missing,]$BEDROOMS <- median(housing.df$BEDROOMS, na.rm = TRUE)
```

