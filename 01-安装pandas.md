# 第1课 安装Pandas

## 0.安装`python`和`pip`

https://www.python.org/

## 1.安装依赖

安装`pandas`

```bash
pip install pandas
```

安装`xlwt` 

```bash
pip install xlwt
```
## 2. Pandas 数据结构 - DataFrame

>`DataFrame `是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数>>值、字符串、布尔型值）。`DataFrame `既有行索引也有列索引，它可以被看做由 `Series `组成的字典（共同用一个索引）。
>原文地址: https://www.runoob.com/pandas/pandas-dataframe.html

![image-20211220214020819](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211220214444.png)


```python
# 实例 - 使用 ndarrays 创建

import pandas as pd

data = {'Site':['Google', 'Runoob', 'Wiki'], 'Age':[10, 12, 13]}
df = pd.DataFrame(data)
print (df)
```

从以上输出结果可以知道， DataFrame 数据类型一个表格，包含 rows（行） 和 columns（列）：

![img](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211220214447.jpg)

![img](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211220214455.png)