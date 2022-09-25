## 概述

pandas 基于 numpy，提供了很多数据分析的工具。numpy 主要用来处理数值型数据，而 pandas 用来处理非数值型数据。

pandas 的核心是其特有的数据结构`DataFrame`和`Series`

pandas对象分成：

- 一维series: 列表、字典、narray等
- 多维DataFrame：Series、字典列表、二维narray

## Series

`Series`的内部结构包含了两个数组，data用来保存数据，index个用来保存数据的索引。

### 创建

Series的创建：

- 通过数组或ndarray创建，必须是一维的
- 通过字典创建

```python
from pandas import Series

# 由数组创建
s = Series(data=[1,2,3,'4'])
print(s)  # 没有指定index，默认从0开始

s2 = Series(data=[1,2,3], index=['a','b','c'])
print(s2)
```

```python
from pandas import Series
import numpy as np
```


```python
s1 = Series(data=[1,2,3,'four'])
s1
```




    0       1
    1       2
    2       3
    3    four
    dtype: object




```python
s2 = Series(data=[1,2,3], index=['a','b','c'])
s2
```




    a    1
    b    2
    c    3
    dtype: int64




```python
# 通过字典创建
scores = {
    'math': 99,
    'english': 125,
    'physical': 99
}
s3 = Series(data = scores)
s3
```




    math         99
    english     125
    physical     99
    dtype: int64



### 索引和切片


```python
s3[0]
s3.english
s3[:2]
```




    math        99
    english    125
    dtype: int64



### 常用属性
- shape
- size
- index
- values 返回元素值，这里不是data
- dtype

### 常用方法
- head() tail()
- unique()
- isnull() notnull()
- add() sub() mul() div()


```python
s = Series(data = [1,2,3,4,5,6,7,8,9,9,9])
s.head(3)  # 前n个数，默认5
s.tail(2)
s.unique()  # 去重重复元素
s.isnull()  # 返回布尔数组
```




    0     False
    1     False
    2     False
    3     False
    4     False
    5     False
    6     False
    7     False
    8     False
    9     False
    10    False
    dtype: bool



Series在什么情况下会产生null呢？看Series的运算法则：
- 索引相同的元素进行算数运算，否则为空


```python
s1 = Series(data=[1,2,3], index=['a','b','c'])
s2 = Series(data=[1,2,3], index=['a','d','c'])
s = s1 + s2
s
```




    a    2.0
    b    NaN
    c    6.0
    d    NaN
    dtype: float64




```python
s.isnull()
```




    a    False
    b     True
    c    False
    d     True
    dtype: bool



## DataFrame
DataFrame 是一个表格型的数据结构，是**由一定顺序排列的多列数据组成**。设计初衷是将Series的使用场景从一维扩展成多维。DataFrame有行索引，也有列索引。
- 行索引 index
- 列索引 columns
- 值 values

### 创建
- 通过数组或ndarray
- 通过字典


```python
from pandas import DataFrame
```


```python
df = DataFrame(data=[[1,2,3], [4,5,6]])
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = DataFrame(data=np.random.randint(1,10,(2,3)))
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9</td>
      <td>8</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
dic = {
    'name':['tom', 'jack', 'mary'],
    'age':[12, 23, 44]
}
df = DataFrame(data=dic, index=['a', 'b', 'c'])  # index指定行索引，字典的key是列索引
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>tom</td>
      <td>12</td>
    </tr>
    <tr>
      <th>b</th>
      <td>jack</td>
      <td>23</td>
    </tr>
    <tr>
      <th>c</th>
      <td>mary</td>
      <td>44</td>
    </tr>
  </tbody>
</table>
</div>



### 属性
- values
- index
- columns
- shape


### 索引和切片
- 对行索引
- 对列索引
- 对元素索引


```python
df = DataFrame(data=np.random.randint(0, 100, size=(10,5)), columns=['a', 'b', 'c', 'd', 'e'])
df
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>70</td>
      <td>8</td>
      <td>67</td>
      <td>53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>50</td>
      <td>70</td>
      <td>19</td>
      <td>66</td>
      <td>63</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>89</td>
      <td>48</td>
      <td>77</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41</td>
      <td>27</td>
      <td>72</td>
      <td>68</td>
      <td>97</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53</td>
      <td>35</td>
      <td>50</td>
      <td>12</td>
      <td>30</td>
    </tr>
    <tr>
      <th>5</th>
      <td>77</td>
      <td>33</td>
      <td>93</td>
      <td>70</td>
      <td>97</td>
    </tr>
    <tr>
      <th>6</th>
      <td>27</td>
      <td>39</td>
      <td>68</td>
      <td>96</td>
      <td>12</td>
    </tr>
    <tr>
      <th>7</th>
      <td>21</td>
      <td>96</td>
      <td>3</td>
      <td>99</td>
      <td>91</td>
    </tr>
    <tr>
      <th>8</th>
      <td>37</td>
      <td>88</td>
      <td>80</td>
      <td>77</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>29</td>
      <td>9</td>
      <td>36</td>
      <td>0</td>
      <td>20</td>
    </tr>
  </tbody>
</table>




```python
df['a']  
# df的列是显式索引，行是默认的隐式索引。
# 这种情况下中括号只能使用显式索引。所以这里是按列取值
```




    0    19
    1    50
    2    37
    3    41
    4    53
    5    77
    6    27
    7    21
    8    37
    9    29
    Name: a, dtype: int32




```python
df[['a', 'c']]  # 取多列
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>50</td>
      <td>19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41</td>
      <td>72</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53</td>
      <td>50</td>
    </tr>
    <tr>
      <th>5</th>
      <td>77</td>
      <td>93</td>
    </tr>
    <tr>
      <th>6</th>
      <td>27</td>
      <td>68</td>
    </tr>
    <tr>
      <th>7</th>
      <td>21</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>37</td>
      <td>80</td>
    </tr>
    <tr>
      <th>9</th>
      <td>29</td>
      <td>36</td>
    </tr>
  </tbody>
</table>



- iloc 通过隐式索引取行
- loc 通过显示索引取行。没有显式索引的时候这个方法和iloc相同


```python
# 取单行
df.iloc[0]
# 取多行
df.iloc[[0,3,5]]
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>70</td>
      <td>8</td>
      <td>67</td>
      <td>53</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41</td>
      <td>27</td>
      <td>72</td>
      <td>68</td>
      <td>97</td>
    </tr>
    <tr>
      <th>5</th>
      <td>77</td>
      <td>33</td>
      <td>93</td>
      <td>70</td>
      <td>97</td>
    </tr>
  </tbody>
</table>




```python
# 取单个元素
df.iloc[0,2]
df.loc[0, 'b']
```




    70




```python
# 取多个元素
df.iloc[[0,1,2],1]
df.loc[[0,1,2],'b']
```




    0    70
    1    70
    2    89
    Name: b, dtype: int32



### 切片

```python
# 切行
df[0:2]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>70</td>
      <td>8</td>
      <td>67</td>
      <td>53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>50</td>
      <td>70</td>
      <td>19</td>
      <td>66</td>
      <td>63</td>
    </tr>
  </tbody>
</table>




```python
# 切列
df.iloc[:, 0:2]
df.loc[:, ['a','c']]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>50</td>
      <td>19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41</td>
      <td>72</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53</td>
      <td>50</td>
    </tr>
    <tr>
      <th>5</th>
      <td>77</td>
      <td>93</td>
    </tr>
    <tr>
      <th>6</th>
      <td>27</td>
      <td>68</td>
    </tr>
    <tr>
      <th>7</th>
      <td>21</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>37</td>
      <td>80</td>
    </tr>
    <tr>
      <th>9</th>
      <td>29</td>
      <td>36</td>
    </tr>
  </tbody>
</table>



**总结**
- df[col] 取列
- df.loc[index] 取行
- df.loc[index, col] 取元素
- df[start:end] 切行
- df.iloc[:, start:end] 切列

### 运算
同Series

### 类型转换


```python
dic = {
    'time':['2022-01-01', '2022-02-02', '2022-03-03'],
    'temp':[33,34,35]
}
df = DataFrame(data = dic)
df
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>time</th>
      <th>temp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-01-01</td>
      <td>33</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-02-02</td>
      <td>34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-03-03</td>
      <td>35</td>
    </tr>
  </tbody>
</table>



```python
# 查看time列的数据类型
df['time'].dtype
```




    dtype('O')




```python
# 将time列转成时间序列类型
import pandas as pd
df['time'] = pd.to_datetime(df['time'])
df['time'].dtype
```




    dtype('<M8[ns]')




```python
# 将time列作为源数据的行索引
df.set_index('time')  # 返回新的DataFrame
# df.set_index('time', inplace=True)  # 改变原始DataFrame
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>temp</th>
    </tr>
    <tr>
      <th>time</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2022-01-01</th>
      <td>33</td>
    </tr>
    <tr>
      <th>2022-02-02</th>
      <td>34</td>
    </tr>
    <tr>
      <th>2022-03-03</th>
      <td>35</td>
    </tr>
  </tbody>
</table>