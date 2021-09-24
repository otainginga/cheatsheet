# 00 Power BI Page 0

---

The in-depth understanding of POWER BI usage. Continuous Update



# 01 Specialized Vocabulary

---

**Model** : Build relationships between tables

**PIVOT** ：converts data from row level to column level. /**UNPIVOT**

![img](https://raw.githubusercontent.com/otainginga/Image/main/powerbi_unpivot.png)



## 01.01<span style="background-color:Tomato;">**Power Query**: For getting data.</span>

![img](https://raw.githubusercontent.com/otainginga/Image/main/powerbi_powerquery.png)

![powerbi_merge and append.png](https://github.com/otainginga/Image/blob/main/powerbi_merge%20and%20append.png?raw=true)

## 01.02 <span style="background-color:Tomato;">M Function</span>

1. M函数对大小写敏感，每一个字母必须按函数规范书写，第一个字母都是大写

2. 表被称为Table，每行的内容是一个Record，每列的内容是一个List

3. 行标用大括号{ }，比如取第一行的内容：=表{0} //PQ的第一行从0开始

4. 列标用中括号[ ]，比如取自定义列的内容：=表[自定义]

5. 取第一行自定义列的内容：=表{0}[自定义]

6. 聚合函数：

   > 求和：List.Sum()
   > 求最小值：List.Min()
   > 求最大值：List.Max()
   > 求平均值：List.Average()

   文本函数：

   > 求文本长度：Text.Length()
   > 去文本空格：Text.Trim()
   > 取前n个字符：Text.Start(文本,n)
   > 取后n个字符：Text.End(文本,n)

   提取数据函数：

   > 从Excel表中提取数据：Excel.Workbook()
   > 从Csv/Txt中提取数据：Csv.Document()

   条件函数：

   > if else then （相当于Excel中的IF)

7. 

# 02 Data cleaning using Power Query Editor





## 02.01 Transform

![powerbi_transform.png](https://github.com/otainginga/Image/blob/main/powerbi_transform.png?raw=true)

### 02.01 Lifting the Title

Transform &rarr; Use first row as headers

### 02.02 Change Data Types

Transform&rarr;  Datatype



### 02.03  Missing Values

### 02.04 Duplicated data



column selection &rarr;  Datatype

### 02.05 Fill in 

Transform&rarr;  Any column &rarr; Fill &rarr;  downward

### 02.06 Merge Columns & Split columns



Transform&rarr;  Text column &rarr; Merge Columns &rarr;  downward

1. <华东><杭州> to <华东: 杭州>
2. <华东: 杭州> to <华东><杭州> 



### 02.07 Grouping

Transform&rarr;  Table &rarr; Group by

### 02.08 Extract tests

Transform&rarr;  Text column &rarr; Extract

### 02.09 Transpose

Transform&rarr;  Text column &rarr; transpose

直接转置时作为column name 的月份不见了，这是因为转置的时候，只转数据的部分，月份并不在数据区，我们要想保留月份，先要把月份降下来，这里用到我们前面介绍的”将标题作为第一行，标题下降以后，再进行转置就可以了



02.10 columns & rows



---

# 03 Modeling

![powerbi_modeling.png](https://github.com/otainginga/Image/blob/main/powerbi_modeling.png?raw=true)

Measure & DAX





# 99 Supplementary

---



[From SIWU](https://zhuanlan.zhihu.com/p/64999937)

日前进度： 7
