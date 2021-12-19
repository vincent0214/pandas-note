# 第17课

## 校验数据

![image-20211219155224865](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219155418.png)

```python
import pandas as pd

# 检查价格
def check_price(x):
    if x["价格"] < 100:
        print(f"#{x.id} 价格少于100")

file = "./excel/books.xlsx"
books = pd.read_excel(file)
books.apply(check_price, axis=1) # axis=1(循环每行)  axis=0(循环每列) 

```

![image-20211219155246914](https://markdown-1301532546.cos.ap-guangzhou.myqcloud.com/markdown/20211219155421.png)



