# Pandas

```python
import pandas as pd

df = pd.read_csv("路径")

df.ix[行索引,列索引]
#行索引前后都取到，列索引顾前不顾后
#行列索引可以用切片，也可以用列表或者元祖。

df['列名']#索引列
df['列名'].unique()#去重
df.count()#统计所有列的有效值个数

df[(df['列名1']==xxx)&(df['列名2']>xxx)]#筛选
df.describe()#按所有列输出平均数，方差，最小值，最大值，中位数和上下四分位数
df.describe(percentiles=(.20,.40,.80,.90))#自定义分位数
df.corr()#输出所有列之间的相关系数，n*n矩阵(n为列数)





```

# Matplotlib

```python
import matplotlib.pyplot as plt
plt.style.use('ggplot')
%matplotlib in line#jupyter设置，设置插图让其在记事本中可见
import numpy as np

fig,ax = plt.subplots(figsize = (6,4))
#创建一个高6英寸，宽4英寸的插图
#这里，fig是该图片的实体，ax是图片在内存中的对象
ax.hist(df['列名'],color='black')#通过插图对象调用hist(创建直方图的函数)，设置颜色
ax.set_ylabel('y轴名称',fontsize = 12)
ax.set_xlabel('x轴名称',fontsize = 12)
#设置x,y轴标签名，以及标签名的大小

plt.title('插图名称',fontsize=14)
```

```python
#拓展
fig,ax = plt.subplots(2,2,figsize = (6,4))
#创建一个2*2的插图，共四幅图
#这里，fig是该图片的实体，ax是四副图片在内存中的四个对象构成的矩阵
ax[0][0].hist(df['列名'],color='black')#
ax[0][0].set_ylabel('y轴名称',fontsize = 12)
ax[0][0].set_xlabel('x轴名称',fontsize = 12)
#设置第一行第一列的图像

'''
之后，可以用ax[0][1],ax[1][0],ax[1][1]设置其他三幅图
'''

plt.title('插图名称',fontsize=14)
```

```python
ax.scatter(df['列1'],df['列2'],color='green')
#先x轴，后y轴
ax.plot(df['列'])
```

