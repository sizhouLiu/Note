# numpy学习笔记

#### 创建numpy数组

```python
t1 = np.array([[1,1,1,],[1,1,1,]])
t2 = np.array(range(100))
t3 = np.arange(100)
```

![image-20230712200656041](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230712200656041.png)

```python
np.array.dtype 是数据类型
np.astype(数据类型) 修改数据类型
```

```python
np.shape 查看数组 形状
np.reshape 更改数组形状
t3 = t1.flatten() 变为一维数组
print(t3)
[1 1 1 1 1 1]
```

```python
数组也可以做加减乘除
两个数组做加减时行列必须有一个是相同的否则无法进行运算
```

#### numpy的索引与切片

```python
取行：
t2[x] x是想要取的那一行的索引
取列：
t2[,x] x是想取的那列
取多行、列：
t2[[1,2,3]]
取连续多行
t2[2:,]
```

#### numpy修改数据

```python
t3[t3>1] = 3
[3 3 3 3 3 3]
t3.where(t<2, ,) 三元运算符 如果为真则取值一若为假取二
t3.clip(10,18) 小于10的换成10 大于18的换18
```

#### 数组拼接：

```
np.vstack((t1,t2)) ->竖直拼接
np.hstack((t1,t2)) ->水平拼接
```

#### 数组行列交换

```python
t[[1,2],:] = t[[2,1],:] 行交换
t[:,[1,2]] = t[:,[2,1]] 列交换
```

```
np.argmax(t,axis = 0)
np.argmin(t,axis = 1)
# axis 是轴
np.ones((2,3))
np.zeros((3,4))
#创建一矩阵和0矩阵
np.eye(10)创建N阶单位方阵
```

#### numpy随机数生成：

![image-20230712221926970](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230712221926970.png)

####  numpy中的nan与inf

1. 如果读取本地文件为float时，如果文件有缺失，就会出现nan。如果做了一个不合适计算如无穷大减去无穷大时也会出现nan
2. 在numpy中一个数字除以时会变为+-inf
3. nan与inf是float类型的数据

![image-20230714164922508](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230714164922508.png)

```python
np.count_nonzero(数组) 返回不为0的数的个数
由于nan != nan 所以
np.count_nonzero(t2!=t2)返回的就是nan的个数
# 注意计算的都是为TRUE地方的值
np.isnan(数组) #返回是nan的个数
```

```python
np.sum() #将所有数据求和
np.sum(axis) axis是轴 只对轴上的数据求和
t2.mean(axis) 求均值
t.median 中值
t.max 最大值
t.min 最小值
t.ptp 极差
t.std 标准差(方差)
```

![image-20230714200229438](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230714200229438.png)



```python
#将为nan的值替换为均值
for i in range(t1.shape[1]):
	temp_col = tl[:,i] #当前的一列
	nan_num = np.count_nonzero(temp_col!=temp_col)
	if nan_num!=0:#不为，说明当前这一列中有nan
		temp_not_nan_col = temp_col[temp_col==temp_col] #当前一列不为nan的array
		temp_col[np.isnan(temp_col)] = temp_not_nan_col.mean()
```

