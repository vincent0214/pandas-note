## Pandas是什么
> Pandas 库是一个免费、开源的第三方 Python 库，是 Python 数据分析必不可少的工具之一，它为 Python 数据分析提供了高性能，且易于使用的数据结构，即 Series 和 DataFrame。Pandas 自诞生后被应用于众多的领域，比如金融、统计学、社会科学、建筑工程等。
>
> Pandas 库基于 Python NumPy 库开发而来，因此，它可以与 Python 的科学计算库配合使用。Pandas 提供了两种数据结构，分别是 Series（一维数组结构）与 DataFrame（二维数组结构），这两种数据结构极大地增强的了 Pandas 的数据分析能力。在本套教程中，我们将学习 Python Pandas 的各种方法、特性以及如何在实践中运用它们。
>
> 出自: http://c.biancheng.net/pandas/


## 安装Pandas
```bash
pip install xlwt
pip install pandas
```

## 读取文件

### 读取Excel

#### 常规读取

```python
import pandas as pd

table = pd.read_excel(path)
```

#### 读取无表头的excel

读取无表头的excel表格

```python
import pandas as pd

table = pd.read_excel(path, header=None) 
```

####  错位读取

```python
import pandas as pd

table = pd.read_excel(path, header=1) #由第二行开始读取
```

### 读取csv

去除csv空格(去空格读取)

```python
import pandas as pd

table = pd.read_csv(path, sep="\s*,\s*")  # sep="\s*,\s*"去除csv空格
```



## 保存文件
### 保存excel文件
```python
import pandas as pd

table1 = pd.read_excel("./student1.xlsx")
table1.to_excel("./table.xlsx", index=False, sheet_name="sheet1") 
```

- index=False不写`index`列

### 同时写多个sheet
```python
import pandas as pd

table1 = pd.read_excel("./student1.xlsx")
table2 = pd.read_excel("./student2.xlsx")
table3 = pd.read_excel("./student3.xlsx")

writer = pd.ExcelWriter(r"./temp/" + filename)
table1.to_excel(writer, index=False, sheet_name="sheet1") 
table2.to_excel(writer, index=False, sheet_name="sheet2")
table3.to_excel(writer, index=False, sheet_name="sheet3")
writer.save()
writer.close()
```

## 查询数据
### 方法1
```python
import pandas as pd

table = pd.read_excel(path)
new_table = table.loc[table["列名"].apply(lambda x: x == "数据值")]   
```

### 方法2
```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
new_table = table.loc[table["语文"] < 60]  # 返回语文少于60分的数据
```

### 多条件查询
```python
file = './excel/books.xlsx'
books = pd.read_excel(file, index_col="id")
r = books.loc[books['价格'].apply(lambda x: x < 200)] \ 
    .loc[books['价格'].apply(lambda x: x > 100)]
print(r)
```

## 行操作
### 追加其他表数据

向下合并其他表的数据

```python
import pandas as pd

table1 = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
table2  = pd.read_excel("./student.xlsx", sheet_name="Sheet3")
table3 = table1.append(table2).reset_index(drop=True) # 合并行,重新设置index
```

动态合并多个表格(工具函数)
```python
import pandas as pd

class PandasUtil:
    @staticmethod
    def merge_tables(tables):
        """
        合并表格(相同格式,上下合并)
        """
        result = None
        for table in tables:
            if result is None:
                result = table
                continue
            result = result.append(table, ignore_index=True)
        result.reset_index(drop=True)
        return result
```

### 新建新行

添加新行,{"id":7, "name":"ken", "语文":100, "数学":90, "英语":70}
```python
import pandas as pd

students = pd.read_excel("./student.xlsx")
stu = pd.Series({"id":7, "name":"ken", "语文":100, "数学":90, "英语":70})  # 新行数据
students =students.append(stu, ignore_index=True ) # 追加新行
print(students)
```

### 修改单元格数据

```python
import pandas as pd

students = pd.read_excel("./student.xlsx")
students.at[0,'name'] = "kelly" # 修改Ben1名称,改为"kelly"
print(students)
```


### 修改一行数据


修改第一行数据为: {"id":100, "name":"Vincent", "语文":100}
```python
import pandas as pd

students = pd.read_excel("./student.xlsx")
students.iloc[0] = pd.Series({"id":100, "name":"Vincent", "语文":100})

print(students)
```


### 插入一行数据

```python
import pandas as pd

students = pd.read_excel("./student.xlsx")
new_student = pd.Series({"id":100, "name":"Boy1", "语文":100})  ### 插入的数据
part1 =  students[:3]  # 第1到2行,
part2 = students[3:] # 第3行到结尾行
students = part1 \
				.append(new_student, ignore_index=True) \
				.append(part2).reset_index(drop=True)
print(students)
```

### 按条件删除数据

按条件删除行数据

#### 单条件删除

```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
remove_index = table.loc[table["列明"] == "数据值"].index
table.drop(index=remove_index, inplace=True)  # 按条件删除数据

```

#### 多条件删除

```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
remove_index = table[(table["收/支"] == "支出") & (table["交易状态"] == "交易关闭")].index
table.drop(index=remove_index, inplace=True)  # 按条件删除数据
```

#### 删除空行
```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
table.dropna(subset=["交易时间"], inplace=True)  # 删除交易时间为空的数据
```


### 排序


#### 单值排序

按价格倒序排序

ascending=False 为倒序排序

```python
import pandas as pd

file = './excel/books.xlsx'
books = pd.read_excel(file)
books.sort_values(by="价格", inplace=True, ascending=False)  # 排序
print(books)

```

![image-20211219124701631](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153526.png)

#### 多值排序

![image-20211219125152086](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153528.png)

先按`是否值得`排序,再按`价格`排序

```python
file = './excel/books.xlsx'
books = pd.read_excel(file, index_col="id")
books.sort_values(by=["是否值得", "价格"], inplace=True, ascending=[False, True])  # 排序
print(books)

```

![image-20211219125513520](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153530.png)







### 去重, 查找重复数据

[原文链接](https://gitee.com/vincent0214/pandas-notes/blob/master/21-%E6%B6%88%E9%99%A4%E9%87%8D%E5%A4%8D%E6%95%B0%E6%8D%AE__%E5%AE%9A%E4%BD%8D%E6%95%B0%E6%8D%AE.md)


#### 去掉重复数据

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221123950.png)

```python
import pandas as pd

students = pd.read_excel("./student.xlsx")
students.drop_duplicates(subset=["name"], inplace=True) # 删除重复数据
print(students)

```

- subset=["name"]  查询条件,可以是多个列例如 subset=["name", "语文", "数学", "英语"]

- keep="first"   默认保留开头

  

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221123956.png)

#### 查找重复项

- 检查所有行是否重复

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221130755.png)

```python
import pandas as pd

students = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
dupe = students.duplicated(subset=["name"])
print(dupe)
```

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221183505.png)



- 查询重复行

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221183507.png)

```python
import pandas as pd

students = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
dupe = students.duplicated(subset=["name"])  
print(dupe[dupe==True])
```

显示7-11行是重复数据

序列是由0开始数的,index为6表示excel表中的第7行数据

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221183509.png)



用`iloc[index]`定位重复数据

```python
import pandas as pd

students = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
dupe = students.duplicated(subset=["name"])  
dupe = dupe[dupe==True]
print(students.iloc[dupe.index])

```

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221183513.png)

### 定位数据

使用`iloc`

```pyton
import pandas as pd

table = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
r = table.iloc[0]   # 获取第1行数据
print(r)
```

- df.iloc[0] 第0行数据（series）

- df.iloc[[0]] 第0行数据（dataframe）

- df.iloc[[0, 1]] 第0、1行数据（dataframe）

- df.iloc[:3] 前三行数据（dataframe）

- df.iloc[[True, False, True]] 第0、2行数据（dataframe）

  

使用`loc`
要设置DataFream的index_col才能使用

```python
df = pd.read_excel("./student.xlsx",sheet_name="Sheet2",index_col="name") # 需要设置index_col
r = df.loc["Dean"]
print(r)
```

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221183521.png)

![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221183524.png)

### 遍历数据
遍历每一行数据
```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
for index, row in df.iterrows():
    print(index)
    print(row["c1"])
```



## 列操作

### 增加列
追加列

```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
table["数学"] = 100
```

### 判断列是否存在
```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
print("数学" in table.columns)
```

### 删除列
```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
table.drop(columns=['语文'], inplace=True) # 删除列
```

### 插入列

插入到第二列
```python
import pandas as pd

students = pd.read_excel("./student.xlsx")
students.insert(1, column="年龄", value=100) # 插入列
print(students)
```


### 调整列位置
调整`收/支`列到第一列
```python
def change_col_place(table, name, new_place, new_name=None):
     """
     移动列
     table: 表格
     name: 原列名
     new_plack: 新位置
     new_name: 新列名
     """
     val = table[name]
     table.drop(labels=[name], axis=1, inplace=True)
     if new_name is None:
         table.insert(new_place, column=name, value=val)  # 插入列
     else:
         table.insert(new_place, column=new_name, value=val)  # 插入列

table = pd.read_excel("./student.xlsx")
change_col_place(table, "收/支", 0, "收/支")  
```

### 修改列名

```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
table.rename(columns={"旧列名1":"新列名1", "旧列名2":"新列名"}, inplace=True) # 修改列名 
```


### 批量修改列数据

修改列数据

案例: 把金额列的`¥`去掉

```python
import pandas as pd

table = pd.read_excel("./student.xlsx")
table["金额"] =  table["金额"].apply(lambda x: x.replace("¥", ""))
```


### 拆分列

```python
import pandas as pd

books = pd.read_excel("./books.xlsx",sheet_name="Sheet2")
df = books['name'].str.split(expand=True) # 拆分name列
books["first name"] = df[0]  # 增加列 first name
books["last name"] = df[1]   # 增加列 last name
print(books)
```
![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221100940.png)
![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221100949.png)

拆分列api原文
![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221100954.png)
- pat: 字符串或正则表达式以拆分。如果未指定，则在空格上拆分。
- n: 拆分后保留多少个元素. -1表示保留全部
- expand: 将拆分字符串展开到单独的列中 expand一般设置为True

### 合并列
```python
import pandas as pd

student = pd.read_excel("./student.xlsx",sheet_name="Sheet1")
student["name"] = student["first name"] +" "+ student["first name"]
print(studentr) 
```
![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221102441.png)


## 求总和和求平均值

### 按行求和

```
pandas.DataFrame.sum
```

> 原文地址: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.sum.html#pandas-dataframe-sum

![image-20211221120420668](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221123913.png)



![](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121552.png)
```python
import pandas as pd

student = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
row_sum = student[["语文","数学","英语"]].sum(axis=1) # axis=1按行计算.
print(type(student[["语文","数学","英语"]])) # 输出: Dataframe
print(row_sum) 
```

![image-20211221115936128](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121556.png)



###  按行求平均值

![image-20211221120816906](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121558.png)

```python
import pandas as pd

student = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
row_avg = student[["语文","数学","英语"]].mean(axis=1) # axis=1按行计算.
print(row_avg) 
```

![image-20211221120834124](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121559.png)



### 计算列总和

![image-20211221121040193](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121602.png)

```python
import pandas as pd

student = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
col_sum = student[["语文","数学","英语"]].sum(axis=0) # axis=1按行计算.
print(col_sum) 
```

![image-20211221121158046](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121603.png)

### 计算列平均

![image-20211221121309020](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121605.png)

```python
import pandas as pd

student = pd.read_excel("./student.xlsx",sheet_name="Sheet2")
col_avg = student[["语文","数学","英语"]].mean(axis=0) # axis=1按行计算.
print(col_avg) 
```

![image-20211221121439004](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211221121606.png)

## 多表联合（从VLOOKUP到JOIN）
https://gitee.com/vincent0214/pandas-notes/blob/master/17-%E5%A4%9A%E8%A1%A8%E8%81%94%E5%90%88%EF%BC%88%E4%BB%8EVLOOKUP%E5%88%B0JOIN%EF%BC%89.md


## 旋转数据表(行列转换)
https://gitee.com/vincent0214/pandas-notes/blob/master/17-%E5%A4%9A%E8%A1%A8%E8%81%94%E5%90%88%EF%BC%88%E4%BB%8EVLOOKUP%E5%88%B0JOIN%EF%BC%89.md


## 常用的文件操作函数

文件工具类

```python

import os
import shutil


class FileUtil:
    @staticmethod
    def scan_file(path):
        """
        递归扫描文件夹下的所有文件
        """
        files = []

        def _scan_file(path):
            for file_name in os.listdir(path):
                file_path = path + "/" + file_name
                if os.path.isdir(file_path):   
                    _scan_file(file_path)
                else:
                    file = File(file_path)
                    files.append(file)

        _scan_file(path)
        return files

    @staticmethod
    def clean_dir(path):
        """
        清空文件夹
        """
        if os.path.exists(path):
            shutil.rmtree(path)   # 删除目录
        os.mkdir(path)  # 创建目录


class File:
    def __init__(self, path):
        self.name = os.path.basename(path)
        self.path = path
```

## 其他网站

- http://c.biancheng.net/pandas/what-is-pandas.html
- https://www.runoob.com/pandas/pandas-tutorial.html