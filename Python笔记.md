 

# Python基础

## 列表、元组、字典

1. 用中括号`[]`表示列表，用逗号分隔

```python
a = ['Tom','Jack','Tony']
a[0]		//Tom
```

2. 元组是不可变的列表，用圆括号`()`表示

```python
b = ('Hangzhou','Shanghai','Nanjing')
b[1]		//Shanghai
```

3. 字典是键值对形式，用花括号`{}`表示

```python
c = {'name':'Zhangsan','sex':'man','age':22}
c['age']		//22
```







# Numpy

## array与ndarray

- array()是一个函数，ndarray是一个对象
- 通过np.array()将数据变成ndarray对象
- array()输入参数类型有 ：   列表、元组、字典

## 维度信息

```python
a = np.arange(24).reshape(2,3,4)
print(a.ndim)			#3
print(a.shape)			#(2,3,4)
print(a.shape[0])		#2
print(a.shape[1])		#3
print(a.shape[2])		#4
print(a.size)			#24   size计算数组元素数量

[[[ 0  1  2  3]
  [ 4  5  6  7]
  [ 8  9 10 11]]

 [[12 13 14 15]
  [16 17 18 19]
  [20 21 22 23]]]
```

- `a.ndim`输出数组的维数。所谓维数，就看有几层中括号`[]`
- `numpy`中“轴”的概念就是数组的维数，第一个轴就是第一层`[]`，第二个轴就看第二层`[]`，所以上面示例共有3个轴
- `a.shape`返回的数据不要用行和列的概念去理解，用轴的概念去理解
- 上文程序框中，最外层`[]`内有两个第二层`[]`，所以a.shape[0]=2；第二层`[]`内有3个第三层`[]`，所以a.shape[1]=3；第三层`[]`内有4个元素，所以a.shape[2]=4；



## axis的理解

```python
cc=np.array([[1,2],[3,4]])
cc.sum(axis=0)	#array([4, 6])
cc.sum(axis=1)	#array([3, 7])
```

- axis=0代表最外层括号，axis=1代表次外层括号，以此类推
- axis=0代表最外层括号内的元素相加(1+3,2+4)，axis=1代表第二层括号内元素相加(1+2,3+4)



## 数据类型

```python
a = np.arange(24).reshape(2,3,4)
print(a.dtype)				#int32	数据类型
print(a.itemsize)			#4		数据类型占多少字节
```



## 数组初始化

```python
a = np.zeros(5)
a = np.ones(5)
a = np.empty(5)
b= np.zeros([2,3])
```

- 这三个都是numpy库中的方法，因此使用时要加上np.
- 当创建多维数组时，要加上`[]`

## 等差数组

```python
a = np.linspace(1,5,5)		#[1,2,3,4,5]
a = np.arange(1,5,1)		#[1,2,3,4]
a= np.arange(5)				#[0,1,2,3,4]
```

- `linspace`是生成**指定元素个数**的数组，`np.linspace(1,5,5)`表示包括1，5在内，等间距生成5个元素
- `arange`是**指定间距大小**，`np.arange(1,5,1)`表示从1到5（不包括5），以1为间距生成数组
- `arange`不添加起始信息，则默认第一个数字为0，间距为1

## 索引

> ***数组切片返回的是数组数据的视图，不是副本，意味着对切片修改后，本身数组也会修改***

```python
#一维索引
a = np.arange(20)
a[2]		#2
a[1:4]		#[1,2,3]
a[:4:2]		#[0,2]
a[4:1:-1]	#[4,3,2]
```

- `[]`内第一个`：`的左右分别是起始索引和终点索引
- 当间隔为正数时，表示从前往后索引，起始索引数字<终点索引数字
- 当间隔为负数时，表示从后往前索引，起始索引数字>终点索引数字
- 当间隔为1时，输出不包含终点索引的元素

```python
#二维索引
a = np.arange(12).reshape(3,4)
a[2,3]				#11
a[:,1]				#[1,5,9]
a[0:2,0:2]			#[[0,1],[4,5]]
a[1]				#[4,5,6,7]
a[1,:]				#[4,5,6,7]
a[,1]				#错误写法
a[:,1]				#[1,5,9]

[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]

```

- 对于二维数组，当`[]`内只有一个参数时，默认访问一行

- 要访问一列，必须写成`a[:,1]`，不能 写成`a[,1]`

## 形状

```python
a = np.arange(6)				#[0,1,2,3,4,5]
b = a.reshape(2,3)				#[[0,1,2],[3,4,5]]
c = b.ravel()					#[0,1,2,3,4,5]
a.shape=(3,2)					#等于a.resize((3,2))
print(a)						#[[0,1],[2,3],[4,5]]
```

- `ravel`是将多维数组展平
- `reshape`、`resize`都是改变数组形状，`reshape`有返回值，即可以写成`print(a.reshape(2,3))`；`resize`无返回值，不可以写成`print(a.resize(2,3))`，只能单独一条语句`a.resize(2,3)`，等同于`a.shape=(2,3)`



## 广播机制

1. 如果两个数组的维度不相同，那么小维度数组的形状将会在最左边补1
2. 如果两个数组的形状在任何一个维度上都不匹配，那么数组的形状将会沿着维度为1的维度扩展以匹配另外一个数组的形状
3. 如果两个数组的形状在任何一个维度上都不匹配且没有任何一个维度等于1，那么将会引发异常



# Pandas

1. Pandas共有三种类型，即Series、DateFrame、Panel



## Series

- 用来存储一维数据
- 通过pd.Series()将数据变成Series类型
- pd.Series()输入参数类型有：   列表、字典、ndarray





## DateFrame

### 简介

- 用来存储二维数据



### 构造

- 通过pd.DateFrame()将数据变成DateFrame类型
- pd.DateFrame()输入参数类型有： 二维列表List 、二维ndarray、 字典（其值是一维列表List、ndarray或Series）



### 访问方法

#### 按行索引

```python
#方法一	按行号切片访问
a = np.arange(12).reshape(3,4)
aa = pd.DateFrame(a,index=['row1','row2','row3'],columns=['col1','col2','col3','col4'])
print(aa[1:2])		
```

1. `[]`内必须是切片，**单独一个行号错误**
2. `[]`内必须是**行号切片**，用行标签错误
3. `[0:2]`表示访问第一行和第二行
4. 当只访问一行时`[0:1]`或多行时`[0:2]`，返回类型都是`DataFrame`

```python
#方法二	按行标签访问
a = np.arange(12).reshape(3,4)
aa = pd.DateFrame(a,index=['row1','row2','row3'],columns=['col1','col2','col3','col4'])
print(aa.loc['row1'])				#访问第一行
print(aa.loc[['row1','row2']])		#访问第一、二行
print(aa.loc['row1':'row3'])		#访问第一、二、三行
```

1. `[]`内必须是是**单独行标签或行标签切片**
2. `[]`内用行号或行号切片均错误
3. `['row1':'row3']`表示访问第一行到第三行
4. 当只访问一行时，返回类型时`Series`，返回多行是`DataFrame`

```python
#方法三	按行号访问
a = np.arange(12).reshape(3,4)
aa = pd.DateFrame(a,index=['row1','row2','row3'],columns=['col1','col2','col3','col4'])
print(aa.iloc[1])		#访问第二行
print(aa.iloc[1:3])		#访问第二、三行
print(aa.iloc[[1,2]])		#访问第二、三行
print(aa.iloc[1,3])		#访问第二行第四列的元素
```

1. `[]`内可以是单独行号、多个行号、行号切片
2. 只访问一行时，返回值是`Series`，访问多行时是`DataFrame`



#### 按列索引

```python
#方法一	按列标签访问
a = np.arange(12).reshape(3,4)
aa = pd.DateFrame(a,index=['row1','row2','row3'],columns=['col1','col2','col3','col4'])
print(aa['col1'])
print(aa[['col1','col3']])
print(aa['col1':'col3'])
```

1. `[]`内必须是列标签，列号错误
2. `[]`可以是单独列标签、多个列标签，**但不可以是列标签切片**
3. 访问一列时返回类型是`Series`，访问多列时是`DataFrame`



#### 多重索引

```python
#方法一    先选择列再选择行
a = np.arange(12).reshape(3,4)
aa = pd.DateFrame(a,index=['row1','row2','row3'],columns=['col1','col2','col3','col4'])
print(aa['col1'].loc[['row1','row2']])	#访问第一列的第一、二行元素
print(aa['col1'].iloc[[1,2]])			#访问第一列的第二、三行元素

#方法二
print(aa.iloc[1,2])		#访问第二行第三列数据
```







## Panel

- 用来存储三维数据
- 通过pd.Panel()将数据变为Panel类型
- pd.Panel()输入参数类型有：   三维列表 、三维ndarray、 字典(其值是DataFrame)