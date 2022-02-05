# 第2课

## 1 查看有多少行和多少列

```python
import pandas as pd

df = pd.read_excel("C:/temp/People.xlsx")
print(df.shape)
print("done")
```

![image-20211218213814065](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223639.png)

## 2 查看列信息

```python
import pandas as pd

df = pd.read_excel("C:/temp/People.xlsx")
print(df.columns) # 查看列
```


![image-20211218214021362](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223642.png)



## 3 查看前面N行

```python
import pandas as pd

df = pd.read_excel("C:/temp/People.xlsx")
print(df.head(5)) # 查看前面5行
```



![image-20211218214355313](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223644.png)



## 4 查看末尾N行

```python
import pandas as pd

df = pd.read_excel("C:/temp/People.xlsx")
print(df.tail(5)) # 查看末尾5行
```

![image-20211218214520594](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223646.png)



## 5 错位读取

### 如果excel出现空行.比如前面2行为空行,第3行开始才有数据

![image-20211218214909271](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223648.png)

 `header`指定头部(标题头部)是第几行 , 

设置header=2. 由第3行开始读取. (header由0开始计算)

```python
import pandas as pd

people = pd.read_excel("C:/temp/People.xlsx", header=2)  # 由0开始数, 第3行为2.
print(people.head())
```

![image-20211218215126610](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223650.png)



#### 如果excel表没有标题头部

![image-20211218215649992](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223652.png)

设置`header=None`即可

```python
import pandas as pd

people = pd.read_excel("C:/temp/People.xlsx", header=None)  
print(people.head())
print(people.columns)
```

![image-20211218220009914](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223654.png)



**也可以自己自定义列名称**

```python
import pandas as pd

people = pd.read_excel("C:/temp/People.xlsx", header=None)
people.columns = ["id", "姓名"]  # 自定义列名称
print(people.head())
print(people.columns)
```

![image-20211218220347155](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211218223656.png)





