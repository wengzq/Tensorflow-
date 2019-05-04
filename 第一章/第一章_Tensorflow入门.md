# 第一章 Tensorflow入门

## 1.1.计算图

计算图是Tensorflow中最基本的概念，Tensorflow中所有的计算都会被转化为计算图上的节点。Tensorflow是一个通过计算图的形式来表达计算的编程系统。Tensorflow中的每一个计算都是计算图上的一个节点，而节点之间的边描述了计算之间的依赖关系。

![1555928079863](C:\Users\Soorop\AppData\Roaming\Typora\typora-user-images\1555928079863.png)

### 1.1.1.计算图的使用

可以使用tf.get_default_graph()来获取当前的计算图。事实上Tensorflow中的计算图不仅仅可以用来隔离张量和计算，它还提供了张量和计算的管理机制。这意味着计算图可以通过tf.Graph.device函数来指定运行计算的设备。

g = tf.Graph()



## 1.2.Tensorflow数据模型-----张量

张量的理解：零阶张量表示标量（scalar），也就是一个数；第一阶张量为向量（vector）;第n阶张量可以理解为一个n维数组。但张量在Tensorflow中的实现不是直接采用数组的形式，它只是对Tensorflow中计算结果的引用。即它保存的是如何得到这些数字的计算过程。

实际上一个张量主要保存了三种属性：name, shape, type。其中name为唯一标识符。当然在定义张量时，可以使用dtype来修改张量的type。

### 1.2.1.张量的使用

张量的两类用途分别是：**对中间的计算结果的引用；获得计算的结果。**



## 1.3.Tensorflow运行模型：会话

会话拥有并管理Tensorflow程序运行时所有的资源，常用的会话管理模式如下：

​				with tf.Session() as sess:
  					  sess.run(....)

## 1.4.神经网络参数与Tensorflow变量

Tensorflow使用tf.Variable()的方法保存和更新神经网络中的参数。我们可以使用该方法对某个变量进行初始化：

`weight = tf.Variable(tf.random_normal([2, 3], stddev = 2))`

其中stddev为标准差，这段代码调用了Tensorflow变量声明函数，并在该函数中给出了初始化方法。其会产生一个2*3的矩阵，其中元素均值为0，标准差为2的随机数矩阵。以下为随机数生成器：

| 函数名称              | 随机数分布                                                   | 主要参数                              |
| --------------------- | ------------------------------------------------------------ | ------------------------------------- |
| tf.random_normal()    | 正态分布                                                     | 均值，标准差，取值类型                |
| tf.truncated_normal() | 正态分布，若随机出来的值偏离平均值超过2个标准差，那么这个值被重新随机。 | 均值，标准差，取值类型                |
| tf.random_uniform()   | 均匀分布                                                     | 最小、最大取值，取值类型              |
| tf.random_gamma()     | Gamma分布                                                    | 形状参数alpha，尺度参数beta，取值类型 |

 以下为常量声明方法：

| 函数名称      | 功能                       |
| ------------- | -------------------------- |
| tf.zeros()    | 产生全0数组                |
| tf.ones()     | 产生全1数组                |
| tf.fill()     | 产生一个全部为给定数的数组 |
| tf.constant() | 产生一个定值的常量         |

同样也支持通过其他变量的初始值来初始化新的变量：

`w2 = tf.Variable(weights.initialized_value())`

注意，在Tensorflow中，一个变量的值在被使用之前，这个变量的初始化过程需要明确的调用。

`w1 = tf.Variable(tf.random_normal((2, 3), stddev = 1, seed = 1))`

`sess.run(w1.initializer)#w1初始化完成`

事实上，我们常用一下语句来对全局的变量进行初始化：

`sess.run(tf.globla_variables_initializer())`

注意，变量有两个属性：**维度（shape）和类型**，其中维度是可以通过设置参数validata_shape = False来提供修改，而类型在定义后是不可以修改的。

## 3.5.通过Tensorflow训练神经网络模型

### 3.5.1.反向传播算法（backpropagation）

反向传播过程：1.首先选取一小部分训练数据(batch)；2.这个batch的样例会通过前向传播算法得到神经网络模型的预测结果；3.基于预测值和真实值之间的差距，反向传播算法会相应的更新神经网络参数的取值。

###  3.5.2.使用placeholder方法来实现数据传输

placeholder相当于定义了一个位置，这个位置中的数据在程序运行时再进行指定。样例：

`x = tf.placeholder(tf.float32, shape = (1, 2), name = 'input')`

`sess.run(y , feed_dict = {x: [[0.7, 0.9]]})`

其中feed_dict是一个字典，在字典中给出需要用到的取值。

## 3.6.神经网络训练三部曲

1.定义神经网络结构和前向传播算法；

2.定义Loss function并选择相应的反向传播优化办法；

3.生成会话，开始训练。