
## 安装pandas
```
pip install xlwt
pip install pandas
```

## 读取文件

### 读取Excel

- 常规读取
```python
import pandas as pd

table = pd.read_excel(path)
```

- 读取无标题的excel
```python
import pandas as pd

table = pd.read_excel(path, header=None) # header=No
```

这是无标题的excel表格

![](E:\code\my_code\excel_code\bill_record_reader\Pandas常用API.assets\20211218223652.png)

- 错位读取

```python
import pandas as pd

table = pd.read_excel(path, header=1) #由第二行开始读取
```

![](E:\code\my_code\excel_code\bill_record_reader\Pandas常用API.assets\20211218223648.png)

### 读取csv

去除csv空格

```python
import pandas as pd

table = pd.read_csv(path, sep="\s*,\s*")  # sep="\s*,\s*"去除csv空格
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

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
new_table = table.loc[table["语文"] < 60]  # 返回语文少于60分的数据
```


## 行操作
### 追加其他表数据

向下合并其他表的数据)

```python
import pandas as pd

table1 = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
table2  = pd.read_excel("./student.xlsx", sheet_name="Sheet3")
table3 = table1.append(table2).reset_index(drop=True) # 合并行,重新设置index
```

### 新建新行

添加新行,{"id":7, "name":"ken", "语文":100, "数学":90, "英语":70}
```python
import pandas as pd

students = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
stu = pd.Series({"id":7, "name":"ken", "语文":100, "数学":90, "英语":70})  # 新行数据
students =students.append(stu, ignore_index=True ) # 追加新行
print(students)
```

### 修改单元格数据

```python
import pandas as pd

students = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
students.at[0,'name'] = "kelly" # 修改Ben1名称,改为"kelly"
print(students)
```


### 修改一行数据


修改第一行数据为: {"id":100, "name":"Vincent", "语文":100}
```python
import pandas as pd

students = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
students.iloc[0] = pd.Series({"id":100, "name":"Vincent", "语文":100})

print(students)
```


### 插入一行数据

```python
import pandas as pd

students = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
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

单条件删除
```python
import pandas as pd

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
remove_index = table.loc[table["列明"] == "数据值"].index
table.drop(index=remove_index, inplace=True)  # 按条件删除数据

```

多条件删除
```python
import pandas as pd

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
remove_index = table[(table["收/支"] == "支出") & (table["交易状态"] == "交易关闭")].index
table.drop(index=remove_index, inplace=True)  # 按条件删除数据
```



### 排序

https://gitee.com/vincent0214/pandas-notes/blob/master/07-%E6%8E%92%E5%BA%8F.md



### 去重, 查找重复数据

https://gitee.com/vincent0214/pandas-notes/blob/master/21-%E6%B6%88%E9%99%A4%E9%87%8D%E5%A4%8D%E6%95%B0%E6%8D%AE__%E5%AE%9A%E4%BD%8D%E6%95%B0%E6%8D%AE.md



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





















## 列操作

### 追加列
添加列

```python
import pandas as pd

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
table["数学"] = 100
```


### 删除列
```python
import pandas as pd

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
table.drop(columns=['语文'], inplace=True) # 删除列
```

### 插入列

插入到第二列
```python
import pandas as pd

students = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
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

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
change_col_place(table, "收/支", 0, "收/支")  
```

### 修改列名

```python
import pandas as pd

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
table.rename(columns={"旧列名1":"新列名1", "旧列名2":"新列名"}, inplace=True) # 修改列名 
```


### 批量修改列数据

修改列数据

案例: 把金额列的`¥`去掉

```python
import pandas as pd

table = pd.read_excel("./student.xlsx", sheet_name="Sheet2")
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
            shutil.rmtree(path)
        os.mkdir(path)


class File:
    def __init__(self, path):
        self.name = os.path.basename(path)
        self.path = path
```

