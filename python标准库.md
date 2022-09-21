# 标准库

## math

ceil(x)：返回大于等于x的最小整数

floor(x)：返回小于等于x的最大整数

sqrt(x)

pow(x, y)

log(x[, base])：返回以base为底的x的对数。默认为自然对数

sin(x)：三角函数，x是弧度

degrees(x)：弧度x转成角度

radians(x)：角度x转成弧度

## time

localtime(): 本地时间

gmtime(): utc 0时区时间

ctime(): 获取本地时间字符串

striftime(): 格式化输出

```python
from time import *

if __name__ == '__main__':
    print(localtime())
    print(gmtime())
    print(ctime())
    print(strftime('%Y-%m-%d %A %H:%M:%S', localtime()))
```



time.time(): 获取到epoch的秒数

perf_couter(): 随意选择一个时间点，获取现在时间到该时间的秒数，计入sleep时间

process_time(): 随意选择一个时间点，获取现在时间到该时间的秒数，不计入sleep时间

```python
import time
from time import *


if __name__ == '__main__':
    t1 = time()
    t2 = perf_counter()
    t3 = process_time()
    print(f't1:{t1}')
    print(f't1:{t2}')
    print(f't1:{t3}')

    res = 0
    for i in range(1000000):
        res += i

    sleep(5)

    t1_end = time()
    t2_end = perf_counter()
    t3_end = process_time()
    print(f'time方法，{t1_end - t1}')
    print(f'perf_count方法，{t2_end - t2}')
    print(f'process_time方法，{t3_end - t3}')

```

sleep():  线程暂停指定秒数

## datetime

datetime模块中有几个类：

datetime: 包含日期和时间

date: 只包含日期

time: 只包含时间

timedelta: 计算时间跨度

tzinfo：时区信息

```
创建datetime类对象
datetime.datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None)

datetime.today() 返回当前的本地日期和时间
datetime.now(tz=None) 返回指定时区的日期和时间
datetime.fromtimestamp(timestamp, tz=None) 返回时间戳对应的时间


创建date类实例
datetime.date(year, month, day)

date.today() 返回当前日期
date.fromtimestamp() 


创建timedelta类实例 下面的时间间隔正数负数都行
datetime.timedelta(days=0, seconds=0, microseconds=0, millseconds=0, minutes=0, hours=0, weeks=0)

d = datetime.datetime(2000, 1, 1)
print(d)
# 一周前
delta = datetime.timedelta(weeks=-1)
d += delta
print(d)
```



## random

>  [python random随机数、numpy.random随机数 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/165642830) 

seed(): 指定随机数种子。种子相同则随机数相同，一个种子只能返回一个随机数。默认种子是当前系统时间。

```python
seed(10)
print(random())  # 0.5714025946899135
seed(10)
print(random())  # 0.5714025946899135
print(random())  # 0.4288890546751146
```

randint(1,10): 产生指定范围的随机整数

randrange(a): 产生[0, a)范围中的随机整数

randrange(a,b,step): 产生[a,b)，步长为step的随机整数

random(): 产生[0.0, 1.0) 之间的随机浮点数

uniform(a,b): 产生[a,b]之间的随机浮点数，服从均匀分布

choice(seq)：从序列中随机返回一个元素

choices(seq, weight=None, k)：对序列进行k次重复采样，可指定权重

```python
print(choices('python', k=5))  # ['n', 'n', 'h', 't', 'o']
print(choices('python', weights=[1, 1, 1, 3, 3, 3], k=5))  # ['h', 'o', 'p', 'h', 'o']
```

shuffle(seq)：对序列中的元素进行原地随机排列

sample(pop, k)：从pop类型中随机采样k个元素



gauss(mean, std)：产生一个符合高斯分布的随机数

```python
from random import *
import matplotlib.pyplot as plt


if __name__ == '__main__':
    # 生成符合高斯分布的大量随机数
    res = [gauss(0, 1) for i in range(100000)]
    # 将一个大区间分成bins个小区间，统计每个小区间的元素个数
    plt.hist(res, bins=1000)
    plt.show()

```



## re 正则表达式模块

match(p, text) 使用正则表达式p去区配字符串text。匹配成功返回一个match对象，否则返回None

search(p, text)  找到就返回第一个匹配的match对象，否则返回None。

findall(p, text) 返回所有匹配的字符串列表，否则返回None

sub(pattern, repl, string, count=0) 替换匹配的字符串。count是要替换的最大数量，默认是0，表示不限制。

split(pattern, string, maxsplit=0) 分割字符串，返回被分割的字符串列表。

```python
import re


p1 = r'.*@gmail.com$'
text1 = 'hellopython@gmail.com'

m = re.match(p1, text1)
print(m)  # <re.Match object; span=(0, 21), match='hellopython@gmail.com'>

p2 = r'\d+'
s2 = 'AB12CD34EF'
s3 = re.sub(p2, ' ', s2)
s4 = re.split(p2, s2)
print(s3)
print(s4)

```



## collections

namedtuple 是模块提供的元组子类

```python
import collections
import random

# 模拟扑克牌 每张牌是一个元组，由大小和花色组成
Card = collections.namedtuple('Card', ['rank', 'suit'])
ranks = [str(n) for n in range(2, 11)] + list('JQKA')
suits = 'spades diamonds clubs hearts'.split()

# 生成52张扑克牌
cards = [Card(r, s) for r in ranks for s in suits]
# 洗牌
random.shuffle(cards)
# 随机抽取
print(random.choices(cards, k=3))


```

Counter 是字典的子类，使用此类就不用像以前一样手动计算每个字符出现的次数了

```python
import collections


text = '牛奶奶给刘奶奶买牛奶'
colors = ['red', 'green', 'blue', 'black']
text_counter = collections.Counter(text)
colors_ctr = collections.Counter(colors)
print(text_counter)  # Counter({'奶': 5, '牛': 2, '给': 1, '刘': 1, '买': 1})
print(colors_ctr)  # Counter({'red': 1, 'green': 1, 'blue': 1, 'black': 1})
# 元素展开
print(list(text_counter.elements()))  # ['牛', '牛', '奶', '奶', '奶', '奶', '奶', '给', '刘', '买']

```

deque是一个双向队列。

append() 是右侧入队，appendleft() 是左侧入队

pop() 右侧出队，popleft() 左侧出队



## itertools 迭代器

**排列组合的工具：**

product() 生成笛卡尔积

```python
for i in itertools.product('abc', '12'):
    print(i)
for i in itertools.product('abc', repeat=3):
    print(i)
```

permutations() 排列

```python
for i in itertools.permutations('abcd', 3):  # 4选3的排列
    print(i)
for i in itertools.permutations(range(3)):  # 默认全排列
    print(i)
```

combinations() 组合

combinations_with_replacement() 元素可以重复的组合

**拉链工具：**

zip 把相同位置的元素组合到一起。如果长度不一样，到最短的停止。zip_longest到最长的停止，不足的补上一个None。

```python
for i in zip('abc', '123', '$#!'):
    print(i)
# ('a', '1', '$')
# ('b', '2', '#')
# ('c', '3', '!')
```

**无穷迭代器：**

count(start=0, step=1) 从初始值开始，返回一个无穷的流

cycle(iterable) 无穷循环迭代对象的元素

repeat(object[, times]) 不断重复object

**其他：**

chain(iterables) 把一组迭代对象串联起来，形成一个更大的迭代器

enumerate(iterable, start=0) 返回元素的索引和值。索引默认从0开始，也可以设置start指定第一个元素的索引。

groupby(iterable, key=None) 对元素分组。返回组名和每组的迭代器。key=None默认按照重复元素分组。

```python
for key, group in itertools.groupby('aaabbbcc'):
    print(key, list(group))
# a ['a', 'a', 'a']
# b ['b', 'b', 'b']
# c ['c', 'c']

colors = ['red', 'blue', 'black', 'green', 'grey']
# 先按照长度排序 再按长度分组
colors.sort(key=len)
for key, group in itertools.groupby(colors, key=len):
    print(key, list(group))
# 3 ['red']
# 4 ['blue', 'grey']
# 5 ['black', 'green']
```

## base64

```
>>> import base64
>>> base64.b64encode(b'binary\x00string')
b'YmluYXJ5AHN0cmluZw=='
>>> base64.b64decode(b'YmluYXJ5AHN0cmluZw==')
b'binary\x00string'


>>> base64.b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd++//'
>>> base64.urlsafe_b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd--__'
>>> base64.urlsafe_b64decode('abcd--__')
b'i\xb7\x1d\xfb\xef\xff'
```

## hashlib

计算md5：128 bit/16字节，通常用32个16进制字符表示

```python
import hashlib

md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
print(md5.hexdigest())

# 如果数据量很大，可以分块多次调用update()，最后计算的结果是一样的
md5 = hashlib.md5()
md5.update('how to use md5 in '.encode('utf-8'))
md5.update('python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

SHA1：160 bit/20字节，通常用40个16进制字符表示

```python
import hashlib

sha1 = hashlib.sha1()
sha1.update('how to use sha1 in '.encode('utf-8'))
sha1.update('python hashlib?'.encode('utf-8'))
print(sha1.hexdigest())
```

SHA256和SHA512用法类似

### 加盐

用户输入的简单密码，比如 123456，拼接上一个复杂的字符串（叫做盐），然后对 `123456Salt`计算出hash值。这个hash值就是要保存的密码。

## urllib.request

```python
import urllib.request

url = 'www.whether.com?city=shanghai'

req = urllib.request.Request(url) # 创建Request对象，默认是get请求
with urllib.request.urlopen(req) as response: # 发送请求，收到响应
    data = response.read()  # 读取所有字节
    json_data = data.decode('gbk')
    print(json_data)
    
```

post方式：

```python
import urllib.request
import urllib

url = 'www.whether.com?city=shanghai'

req_body = {'city':'shanghai', 'date':'20220921'}
req_str = urllib.parse.urlencode()

req = urllib.request.Request(url) # 创建Request对象，默认是get请求
with urllib.request.urlopen(req) as response: # 发送请求，收到响应
    data = response.read()  # 读取所有字节
    json_data = data.decode('gbk')
    print(json_data)
```



