# 第17课  多表联合（从VLOOKUP到JOIN）
## 1 原生excel查找其它sheet中的数据,vlookup公式

Sheet1的数据

![image-20211219150420184](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153626.png)

Sheet2的数据

![image-20211219150447059](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153628.png)

 假如`sheet1`增加1列基于`Sheet2`的数据.

比如增加**优惠价**



![image-20211219150657146](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153631.png)

1 添加公式的单元格,然后输入=vlookup

选中`id`后,输入`逗号`输入第二个值

![image-20211219150826114](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153632.png)



2 到查询的表中选择查询数据的范围,作为第二个值

![image-20211219151138240](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153634.png)

3 选择需要返回的数据列

比如选择列2返回优惠价

![image-20211219151253661](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153636.png)

4 整条公式

```
=VLOOKUP(A2,Sheet2!A2:B6,2,FALSE)
```

最后一个参数为`非近似匹配`(精准匹配)所以是`FALSE`



## 2 使用merge合并其他表格数据

### 内连接

>内连接: 两个表都匹配的数据才合并(`相当于求交集`)

![image-20211219151828406](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153638.png)

![image-20211219151837271](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153642.png)

```python
import pandas as pd

file = "./excel/books.xlsx"
table1 = pd.read_excel(file, sheet_name="Sheet1")
table2 = pd.read_excel(file, sheet_name="Sheet2")
r = table1.merge(table2, how="inner", on="id"  )
print(r)

```

![image-20211219151809253](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153644.png)



### 左连接

![image-20211219152313423](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153646.png)

![image-20211219152331127](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153647.png)


```python

import pandas as pd

file = "./excel/books.xlsx"
table1 = pd.read_excel(file, sheet_name="Sheet1")
table2 = pd.read_excel(file, sheet_name="Sheet2")
r = table1.merge(table2, how="left", on="id") # id 列做为条件 , 左连接
print(r)
```

![image-20211219152409020](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153649.png)



### join字段设置

table1的id字段和, table2的"id2"字段做为查询条件合并

```
import pandas as pd

file = "./excel/books.xlsx"
table1 = pd.read_excel(file, sheet_name="Sheet1")
table2 = pd.read_excel(file, sheet_name="Sheet2")
r = table1.merge(table2, how="left", left_on="id",right_on="id2")
print(r)
```



 ## 3 join函数

![image-20211219153411216](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153652.png)

![image-20211219153419793](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153653.png)

```python
import pandas as pd

file = "./excel/books.xlsx"
table1 = pd.read_excel(file, sheet_name="Sheet1", index_col="id")
table2 = pd.read_excel(file, sheet_name="Sheet2", index_col="id")
r = table1.join(table2, on="id")  ## 默认 how="left"左连接
print(r)
```

![image-20211219153349002](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219153655.png)



##  4. 向下合并其他表的数据

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
