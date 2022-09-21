# Numpy

## 概述
支持常见的数组和矩阵操作，通过`ndarray`类实现了对多维数组的封装，提供了操作这些数组的方法和函数集。

Numpy 并没有提供很多统计的算法，算法主要使用 pandas 等模块，它们都可以操作 ndarray 对象。

为什么numpy运行的速度更快？

1. numpy 底层是由c语言编写的。
2. numpy数组中的元素都是同类型的，而python列表支持任意类型数的填充。所以numpy数组是连续存储在栈中的，读取速度更快。
3. python执行时有线程锁，无法实现真正的多线程并行，而c语言可以。当使用多核 CPU 时，Numpy会自动做并行计算。

什么时候用numpy？

numpy 可以使用数组表达式完成多种数据操作，而不用写大量的循环，这种利用数组表达式替代显示循环的方法，叫做向量化。

遇到使用python for循环处理一些向量化、矩阵化操作的时候，优先考虑使用numpy。

> https://numpy.org/doc/stable/reference/index.html

## numpy数组的创建

1. 从列表创建

   ```python
   import numpy as np
   
   x = np.array([1, 2, 3, 4, 5])
   print(x)  # [1 2 3 4 5]
   # nx = np.array([1, 2, 3], dtype='float32') 创建numpy数组的时候执行类型
   print(type(x))  # <class 'numpy.ndarray'>
   print(x.shape)  # (5,)
   
   
   # 二维数组
   y = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
   print(y)
   print(y.shape)  # (3, 3)    
   
   ```

2. 从头创建数组

   ```python
   import numpy as np
   
   # 创建长度为5的数组，值都为0
   np.zeros(5, dtype=int)
   # 创建2X4的浮点数组，初值为1
   np.ones((2, 4), dtype=float)
   # 创建3X4的数组，初值8.8
   np.full((3, 4), 8.8)
   # 创建n=3的单位矩阵
   np.eye(3)
   # 创建没有初始化的数组
   np.empty((2, 3))
   # 创建等差数组
   np.arange(1, 10, 2)
   # 数组有4个元素，均匀分布在0到1之间
   np.linspace(0, 1, 4)
   
   # 创建3X3的，[0,1)之间的随机数组
   下面两个方法一样，但是参数不一样
   # 均匀分布
   np.random.random((3, 3)) 接收元组
   np.random.rand(3, 4) 接受可变参数
   # 标准正态分布
   np.random.randn(3, 4) 
   np.random.standard_normal(3, 4)
   # 一般正态分布
   np.random.normal(0, 1, (3, 3))
   # 创建3X3的，[0, 10)之间的随机整数数组
   np.random.randint(0, 10, (3, 3))
   
   # 随机重排列
   x = np.array([1, 2, 3, 4, 5])
   # 返回一个新列表
   print(np.random.permutation(x))
   # 原地重排
   np.random.shuffle(x)
   
   # 这个方法和内置的range()方法用法相同
   x = np.arange(0, 25)
   # 随机采样
   # 采样一个3x4的数组
   np.random.choice(x, size=(3, 4))
   # 按概率采样
   np.random.choice(x, size=(3, 4), p=x / np.sum(x))
   
   ```

## numpy数组的性质

**内部属性：**

   1. shape 形状
   2. ndim 维度
   3. size 大小

   ```python
   import numpy as np
   
   x = np.array([[1, 2], [3, 4]])
   print(x.size)  # 4
   print(x.shape)  # (2, 2)
   print(x.ndim)  # 2
   
   ```

**数组的索引：**

多维数组有两种表达方式：

```
a[0, 2]
a[0][2]
```

注意：行所在的轴叫做0轴，列所在的轴叫做1轴。

**数组的数据类型：**

numpy数组的元素类型是确定的，如果把浮点数赋值给整型数组，会取整。

创建 ndarray 时不指定 dtype，默认都是 float64。

```python
x = np.full((2, 3), 1, dtype=int)
x[1][2] = 2.4
print(x)
# [[1 1 1]
# [1 1 2]]
```

可以使用 astype() 显示转换数据类型：

```python
arr = np.array([1, 2, 3])
print(arr.dtype)  # int
float_arr = arr.astype(np.float64)
print(float_arr.dtype)  # float64
```

astype() 还可以将字符串转成数值型，可能抛出 ValueError。

**数组的切片：**

一维数组没变，二维数组如下：

```python
x = np.random.randint(10, size=(3, 3))
print(x)
print(x[::-1, ::-1])

```

单独一个冒号代表获取整个轴，用这种方式可以获取数组的行列

```python
# 获取第1行, 行索引从0开始
print(x[1, :])  # x[1] 这是简写
x[:, 2]  # 获取第二列
```

注意：数组的切片获取的都是视图，不是副本。对切片后数据的修改会影响本来的数组。可以使用copy()来获取副本：

```python
x[1].copy()
```

## 数组的变换

**数组的变形：**

一维数组在数量不变的情况下变成二维数组：

```python
x = np.random.randint(0, 10, (12, ))
print(x)
x2 = x.reshape((3, 4))
print(x2)

# reshape返回的元素还是原来的
x2[0][0] = 100  # x[0]也会变成100

x = np.arange(10)
print(x)
# 一维向量变成二维数组的一个行向量
x2 = x[np.newaxis, :]
print(x2)
# 一维向量变成二维数组的一个列向量
x3 = x[:, np.newaxis]
print(x3)
```

多维变成一维：

```python
x = np.random.randint(0, 10, (2, 3))

# 返回的是副本
x.flatten()
# 返回的是视图
x.ravel()
# 返回的是试图
x.reshape(-1)
```

拼接：

hstack() 水平拼接，两个数组的行数必须一样

vstack() 垂直拼接

## 运算

**一个向量运算：**

`+ - * / ** % // ` 作用到每一个元素

`np.abs(x) np.sin(x) np.exp(x) np.log(x) np.log2(x) np.log10(x) `也都是作用到每个元素

```python
x = np.arange(10)
print(x + 5)
print(np.sin(x))
```

**两个向量运算：**

两个向量相同位置的元素进行运算。

```python
x = np.arange(5)
y = np.arange(5)
print(x - y)
np.maxium(x, y)
```

**矩阵运算：**

```python
x = np.arange(9).reshape((3, 3))
x = x.T  # 转置

x = np.array([[0, 1], [1, 0]])
y = np.array([[1, 0], [0, 1]])
print(x.dot(y))  # 矩阵乘法
# 或者np.dot(x, y)
```

**广播运算：**

```python
# 如果两个矩阵维度不一样，低维的会扩展
x = np.arange(9).reshape((3, 3))
y = np.array([1, 2, 3])
print(x + y)  # y被扩展成了二维的

# 一个列向量和一个行向量进行四则运算，两个向量都会扩展成二维的
x = np.array([1, 2, 3])  # 行向量
y = x[:, np.newaxis]  # 列向量
print(x + y)
```

**网格坐标矩阵：**

meshgrid()

**比较运算、逻辑运算和掩码：**

numpy 使用C语言的符号，python的关键字 and、or都没用

```python
# 下面所有的np都不能丢了
x = np.random.randint(0, 9, (3, 3))
print(x)
print(x > 5)  # bool值组成的二维数组
x[x !> 0] = 0  # 把所有不大于0的数设置成0
print(np.sum(x > 5))  # 统计大于5的个数
print(np.any(x > 10))  # False
print(np.all(x < 10))  # True
print(np.all(x < 5, axis=1))  # 按列判断
print((x > 0) & (x < 7))

print(x[x > 5])  # bool数组作为掩码获取数据
# where()的作用类似if...else
a = np.arange(-10, 10, 1)
cond = a > 0
a = np.where(cond, 2, -2)  # 把a中大于0的数都变成2，否则变成-2
```

**神奇索引：**

神奇索引：索引是数组 结果和索引的结构一样
```python
x = np.random.randint(100, size=9).reshape(3, 3)

ids = [1, 2]
print(x[ids])  # 索引1，2行

# 索引是多维数组
rows = [1, 2]
cols = [1, 2]
x[rows, cols]  # x[1, 1] x[2, 2]
```
神奇索引的结果总是一维的，比如上面的 x[rows, cols] 只返回了两个数据。

结果和我的想法不一样，我想要的是二阶子式，应该这样写：
```python
x[[1, 2]][:, [1,2]]
```

**布尔索引常用来修改数据：**

```python
a = np.random.randn(3,3)
print(a < 0)
a[a < 0] = 0  # 把负数都设置成0
```


**排序：**

np.sort(x) 返回新的排序数组

x.sort() 原地排序，可以指定按轴排序

np.argsort(x) 返回数组中元素对应的位置

```python
x = np.array([5, 3, 1, 6])
print(x.argsort())  # [2 1 0 3]
```

**最值**

np.max(x) np.min(x) 返回最大最小值

np.argmax(x) np.argmin(x) 返回最值的索引

**求和：**

np.sum(x) x.sum() 都可以求和

np.sum(x, axios=1) 按行求和

np.sum(x, axios=0) 按列求和

布尔值在底层是1和0，所以布尔值数组也可以求和。

np.prod(x) x.prod() 求积，就是元素按个相乘。与求和类似，可以按行按列运算。

**过滤:**

np.unique()

**累积数组：**

np.cumsum 和 np.cumprod都可以把数组转成累计数组，前者是累积和，后者是累积积

```python
In [186]: arr = np.array([[0, 1, 2], [3, 4, 5], [6, 7, 8]])
In [187]: arr
Out[187]:
array([[0, 1, 2],
[3, 4, 5],
[6, 7, 8]])
In [188]: arr.cumsum(axis=0)  # 每行累计
Out[188]:
array([[ 0,  1,  2],
[ 3,  5,  7],
[ 9, 12, 15]])
In [189]: arr.cumprod(axis=1)  # 每列累计
Out[189]:
array([[  0,    0,    0],
[  3,  12,  60],
[  6,  42, 336]])

```



## 统计

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.random.normal(0, 1, 10000)  # 1000个正态分布的随机数
plt.hist(x, bins=50)  # 分成50组计算频率
plt.show()
```

![](numpy%E5%BA%93.assets/Figure_1.png)

```python
np.median(x)  # 中位数
np.mean(x)  # 均值
np.std(x)  # 标准差
np.var(x)  # 方差
# 上面的方法都有ndarray实例方法版本
```

