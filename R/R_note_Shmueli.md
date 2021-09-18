# Summary

![datamining progress in R .png](https://github.com/otainginga/Image/blob/main/datamining%20progress%20in%20R%20.png?raw=true)



# 00. basic Idea for dealing with data

## 00.01 read data



```R
housing.df <- read.csv("R_data/WestRoxbury.csv", header = TRUE)  # load data

dim(housing.df)  # find the dimension of data frame
head(housing.df)  # show the first six rows
View(housing.df)  # show all the data in a new tab

names(housing.df)  # print a list of variables to the screen.
t(t(names(housing.df)))  # print the list in a useful column format

class(housing.df[, 1]) # the class of 1st column is ?

```



### Data type

```R

vec_updated <- as.factor(vec)  # a simple vector

housing.df$REMODEL = as.factor(housing.df$REMODEL)  # a simple column

data3 <- as.data.frame(unclass(data3),                     # Convert all columns (in dataframe)to factor
                       stringsAsFactors = TRUE)
```



## 00.02 Data Preparation

```R
# use set.seed() to get the same partitions when re-running the R code.
set.seed(1)
```



### |Variable Selection

#### loc method

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

#### New dataframe &delete

```R
tr.res <- data.frame(train.data$TOTAL_VALUE, reg$fitted.values, reg$residuals)
head(tr.res)

A <- data.frame('name' = c("AA", "BB", "CC"), 'weight' = c(50, 70, 80), 'seesight' = c(5.0, 4.8, 1.2))
# delete the first row
A <- A[-1,]
# delete the second column
A <- A[,-2]
```



#### random sample in %

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



### |Dummy

change all factor levels to extra columns

```
install.packages('dummies')
library(dummies)
housing.df <- dummy.data.frame(housing.df, sep = ".")
names(housing.df)
```

### | Bins

```R
boston.housing.df$RM.bin <- .bincode(boston.housing.df$RM, c(1:9))
```



### |NaNs

```R
# create NaNs
rows.to.missing <- sample(row.names(housing.df), 10)
housing.df[rows.to.missing,]$BEDROOMS <- NA
summary(housing.df$BEDROOMS)  # Now we have 10 NA's and the median of the 
# remaining values is 3.


# Fill NaNs with median
housing.df[rows.to.missing,]$BEDROOMS <- median(housing.df$BEDROOMS, na.rm = TRUE)

# find the number of missing values of variable CRIM
sum(is.na(boston.housing.df$CRIM)) 
```



### |Normalizing and Rescaling 





## 00.03 EDA

### |stats

```R
#### Table 4.3

boston.housing.df <- read.csv("./R_data/BostonHousing.csv", header = TRUE) 
head(boston.housing.df, 9)
summary(boston.housing.df) 

# compute mean, standard dev., min, max, median, length, and missing values of CRIM #######################
mean(boston.housing.df$CRIM) 
sd(boston.housing.df$CRIM)
min(boston.housing.df$CRIM)
max(boston.housing.df$CRIM)
median(boston.housing.df$CRIM) 
length(boston.housing.df$CRIM) 

# find the number of missing values of variable CRIM
sum(is.na(boston.housing.df$CRIM)) 

# compute mean, standard dev., min, max, median, length, and missing values for all #########################
# variables
data.frame(mean=sapply(boston.housing.df, mean), 
                         sd=sapply(boston.housing.df, sd), 
                         min=sapply(boston.housing.df, min), 
                         max=sapply(boston.housing.df, max), 
                         median=sapply(boston.housing.df, median), 
                         length=sapply(boston.housing.df, length),
                         miss.val=sapply(boston.housing.df, function(x) 
                         sum(length(which(is.na(x))))))

                                         
# corrlation map ####################################################################################
round(cor(boston.housing.df),2)
# table for categorys 
table(boston.housing.df$CHAS)
                                         
# in the shape of a table #########################################################################
# compute the average of MEDV by (binned) RM and CHAS
# in aggregate() use the argument by= to define the list of aggregating variables, 
# and FUN= as an aggregating function.
aggregate(boston.housing.df$MEDV, by=list(RM=boston.housing.df$RM.bin, 
                                          CHAS=boston.housing.df$CHAS), FUN=mean) 
   RM CHAS        x
1   3    0 25.30000
2   4    0 15.40714
3   5    0 17.20000
4   6    0 21.76917
5   7    0 35.96444
6   8    0 45.70000
7   5    1 22.21818
8   6    1 25.91875
9   7    1 44.06667
10  8    1 35.95000
                                         
                                         
                                         
```



### |library(reshape?2?)



**data sample(original)**: 宽型数据格式，经过melt 的重构将成为长格式<span style="background-color: yellow;">[对于每个ID1 和ID2的组合行，若original data有n measurements, 则 长格式的数据有 4*n rows]</span>



| ID1(ID  or bin_value) | ID2  | MeasurementA (value) | MeasurementB (value) | MeasurementC (value) |
| --------------------- | ---- | -------------------- | -------------------- | -------------------- |
| 1                     | A    | 0.951371             | 0.995423             | 0.832185             |
| 2                     | B    | 0.87416              | 0.530091             | 0.823064             |
| 3                     | A    | 0.632189             | 0.436937             | 0.970236             |
| 4                     | B    | 0.113804             | 0.504563             | 0.248914             |

**长格式数据**

| ID1(ID or  bin_value) | ID2  | variable(default) | value(default) |
| --------------------- | ---- | ----------------- | -------------- |
| 1                     | A    | MeasurementA      |                |
| 2                     | B    | MeasurementA      |                |
| 3                     | A    | MeasurementA      |                |
| 4                     | B    | MeasurementA      |                |
| 1                     | A    | MeasurementB      |                |
| 2                     | B    | MeasurementB      |                |
| 3                     | A    | MeasurementB      |                |
| 4                     | B    | MeasurementB      |                |
|                       |      | ...               |                |





```R
#######################

# use install.packages("reshape") the first time the package is used
library(reshape) 
boston.housing.df <- read.csv("./R_data/BostonHousing.csv")

# create bins of size 1
boston.housing.df$RM.bin <- .bincode(boston.housing.df$RM, c(1:9)) 


# use melt() to stack a set of columns into a single column of data.
# stack MEDV values for each combination of (binned) RM and CHAS
mlt <- melt(boston.housing.df, id=c("RM.bin", "CHAS"), measure=c("MEDV"))

> head(mlt, 5)
    RM.bin CHAS variable value
1        6    0     MEDV  24.0
2        6    0     MEDV  21.6
3        7    0     MEDV  34.7
4        6    0     MEDV  33.4
5        7    0     MEDV  36.2

# use cast() to reshape data and generate pivot table
cast(mlt, RM.bin ~ CHAS, subset=variable=="MEDV", 
     margins=c("grand_row", "grand_col"), mean)

  RM.bin        0        1    (all)
1      3 25.30000      NaN 25.30000
2      4 15.40714      NaN 15.40714
3      5 17.20000 22.21818 17.55159
4      6 21.76917 25.91875 22.01599
5      7 35.96444 44.06667 36.91765
6      8 45.70000 35.95000 44.20000
7  (all) 22.09384 28.44000 22.53281
  
```





cast(data, **formula = ... ~ variable**, fun.aggregate=NULL, ...,
  margins=FALSE, subset=TRUE, df=FALSE, fill=NULL, add.missing=FALSE,
  value = guess_value(data))
Arguments

- data	:molten data frame, see melt
- formula	:casting formula, see details for specifics
- fun.aggregate	:aggregation function
- add.missing	:fill in missing combinations?
- value	:name of value column
- ...	:further arguments are passed to aggregating function
- margins	:vector of variable names **(can include "grand\_col" and "grand\_row")** to compute margins for, or TRUE computer all margins
- subset	:logical vector to subset data set with before reshaping
- df	:argument used internally
- fill	:value with which to fill in structural missings, defaults to value from applying fun.aggregate to 0 length vector

#### in reshape2

[R语言学习 第十三篇：利用reshape2包重塑数据 - 爱码网 (likecs.com)](https://www.likecs.com/show-25712.html)

melt(data,id.vars,measure.vars,variable.name='variable',...,na.rm=FALSE,value.name='value',factorAsStrings=TRUE)
参数注释：

data：融合的数据框
id.vars：由标识变量构成的向量，用于标识观测的变量
measure.vars ：由观测变量构成的向量
variable.name：用于保存原始变量名的变量的名称
value.name：用于保存原始值的名称

dcast(data, formula, fun.aggregate = NULL, ..., margins = NULL,  subset = NULL, fill = NULL, drop = TRUE,
  value.var = guess_value(data))

参数注释：

data：已融合的数据框
formula：用于指定输出的结果集格式
fun.aggregate：用于指定聚合函数，对已聚合的数据执行聚合运算
margins：相当于透视表中的行总计和列总计
subset：选取满足一些特定值的数据，相当于Excel透视表的筛选。例如， subset =.（variable ==“length”)

## 00.04 Fitting

```R
# fitting peogress, 
# remove variable "TAX"
reg <- lm(TOTAL_VALUE ~ .-TAX, data = housing.df, subset = train.rows) 

tr.res <- data.frame(train.data$TOTAL_VALUE, reg$fitted.values, reg$residuals)
head(tr.res)


pred <- predict(reg, newdata = valid.data)
vl.res <- data.frame(valid.data$TOTAL_VALUE, pred, residuals = 
                       valid.data$TOTAL_VALUE - pred)
head(vl.res)

```





## 00.05 Evaluating

```R
library(forecast)
# compute accuracy on training set
accuracy(reg$fitted.values, train.data$TOTAL_VALUE)

# compute accuracy on prediction set
pred <- predict(reg, newdata = valid.data)
accuracy(pred, valid.data$TOTAL_VALUE)
```





## 00.06 Visualization

### |ggplot2

<span style="background-color: yellow;">with ggplot</span>

```R
install.packages(c("tidyverse", "colorspace", "corrr",  "cowplot",
                   "ggdark", "ggforce", "ggrepel", "ggridges", "ggsci",
                   "ggtext", "ggthemes", "grid", "gridExtra", "patchwork",
                   "rcartocolor", "scico", "showtext", "shiny",
                   "plotly", "highcharter", "echarts4r"))
# install from GitHub since not on CRAN
install.packages('devtools')
devtools::install_github("JohnCoene/charter")
library(ggplot2)
library(tidyverse)

```

https://zhuanlan.zhihu.com/p/370223674

https://zhuanlan.zhihu.com/p/391832351

https://www.cedricscherer.com/2019/08/05/a-ggplot2-tutorial-for-beautiful-plotting-in-r/



| Issue        | Code      | Note                                                         |
| ------------ | --------- | ------------------------------------------------------------ |
| **数据**     |           | **作图用的原始数据**                                         |
| **几何图形** | **geom_** | **表示数据的几何形状**                                       |
| **美学**     | **aes()** | **几何或者统计对象的美学，比如位置，颜色，大小，形状等**     |
| 刻度         | scale_()  | 数据与美学维度之间的映射，比如图形宽度的数据范围，           |
| 统计转换     | stat_     | 数据的统计，比如百分位，拟合曲线或者和                       |
| 坐标系统     | coord_    | 数据的转换                                                   |
| 面           | facet_    | 数据图表的排列                                               |
| 主题         | theme()   | 图形的整体视觉默认值，如背景、网格、轴、默认字体、大小和颜色 |





codes( link with "+")

| ggplot(chic,  )                                   | geom_point() | geom_line()  | them_bw() | labs()                 | ylim()   | theme()                                                      | expand_limits()          |
| ------------------------------------------------- | ------------ | ------------ | --------- | ---------------------- | -------- | ------------------------------------------------------------ | ------------------------ |
| chic,                                             | color="",    | color= "",   |           | x="",                  | c(0, 50) | axis.title.x=element_text(vjust=0,size=15),                  | x=0,<br /> y=0           |
| aes(x=d,<br />y=t,<br />color = c<br />shape = e) | shape="",    | linetype="", |           | y="(°F)"               |          | axis.title.x=element_text(margin=margin(t=10),size=15),      | **使图表从坐标原点开始** |
|                                                   | size=2,      | size=.3      |           | title=""               |          | margin()对象中的标签t和r分别指代top和right。<br />你还可以将这四个边距指定为margin(t,  r, b, l)。 |                          |
|                                                   | aplha = 0.5, |              |           | labs(x = NULL, y = "") |          | axis.title = element_text(size = 15, color = "",  face = "bold.italic") |                          |
|                                                   |              |              |           |                        |          | italic , bold                                                |                          |
|                                                   |              |              |           |                        |          | axis.text = element_text(color = "dodgerblue", size = 12),   |                          |
|                                                   |              |              |           |                        |          | legend.position = "none"/top/                                |                          |
|                                                   |              |              |           |                        |          | panel.grid.major.x = element_line(color = "red1")            |                          |
|                                                   |              |              |           |                        |          |                                                              |                          |







PCA

