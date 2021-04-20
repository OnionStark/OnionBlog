---
title: 算法课笔记
date: 2020-08-26 15:06:58
categories: CS基础
---

# 算法分析

## 输入大小

包括**时间**和**空间**复杂度

输入大小分为最好、平均、最坏情况

## 算法增长率（Order of growth）

### O(g(n))

算法f(n)的增长率不会比g(n)快

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-25-56-0e1e17a8b5c8b68f7b270383fc7a6ec2-image-20200809114123290-2fd243.png" alt="image-20200809114123290" style="zoom:50%;" />
$$
\exist c\ and\ n_0 \geq 0 \qquad f(n)\leq cg(n)\qquad \forall n\geq n_0
$$


### Θ(g(n))

算法f(n)的增长率与g(n)一致

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-05-af89571174d125d9c1fcd5e1603ec2e9-image-20200809114655209-39b2cb.png" alt="image-20200809114655209" style="zoom:50%;" />
$$
\exist c\ and\ n_0 \geq 0 \qquad c_1g(n)\leq f(n)\leq c_2g(n)\qquad \forall n\geq n_0
$$


### Ω(g(n))

算法f(n)的增长率至少比g(n)快

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-09-ccef9842ee0a5e490b496dcc9b40868d-image-20200809114645669-ee37d2.png" alt="image-20200809114645669" style="zoom:50%;" />
$$
\exist c\ and\ n_0 \geq 0 \qquad 0\leq cg(n)\leq f(n)\qquad \forall n\geq n_0
$$

### o()和ω()

o()类似<， ω()类似>， 

O()类似<=, Ω()类似>=, Θ()类似=

### 常见公式的增长率

![image-20200809120604343](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-16-34265d982a7bcdb913e72d20b39f7377-image-20200809120604343-beeda5.png)

### 增长率量级比较

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-19-73c78787375050fc2791fd797480a50e-image-20200809140453002-6e0cc1.png" alt="image-20200809140453002" style="zoom:150%;" />

### 算法：汉诺塔问题

算法伪代码：

![image-20200809140609763](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-28-430465845b158631f6e037cd0991f8c5-image-20200809140609763-8883bc.png)

递归次数：
$$
M(n) = 2M(n-1)+1  \qquad M(1)=1
$$
![image-20200809140947909](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-33-a19250d02ff30a1399c27dcc76bf6198-image-20200809140947909-928cf0.png)
$$
M(n) = 2^n -1
$$


### 算法：求十进制整数的二进制位数

![image-20200809141255042](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-35-49395360dffed41441f9187817d4aada-image-20200809141255042-96d7e1.png)

# 分治算法

## 暴力枚举

### 算法：冒泡排序

输入：待排序的序列<a1,a2,...,a3>

输出：排好序的序列

![image-20200809141731483](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-41-b55e3b9b8d9884db5e093c108e9367bd-image-20200809141731483-29cb4c.png)

![image-20200809141841847](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-44-d86a103ade695c64cf12893fd4231a7d-image-20200809141841847-95205f.png)

思路：把数组中的最大的数不断排在最后面

### 算法：最近点对问题

描述：在一组n个点（在二维笛卡尔平面）中找到两个最近的点

![image-20200809142110737](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-48-dd1064d134be4a34a761d473f16d7bb1-image-20200809142110737-cfe193.png)

思路：比较每个点的距离，不断刷新最小距离

## 分治算法思路

最著名的算法设计策略：

1. 将问题实例分成两个或多个较小的实例

2. 递归求解较小的实例

3. 通过组合这些解决方案获得原始（较大）实例的解决方案

   ![image-20200809142405435](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-26-54-e3b5fdc49bc84d940c7d3ea2f6f77949-image-20200809142405435-a97ab1.png)
   $$
   <Empty \space Math \space Block>
   $$

### 一般分治算法复杂度

分治算法，就是把一个大的问题分为很多个形式相同的子问题，把问题规模缩小。假使，最初的问题规模是N,这些小的子问题的个数为a，子问题的规模是n / b，分解或者合并的复杂度表示为f( n ),那么总的时间复杂度就可以表示为
$$
T(n)=aT(n/b)+f(n)\qquad where f(n)\in\Theta(n^d),\quad d\geq0
$$
很明显，这是一个递推的表达式，为了便于计算，赋初值，子问题规模为 n / 2:

![image-20200809143908694](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-00-189862adc18e71f411a4040b1dbee559-image-20200809143908694-2b7e27.png)

### *主定理

$$
If\quad a<b^d, \quad T(n)\in\Theta(n^d)\\If\quad a=b^d, \quad T(n)\in\Theta(n^dlogn)\\If\quad a>b^d, \quad T(n)\in\Theta(n^{logb^a})\\
$$

**证明：**

我们抛开具体的问题，用这几个参数推导一下计算量。

把递归问题每一层的问题数目，计算量都一一列出，可以得到如下公式

![image-20200810113723691](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-07-48920b5a658e36fe1cdc1fd3e4b30fa8-image-20200810113723691-606be1.png)

最后的公式是一个累加函数

![image-20200810113744106](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-11-bc08e04e3891a47f4437049a0c39c668-image-20200810113744106-90cbab.png)

一看结构，是一个等比数列，公比为 ![image-20200810113821674](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-13-55f33598fe386bffec9d67b8fbc7e7ee-image-20200810113821674-0a3ce5.png)

等比数列的求和公式为![image-20200810113837793](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-17-cdd9fb911bcb4b3781dee61d5d72a102-image-20200810113837793-f3d288.png)

其实分析时间复杂度，比较简单，对于等比数列来说，如果公比q<1, 那么第一项最大，其他项会依次减少，最终的时间复杂度就是![image-20200810113852988](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-21-624c99aa75b240b1049110c946d4c25a-image-20200810113852988-a4c912.png)



所以，当公比![image-20200810113909643](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-25-ad5a457bfba3f4ad4524766aa3aae647-image-20200810113909643-af8083.png)

的时候，整个算法的时间复杂度由第一项决定，

![image-20200810113923193](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-28-a20ddf13420181ffbcec658db5f745d0-image-20200810113923193-2139b7.png)

第二种情况，而如果反过来，公比q>1, 那么最后一项最大，最终的时间复杂度由最后一项![image-20200810113957302](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082615-18-45-c0dacae0c77cdbcb624f8d9c9ad8ecce-image-20200810113957302-c65440.png)统治（dominate）

![image-20200810114004389](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-31-8f56ff7b63783073919cf65b02ec49b8-image-20200810114004389-1b8a95.png)

整个算法的时间复杂度由最后一项决定

![image-20200810114017671](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-41-4a1f2a4e4e5e4a6228f1f441773eb794-image-20200810114017671-a1d802.png)

注：这里面为什么 ![image-20200810114030215](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-46-1103d9550c2d8d7f8e1e730ac2ce9e41-image-20200810114030215-6833e2.png)呢？化简方式如下

![image-20200810114045495](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-51-76920a6fb9fb61b14fffa2e90dfaf683-image-20200810114045495-7cc251.png)

而![image-20200810114059184](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-27-56-cd8cdb2b4b3a38d3633737ef805ef574-image-20200810114059184-29df89.png)，所以![image-20200810114115833](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-02-83266cf299170e0319d7c8825dc41b57-image-20200810114115833-d5a312.png)



最后一种情况，公比q =1， 每一项都一样大，这样子就要求和了，所以

![image-20200810114223386](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-06-196d5d0a1d13e9d1eb6759562b4ff912-image-20200810114223386-f94698.png)

还有一个更快速的小技巧，能够不用记以上的公式就能进行推导

![image-20200810114301192](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-08-3215b9986676ce66c830efc23f475801-image-20200810114301192-c021a9.png)

简单看一下第一层和第二层，比较下这两层的工作量，如果每层工作量是一样（如上图），则对应公比为1的情况，直接![image-20200810114320808](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-13-3762ea661193867f599609815b3e849a-image-20200810114320808-ee1fcf.png)

如果每层第一层工作量比第二层多（如下图），对应公比大于1的情况，直接![image-20200810114356188](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-19-dbee33e5f576069e0b287dbb45e1d7c6-image-20200810114356188-504c07.png)

![image-20200810114417891](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-21-f048b54f481efd49a66200a31fec3042-image-20200810114417891-6823f4.png)

如果第一层工作比第二层少，对应于公比小于1的情况，直接![image-20200810114436705](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-26-61b9ca9a6a657be8c057ebbfd5d8c5d7-image-20200810114436705-c21154.png)

## 分治算法例子

### 归并排序

**算法思路：**

* 将数组A[0..n-1]等分成数组B和C

* 对数组B和C递归排序

* 将排好序的数组B和C合并到数组A中：

  * 重复以下操作，直到其中一个数组中没有元素保留：

    * 比较数组剩余未处理部分中的第一个元素
    * 将两者中较小的一个复制到A中，同时递增表示该数组未处理部分的索引


  * 处理完其中一个数组中的所有元素后，将其余未处理的元素复制到A中。

分阶段伪代码：

![image-20200809145040244](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-33-a418771701dce92bd86f9a633510ebed-image-20200809145040244-b49ff5.png)

合并阶段伪代码：

![image-20200809145203040](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-36-e2423502ab88511ce525b32be0de0449-image-20200809145203040-fafd93.png)

**算法图解：**

![image-20200809145406864](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-40-abd3608d25979642355353e42fe801ec-image-20200809145406864-1df336.png)

**算法分析：**

![image-20200809145559060](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-43-206aec03e864fdeefc9e7a30a37cf289-image-20200809145559060-047b4e.png)

**递归复杂度计算：**

![image-20200809145723055](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-46-0e440602875b8e5216504e26ec973795-image-20200809145723055-ab0d48.png)
$$
当n/2^i=1时，有i=logn\\此时T(n)=bn+bnlogn
$$
任何情况下，复杂度为Θ(*n* log *n*) 

### 快速排序

**算法思路：**

* 选择一个pivot（分区元素）——这里选择第一个元素

* 重新排列数组，使前s个位置的所有元素都小于或等于pivot，其余n-s位置的所有元素都大于或等于pivot

  ![image-20200809151254283](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-53-b0d348ecaa23c377299d742d6599d997-image-20200809151254283-76d89c.png)

* 将privot与第一个子序列中的最后一个元素交换

* 递归排序两个子序列

**算法伪代码：**

![image-20200809151508250](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-28-56-84bbf2bd5725f37634d7d0281fbac651-image-20200809151508250-16ee2e.png)

**算法图解：**

![image-20200809152101856](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-06-1b58ab514a2f0570109051e226395e40-image-20200809152101856-ee16c3.png)

**算法分析：**

* 最坏情况：分区总是不平衡

  T(1) = Θ(1)

  T(n) = T(n - 1) + Θ(n)

  总复杂度：T(n) = Θ(n^2)

* 最好情况：分区总是平衡

  T(n) = 2T(n/2) + Θ(n)

  总复杂度：T(n) = Θ(n log n) 

* 平均情况：数组中所有元素都有可能被选为pivot

  复杂度计算：<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-14-1d1a6e4ff12289dbc1b8955eb2672964-image-20200809153348693-fdc596.png" alt="image-20200809153348693" style="zoom:80%;" />

![image-20200809153518021](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-17-ace71b29e281a76066aef502783ec0c3-image-20200809153518021-fda1fc.png)

​	归纳假设法：

![image-20200809153627084](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-20-961afb00a0dfad8b9dd52dd4c4dfd2e6-image-20200809153627084-a1fc55.png)

![image-20200809153700108](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-25-730a76926d88762a478ee042bf4f4667-image-20200809153700108-20d696.png)

所以平均情况下，快速排序复杂度：T(n) = O(n log n)

**总结：**

❖ Best case: split in the middle — Θ(*n* log *n*) 

❖ Worst case: sorted array! — Θ(*n^2* ) 

❖ Average case: random arrays — Θ(*n* log *n*) 

**优化：**

* pivot的选择：选择分区的中位数
* 分区较小时可以选择插入排序
* 消除递归

另一种类型的快排伪代码：

![image-20200809154141977](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-32-0781a49357e774dc128ef57fdbafe323-image-20200809154141977-cff6f2.png)

将比Aq小的元素排到前面，最后s指向的就是最后一个比Aq小的索引，因此第4步交换Aq和As

### 大整数乘法

**描述：**将两个n位整数相乘，整数用数组表示，例如：

A = 12345678901357986429 

B = 87654321284820912836

**算法思路：**

A*B ,当A=2135, B=4014时
$$
A = (21*10^2+35),\quad B=(40*10^2+14)\\ 所以，A*B=(21*10^2+35)*(40*10^2+14)\\=21*40*10^4+(21*14+35*40)*10^2+35*14
$$
通常情况下，
$$
If\quad A=A_1A_2, B=B_1B_2， A_1,A_2,B_1,B_2是n/2位整数，\\A*B=A_1*B_1*10^n+(A_1*B_2+A_2*B_1)*10^{n/2}+A_2*B_2
$$
算法复杂度：M(n)=3M(n/2),	M(1)=1
$$
M（n)=3^{log_2n}=n^{log_23}\approx n^{1.585}
$$




### 最近点对问题

**算法步骤：**

1. 用一条垂直线x=c将给定点分成两个子集S1和S2，使一半的点位于左侧或线上，一半点位于右侧或线上。

![image-20200809161609800](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-44-8073175de9964ed1d777e7f2a5b7fa69-image-20200809161609800-dc9bef.png)

2. 递归为左子集和右子集查找最近的点对

3. 令d=min{d1,d2},我们可以只关注宽度为2d的对称垂直条带中的点，尽可能地将注意力集中在最接近的对上。设C1和C2分别是左子集S1和右子集S2中位于该垂直条带中的点的子集。C1和C2中的点按y坐标的递增顺序存储，在下一步的执行过程中通过合并来保持y坐标。

4. 对于C1中的每个点P(x,y)，我们检查C2中最接近P的点，其距离可能小于d。这样的点不会超过6个（因为d≤d2）！最坏的情况如下图：

   ![image-20200809162553305](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-47-e933af43ff459e5a90246cbcc8c4e9bd-image-20200809162553305-ad9807.png)

对于p点来说，C2中的每个点都要检查

**算法效率分析：**T(*n*) = 2T(*n*/2) + M(*n*), 	 M(*n*) ∈O(*n*) 

根据定理，T(*n*) ∈ O(*n* log *n*)

### 基于分治的凸包算法

**凸包的定义：**在一个实数向量空间V中，对于给定集合X，所有包含X的凸集的交集S被称为X的凸包。

X的凸包可以用X内所有点(X1，...Xn)的线性组合来构造。

在二维欧几里得空间中，凸包可想象为**一条刚好包住所有点的橡皮圈**。

用不严谨的话来讲，给定二维平面上的点集，凸包就是将最外层的点连接起来构成的凸多边型，它能包含点集中所有的点。

例子：假设平面上有p0~p12共13个点，过某些点作一个多边形，使这个多边形能把所有点都“包”起来。当这个多边形是凸多边形的时候，我们就叫它“凸包”。如下图： 

![image-20200809163535897](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-53-fc7401d1660fd57f3a5a141dabaa935f-image-20200809163535897-0e11ce.png)

**Graham扫描法**

![image-20200809165157328](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-29-56-fde195f47f09ce451987bfc4160f1d7c-image-20200809165157328-1de646.png)

1. 把所有点放在二维坐标系中，则纵坐标最小的点一定是凸包上的点，如图中的P0。
2. 把所有点的坐标平移一下，使 P0 作为原点，如上图。
3. 计算各个点相对于 P0 的幅角 α ，**按从小到大的顺序对各个点排序**。当 α 相同时，距离 P0 比较近的排在前面。例如上图得到的结果为 P1，P2，P3，P4，P5，P6，P7，P8。我们由几何知识可以知道，**结果中第一个点 P1 和最后一个点 P8 一定是凸包上的点**。 
   （以上是准备步骤，以下开始求凸包） 
   以上，我们已经知道了凸包上的**第一个点 P0 和第二个点 P1**，我们把它们**放在栈里面**。**再把P2放在栈里**，现在从步骤3求得的那个结果里，把 P2 后面的那个点拿出来做当前点，即 P3 。接下来开始找第三个点：
4. 若**栈顶的两个点**和**当前的点pi**这三点连线的方向（按下标从小到大连线）向**顺时针方向偏转**，表明栈顶的点是一个凹陷处，**应删除，则栈顶元素出栈**。**并将pi压入栈中**。如果向**逆时针方向偏转，则直接压入栈中。**
5. 重复步骤4，直到遍历完成，在栈中的点就是凸包上的顶点。

![这里写图片描述](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-05-585ca00d0407c525d7ff941bc7c46078-20150530145453912-8d6a02)

**算法步骤：**

![image-20200809171248005](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-08-345fd4f7b09e6faf572c2964b59c3911-image-20200809171248005-27541d.png)

三个序列：

![image-20200809171819189](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-12-8eae751b14ab68770522b482214da240-image-20200809171819189-b9347a.png)

**算法复杂度：**T(n)*=2*T(n/2) +*O(n)*=*O(n log n)*

### 快速凸包算法

1. 找出最左点与最右点，连线，所有点分为上半部与下半部。（上半部与下半部分开求解）

2. 处理上半部，找出上半凸包：

3. 找到距离底线最远的点，形成一个三角形区域。（如果有许多点，就找最靠近左端点的那一点。避免凸包上多点共线。）

4. 运用叉积运算，找出三角形外部的点，分为左、右两份。

   * 先求出三个向量MA,MB,MC. 

   * 计算MA X MB,MB X MC,MC X MA (X表叉乘)

   * 如果此三组的向量叉乘的结果都是同号的（或都正，或都负），即方向相同的，则说明点M在三角形每条边的同侧，即内部。否则必在外部！

   * 具体示例：A(0,3) B(0,0) C(3,0)     

     M(1,1)
     MA (1,-2)
     MB (1,1)
     MC (-2,1) 

     MA X MB = (1×1-(-2)×1) = 3
     MB X MC = (1×1-1×(-2)) = 3
     MC X MA = (-2×(-2)-1×1) = 3

5. 至于三角形内部、三角形上的点，不会是凸包顶点，尽数剔除之。

6. 左、右两份分别递归求解，直到找出上半凸包。（三角形的两边，分别做为左、右两份的底线。）

7. 处理下半部，找出下半凸包，重复3-6步。

**算法复杂度：**

最差情况：Θ(n^2) （每次找到的点都在线段的一边）

平均情况：Θ(nlogn)	如果点集中的点是随机分布的，则快速凸包算法的平均效率是线性的

### 二维极大值点问题

**极大值点的定义：**在二维平面上，如果x1>x2且y1>y2（注意这个条件是且，如果没有其他点的x和y同时大于该点，则该点属于极大值点集），我们说一个点（x1，y1）支配（dominates）（x2，y2）。如果没有其他点支配它，这个点被称为极大点，在给定一组n个点的情况下，求极大值的问题就是求所有的极大值点。

**算法步骤：**

1. 如果S只包含一个点，则将其作为极大值点返回。否则，找到一条垂直于x轴的线L，它将点集分成两个子集SL和SR，每个子集由n/2个点组成。

2. 递归求SL和SR的极大值点。

3. 将SL和SR的极大值点投影到L上，并根据它们的y值对这些点进行排序。对投影进行线性扫描，如果SL的y值小于某个SR极大值点的y值，则丢弃每个SL的极大值点

   ![image-20200810103931720](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-23-e7c628c55d3b455f55e324acf3e4efed-image-20200810103931720-276c95.png)

**算法复杂度分析：**

T(n) = 2T(n/2) + O(n) + O(nlogn), O(n)是第三步比较的复杂度，应该是线性扫描那一步，O(nlogn)是划分L的复杂度？

令n=2^k

T(n) = O(nlogn) + O(nlog²n) = O(nlog²n)

优化：按x坐标升序对点进行排序，这是一个预处理，有利于划分。

预处理排序的代价是O（n logn）。因为划分阶段成本为O（1），合并两个解决方案的成本为O（n）

复杂度变为：T(n) = 2T(n/2) + O(n)

根据主定理，T(n) = O(n log n)

### 维诺图

**维诺图的定义：**又叫泰森多边形或Dirichlet图，它是由一组由连接两邻点直线的垂直平分线组成的连续多边形组成。

（1）每个V多边形内有一个生成元； 
（2）每个V多边形内点到该生成元距离短于到其它生成元距离； 
（3）多边形边界上的点到生成此边界的生成元距离相等； 
（4）邻接图形的Voronoi多边形界线以原邻接界线作为子集。

![image-20200810115943534](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-28-eaef83c5dc09cfc89967a1145c1d46de-image-20200810115943534-e88d04.png)

**算法步骤：**

1. 如果S只包含一个点，则返回；
2. 求一条垂直于x轴的中线L，它将点集分成两个子集SL和SR，使得SL（SR）位于L的左（右），且大小相等；
3. 递归构造Voronoi图，用VD（SL）和VD（SR）表示这些Voronoi图；
4. 构造轮廓线Contour，介于左边的子图和右边子图之间的边界，它到左侧site和右侧site的距离是相等的，它具有等距性。因此Contour上的任何一个点相对于最后要合成的维诺图来说必然是某一条edge上的点，所以Contour必然是由最终的Voronoi图上的若干条edge前后依次相连而构成的一条裂缝。它是同时距离SL中的一个点和SR中的一个点最近的点的轨迹。丢弃位于Contour右侧的所有VD（SL）边和HP左侧的所有VD（SR）边。结果为VD(S)。

![image-20200810122045836](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-32-c782b8c76f683b25816b2a8554e8a8d5-image-20200810122045836-0b4666.png)

第4步详细算法：

![image-20200810122307465](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-36-afe0145f741f6bdbb58feff77f0c3405-image-20200810122307465-15cf82.png)

步骤1中，因为他们各自的维诺图已知，求凸包只需要扫描一次所有的点，判断这个点的**维诺图区域是否封闭**就知道这个点是否在凸包中。这里需要Θ(n)的时间。

步骤2中，分别在各自凸包中找出最高的点PL和PR，求出连线的垂直平分线BS，这条垂直平分线就是轮廓线的起始部分。（Pa和Pc属于SL，图中应该标错了）

步骤4中，在SL和SR中自上而下选取点Pi以及属于点Pi的射线，然后求出BS与射线之间的交点（更新轮廓线）。这条射线如果来自左边则PL=Pi，来自右边则PR=Pi重新计算BS。

**算法复杂度分析：**

* 构造SL和SR的凸包需要O(n)
* 合并两个维诺图需要O(n)，合并的过程，主要利用了Contour在Y方向上的单调性，使得只需要自上而下遍历一次凸包中的点就能求出轮廓线。合并的效率为O(n)
* 总的复杂度：T(n)=2T(n/2) +O(n)=O(n log n)

# 减治算法

## 减治算法思路

1. 将问题实例缩减为同一问题的较小实例

2. 求解较小的实例

3. 扩展较小实例的解，得到原实例的解

   

减治可以自上而下或自下而上实现，也称为归纳法或增量法。

分为三种类型：

1. 按常数减治（通常为1）：
   * 插入排序
   * 图遍历（DFS和BFS）
   * 拓扑排序
   * 生成置换、子集的算法

2. 按常数因子减治（通常减半）：
   * 二分查找
   * 平方求幂
   * 俄罗斯乘法

3. 可变减治：
   * 欧几里得算法
   * 分区选择
   * Nim-like games

## 减治算法的区别

考虑计算a^n

* 暴力法：a^n = a×a×a×a...×a
* 分治法：a^n =a^(n/2)×a^(n/2)
* 每次减治1：a^n=a^(n-1)×a    （与暴力有什么区别？当然，生成的算法中没有，但是可以用两种不同的方法来实现它）
* 按常数因子减治：a^n=(a^(n/2))^2    (与分治法的区别在于分治法重复计算a^(n/2))

## 减治算法例子

### 插入排序

**算法思路：**要对数组A[0..n-1]进行排序，递归地对A[0..n-2]排序，然后在排序后的A[0..n-2]中的适当位置插入A[n-1]

![image-20200810125400841](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-48-261b48ce7f2df14cfd782f35c9bf82d4-image-20200810125400841-0345be.png)

**算法伪代码：**

![image-20200810125413968](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-52-6908f9b3c9c43259b1226a928334abc5-image-20200810125413968-c7e23f.png)

把A[i]的值插入到0~i-1中的适当位置，j用于在0~i-1中找位置，当A[j]比A[i]大时，不断将A[j]向后移，直到找到了第一个小于A[i]的位置，把A[i]放到A[j]的下一位。

**算法复杂度分析：**

最差情况：Cworst(n) = n(n-1)/2 ∈ Θ(n^2)    (每一次循环j都要遍历到第一位，因此n-1+n-2...+1)

平均情况：Cavg(n) ≈ n^2/4∈ Θ(n^2)

最好情况：Cbest(n) = n - 1 ∈ Θ(n) 

### DFS

**算法描述：**

使用栈来保存顶点：

* 第一次到达顶点时，顶点入栈
* 当一个顶点没有相邻的未访问顶点时，出栈

**算法伪代码：**

![image-20200810131317333](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-30-56-683f0a86006806e8196efbd4d0a24102-image-20200810131317333-1fc858.png)

**算法示例：**

![image-20200810131913355](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-00-79eca2085d06644af881bd144a96f011-image-20200810131913355-a3e9d1.png)

DFS遍历过程中，栈的情况：

![image-20200810131940825](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-03-824f579bd45b9416cd423f6ad64db2a7-image-20200810131940825-dfc996.png)

左边的数字表示入栈的顺序，右边的数字表示出栈的顺序。

DFS遍历有向图时，根据在图G上进行深度优先搜索所产生的深度优先森林，可以把图中的边分为四类：
(1)树边：遍历中遇到新顶点的边
(2)反向边：搜索树中，子结点指向祖先节点的边
(3)正向边：搜索树中，祖先结点指向子节点的边
(4)交叉边：是指存在于同一棵深度优先树中的两个顶点之间，条件是其中一个顶点不是另一个顶点的祖先。交叉边也可以在不同的深度优先树的顶点之间。

![image-20200810135529520](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-06-4e93d5acd2797f2e26ce4c1b5acf62c8-image-20200810135529520-792d0a.png)

**算法复杂度：**

对于邻接矩阵表示的图：Θ(V^2)

对于邻接表表示的图：Θ(|V|+|E|)

### BFS

BFS使用队列而不是栈，类似于逐层树遍历

**算法伪代码：**

![image-20200810140020299](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-10-26f052eedcf2051e0acd90ce1f464916-image-20200810140020299-80274d.png)

**算法步骤：**

1. 准备工作：创建一个visited数组，用来记录已被访问过的顶点；创建一个队列，用来存放每一层的顶点；初始化图G。

2. 从图中的v0开始访问，将的visited[v0]数组的值设置为true，同时将v0入队。

3. 只要队列不空，则重复如下操作：

  (1)队头顶点u出队。

  (2)依次检查u的所有邻接顶点w，若visited[w]的值为false，则访问w，并将visited[w]置为true，同时将w入队。

**算法复杂度：**

对于邻接矩阵表示的图：Θ(V^2)

对于邻接表表示的图：Θ(|V|+|E|)

### Russian Peasant Multiplication

**算法描述：**计算两个正整数的乘积，可通过基于以下公式的减半算法求解。

![image-20200810141326264](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-19-c1984c1a1b4a12eae946f7bfb038c895-image-20200810141326264-7c8585.png)

算法示例：

![image-20200810141444380](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-22-d6314ea14c2247b379fa67d88b28ec1c-image-20200810141444380-b3c90e.png)

### Fake-Coin Puzzle

问题描述：有n种外观相同的硬币，其中一个是假的。有一个天平秤，但不能显示重量；天平可以分辨出两套硬币的重量是否相同。设计一种有效的假币检测算法。假设已知假币比真币轻。

算法思路：

1. 减治法（减半）：

   把n个硬币分为两堆，每堆n/2个，每次称一堆。

   易见   W(1)=0，W(n)=W(n/2)+1
   解得  W(n)= logn

    T(n)=O(logn)

2. 减治法（减n/3）：

   把n个硬币分为三堆，每堆n/3个，每次称任意两堆。
   易见   W(1)=0
             W(n)=W(n/3)+1
   解得  W(n)= log3n

   ​     T(n)=O(log3n)
    结果比减半法更好。

### Selection Problem

问题描述：在n个数的列表中找到第k个最小的元素

算法思路：

1. 基于排序的算法：排序并返回第k个元素的效率（如果按mergesort排序）：Θ（nlog n）

2. 一种更快的算法基于使用列表的快速排序分区。设s是由一个分区得到的分割位置：

![image-20200810142526690](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-28-42fdbeeeabe8f9e81b48e55c10c8ff99-image-20200810142526690-b03b80.png)

假设列表的索引从1到n：

* 如果s=k，问题就解决了；

* 如果s>k，则查找的第k个最小元素在左边；
* 如果s<k，在右边寻找第（k-s）个最小元素。
  注：算法可以简单地持续到s=k。

示例：

![image-20200810142811531](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-32-44c901b170a931255e5f17c23429bed1-image-20200810142811531-1d738d.png)

### One-Pile Nim

问题描述：有n个筹码。两名玩家轮流从牌堆中取出至少1个至多m个筹码。（每一步的筹码数目可能会有所不同）拿走最后一个筹码的玩家是赢家。判断谁会赢得这场比赛——如果两位选手每一步都是最优解，那么是先手赢还是后手赢？

**算法思路：**

可以倒着往前推，如果有n个筹码，每次至多拿m个筹码，如下图：

![image-20200810143641807](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-36-46cfee68b8941d0e46dca3da5ad39f62-image-20200810143641807-90a819.png)

顶点表示剩余的筹码数，粗体的箭头表示后手玩家的操作，从图中可以看出后手玩家必赢。

**总结：**第一个移动的玩家在n不是5的倍数时获胜（更普遍地说不是m+1的倍数）；获胜的动作是每次都拿n mod 5（或n mod（m+1））数量的筹码。

### 拓扑排序

**问题描述：**在建模中出现了许多涉及先决条件约束的问题（构建项目、文档版本控制），有向无环图（DAG）的顶点可以线性排序，这样对于每条边，其起始顶点列在其结束顶点之前（拓扑排序）。有向无环图也是能否拓扑排序的必要条件。

**算法思路：**

1. 基于DFS的拓扑排序算法

   * 在任一有向无环图中，必然存在出度为0的顶点。否则，每个顶点都至少有一条出边，这意味着包含环路。

   * 在对有向无环图的DFS搜索中，首先因访问完成而转换至VISITED状态的顶点m，其出度必然为0。

基于上述两条特性，我们可以得出结论：DFS搜索过程中各顶点被标记为VISITED的次序，恰好（按逆序）给出了原图的一个拓扑排序。

2. Source Removal算法

   首先要找到任意入度为0的一个顶点，删除它及所有相邻的边，再找入度为0的顶点，以此类推，直到删除所有顶点。顶点的删除顺序即为拓扑排序。

**算法复杂度：**与DFS算法一样

### 连接点

**问题描述：**

* 设G=（V，E）为连通无向图
* 连接点的定义为：删除该点（连同该点相关的边）会导致图不连通。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-40-7de95b01dbe615c44cee5a07e192141d-image-20200810145847357-8f2b95.png" alt="image-20200810145847357" style="zoom:50%;" />

**算法思路：**

* 为了找到无向图的连接点，我们将调用DFS并使用DFS树结构来帮助我们找到连接点

示例：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-43-3d6e5f3b68eeed74738a2999e087e6d2-image-20200810150056404-c095d0.png" alt="image-20200810150056404" style="zoom:50%;" />

* 叶节点不会是连接点，因为叶节点在DFS树中没有任何子树，删除叶不会使树断开连接。图中为g、f、j
* 当根节点有两个子节点时，根节点是连接点
* 对于非叶节点，如果其子树中的节点**没有到该节点祖先的反向边**，那么该节点是连接点，如下图中的u

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-46-fbb99714a0eae2d68a43251522b362f0-image-20200810150757882-84509f.png" alt="image-20200810150757882" style="zoom:50%;" />

前两点很容易去验证，第三点如何验证？

* 如下图所示，内部节点“u”的祖先节点“v”的发现时间比u更小，即d[v]<d[u]
* 定义Low[u]为d[u]的最小值，{d[v]| w是u的后代节点，（w，v）是反向边}

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-31-58-ed75bb3e4d80cd7c583b4bcf345218bb-image-20200810151357751-3d01db.png" alt="image-20200810151357751" style="zoom:50%;" />

如何计算Low[u]？

思路：假如我们在dfs时访问到了u点，此时图就会被u点分割成为两部分。一部分是已经被访问过的点，另一部分是没有被访问过的点。**如果u点是割点，那么剩下的没有被访问过的点中至少有一个点在不经过u点的情况下，是无论如何再也回不到已经访问过的点了**。

假如到了u后，图中还有顶点v是没有访问过的点，如何判断v在不经过u的情况下是否还能回到之前访问过的任意一个点？

u是v的父亲，而之前访问过的顶点就是祖先。也就是如何**检测v在不经过父亲u的情况下还能否回到祖先**。那就是**对v再进行一次dfs，但此次遍历不经过u**，看**能否回到祖先，不能u即为割点**。因此需要一个**数组low来记录每个顶点在不经过父顶点时，能够回到的最小“时间戳”。**

![image-20200810161117693](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-32-15-ad79e165e8abc2f557516f7863e69b8a-image-20200810161117693-da200c.png)

对于某个顶点u，**如果存在至少一个顶点v（u的儿子），使得low[v]>=num[u]**，即不能回到祖先，那么u点为割点。

DFS遍历时，

* 初始状态：Low[u] = d[u]
* 访问到反向边(u, v)时：Low[u] = min{Low[u], d[v]}
* 访问到树边(u, v)时：Low[u] = min{Low[u], Low[v]}

示例：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-32-18-ccdca1596de00eb32a62394e5c0a3a58-image-20200810151719988-4054f5.png" alt="image-20200810151719988" style="zoom:50%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-32-22-c73498790d25a98c1b2e451415ec37f4-image-20200810151937623-8ac887.png" alt="image-20200810151937623" style="zoom:50%;" />

注（图中应该存在错误，low[c]应该=1，因为它的孩子节点f的low[f]=1）

### Generating permutations（全排列问题）

n个数有n!=n×(n − 1)×(n − 2)×...×2×1个不同排列

### 约瑟夫环问题

**问题描述：**一圈共有N个人，开始报数，报到M的人自杀，然后重新开始报数，问最后自杀的人是谁？

**算法思路：**

1. 递归解法：

   令J(n)表示最后一个自杀的位置，则J(n)有如下递推关系：

   J(1) = 1, 

   J(n) = 2J(n/2) – 1, 当n是偶数

   J(n) = 2J((n-1)/2) + 1, 当n是奇数

   复杂度：O(log n)

2. 暴力解法：

   循环遍历alive数组，当满足条件时，把alive数组设为0表示自杀的人，当剩下最后一个人时，输出位置。

   复杂度：O(nlog n)

### 二分搜索树（Binary Search Trees）

BST特点：key[leftSubtree(x)] ≤key[x] ≤key[rightSubtree(x)]

BST结点的结构：

* key: an identifying field inducing a total ordering 
* left: pointer to a left child (may be NULL) 
* right: pointer to a right child (may be NULL) 
* p: pointer to a parent node (NULL for root)

1. **查找操作：**

   给定key和指向根结点的指针，返回key的结点或NULL

   递归解法：

   <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-32-31-1cb76e5c398798497d07b7b39b2ecf7f-image-20200810165118555-c5e304.png" alt="image-20200810165118555" style="zoom:50%;" />

   非递归解法：

   <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-32-36-5ca719e2af57307397f46bae020c6649-image-20200810165419851-837489.png" alt="image-20200810165419851" style="zoom:50%;" />

   复杂度：

   最差情况：C(n) = n

   平均情况：C(n) ≈ 2ln n ≈ 1.39log n

   最好情况：C(n) = logn

2. **插入操作：**

   复杂度：O(h)，h是树的高度

3. **使用BST排序**：

   伪代码：<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-04-fbaecb8849aa87202c131b63f0c79616-image-20200810165658381-496a85.png" alt="image-20200810165658381" style="zoom:50%;" />

   思路：先构建BST，再进行中序遍历

   算法复杂度：Ω(n log n)

   平均情况：

   因为构建BST的过程和快排的过程很像，因此算法复杂度类似快排的复杂度，平均情况是O(n log n)

   哪种更好？答：快排，因为快排直接排好位置，不用再进行中序遍历，也不用额外构建树的数据结构。

   但是BST可以用于其他方面，比如优先队列。

4. **删除操作**：

   叶节点直接删除。

   非叶结点有三种情况：

   1. 待删除的节点左子树为空，让待删除节点的右子树替代自己。

      <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-21-706b97788946bc29e1af80765316cc4e-image-20200810171206076-a119ec.png" alt="image-20200810171206076" style="zoom:50%;" />

   2. 待删除的节点右子树为空，让待删除节点的左子树替代自己。

   <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-27-a6db4cf4c014ce25ba05addc975e6471-image-20200810171320981-d82548.png" alt="image-20200810171320981" style="zoom:50%;" />

   3. 如果待删除的节点的左右子树都不为空。我们需要找到**比当前节点小的最大节点（前驱）**，来替换自己

      <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-30-aa45614a3ebd65a832e5e1beb98c7820-image-20200810171401380-858187.png" alt="image-20200810171401380" style="zoom:50%;" />

      或者**比当前节点大的最小节点（后继）**来替换自己。

   <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-34-2ea08c63fdc8211ee6a78494e60a0ce4-image-20200810171436526-0b9503.png" alt="image-20200810171436526" style="zoom:50%;" />

# 变治算法

## 变治算法思路

根据对问题实例的变换方式，变治 思想有*3*种主要类型：

*•*  变换为同样问题的一个更简单或者更方便的实例—实例化简(Instance simplification)

*•*  变换为同样实例的不同表现(Representation Change)

*•*  变换为另一个问题的实例， 这种问题的算法是已知的—问题化简(Problem reduction)

## 实例化简——预排序

**算法思路：**通过将问题转换为同一问题的另一个更简单/更简单的实例来解决问题的实例

许多涉及列表的问题在对列表进行排序时更容易。

* 搜索
* 计算中值（选择问题）
* 检查所有元素是否不同（元素唯一性）

另外：

* 拓扑排序有助于解决dags的一些问题。
* 预排序用于许多几何算法

涉及排序的算法的效率**取决于排序的效率**。

### 使用预排序搜索

问题描述：在A[0..n-1]中搜索给定的K

算法步骤：

1. 通过有效的排序算法对数组进行排序
2. 使用二分法搜索

算法复杂度：Θ(*n*log *n*) + O(log *n*) = Θ(*n*log *n*) 

### 预排序检查元素唯一性

算法步骤：

1. 按有效排序算法排序（如mergesort）
2. 扫描数组检查相邻元素是否相等

算法复杂度： Θ(*n*log *n*) + O(*n*) = Θ(*n*log *n*)

### 与排序查找数组的主元素

算法步骤：

1. 通过有效的排序算法对数组进行排序
2. 扫描数组以找到最频繁的元素

算法复杂度： Θ(*n*log *n*) + O(log *n*) = Θ(*n*log *n*)

### 高斯消元

问题描述：具有任意系数矩阵的n个未知数的n个线性方程组转化为具有上三角系数矩阵的n个未知数的n个线性方程组的等价矩阵。

思路：

![image-20200810195942733](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-43-bcce9eff6e8a96e9d2862a90623ecab0-image-20200810195942733-302f7f.png)

## 变换形式

### Horner’s Rule

问题描述：给定一个n次多项式以及一个特定的x值，在这个点上求p的值。

![image-20200810200153173](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-33-47-6af46962f51b4ac88ac8e3cab54df0cd-image-20200810200153173-852355.png)

暴力算法：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-00-eb4413231676699d7284f33040df6014-image-20200810200358498-613f1b.png" alt="image-20200810200358498" style="zoom:67%;" />

Horner’s Rule算法示例：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-03-1ccdfc539f3ad237a1a6eb6417be706f-image-20200810200435223-394838.png" alt="image-20200810200435223" style="zoom:67%;" />

代入最后一个等式可以得到更快的算法，通过简单地将系数排列在表格中并按以下步骤进行，可获得相同的计算顺序：

![image-20200810200547271](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-05-fe0e37530eb2d2ff5e463bde654a9d45-image-20200810200547271-e02cda.png)

算法伪代码：

![image-20200810200908267](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-07-9a7de393ae6054d2a6a835c33604ca6b-image-20200810200908267-7c4a9e.png)

相当于预先把系数提取出来，再不断累加结果

### 计算a^n

问题描述：在计算a^n次方时，先将n变一变，寻找新的计算路径，预处理就是变治法的根本！

如果单纯循环执行n次相乘，那么时间复杂度为O(n),n为指数；利用二进制幂大大改进效率。

利用二进制幂求解分两种方法：从左至右二进制幂 和 从右至左二进制幂。

1. 从左至右二进制幂

   变化:
   $$
   a^n=a^{b[n]×2^n+b[n-1]×2^{n-1}+...+b[0]×2^0}
   $$
   根据Horner's Rule

   ![image-20200810202819755](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-11-48b60834c5b704c155efe5888eeced73-image-20200810202819755-867796.png)

![image-20200810202848916](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-24-7560232305c644ab1464fa23584c5221-image-20200810202848916-b75277.png)

![image-20200810203304042](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-27-f2f067ca448b567ed9208ff422570c5f-image-20200810203304042-40d9be.png)

![image-20200810203354390](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-31-f5ab796e15c38905c27df0e749500afb-image-20200810203354390-ac4843.png)

算法步骤：

* p初始化为1
* 从左到右扫描n的二进制位，然后执行以下操作：
  * 如果当前二进制数为0，则将累加器平方；
  * 如果二进制数字是1，则将累加器平方，然后乘以a（SM）。

示例：

计算a^13, 此时n=13=1101(二进制)

![image-20200810204054031](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-35-9ffd42e8a008297b366ee04d36f7fea1-image-20200810204054031-32b5e5.png)

算法复杂度： (*b-*1) ≤ M(*n*) ≤ 2(*b-*1) where *b =* floor(log n) + 1

伪代码：

![image-20200810204413827](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-40-74db22a55c277c13b83b359b32f6e95e-image-20200810204413827-e45769.png)

2. 从右至左二进制幂

   ![image-20200810204658010](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-45-3ca8e7c834d327a83f04e93a43827fe1-image-20200810204658010-80c09a.png)

![image-20200810204731226](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-48-72b3b1dbdba4bf9dc89ac470a6fc109d-image-20200810204731226-9c2209.png)

示例：

![image-20200810204845187](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-50-eefbc986eb273b46ff481796c62500da-image-20200810204845187-f833bc.png)

伪代码：

![image-20200810204947406](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-53-4ea6aed8cb1df2f496aa2f1b15e72f7d-image-20200810204947406-4a8822.png)

## 问题化简

思路：将问题转化为另一个已解决的问题。

![image-20200810205304699](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-56-fe48b6ce7b0ba4bf0b9a41a541db575a-image-20200810205304699-d62782.png)

###  路径计数

思路：求图的邻接矩阵n次方来计算图中长度为n的路径数

![image-20200810210122902](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-34-58-283f4316037af22b479aeb7bc450509d-image-20200810210122902-8c62e2.png)

总结下A^n中，A\[i\]\[j\]表示的是从i出发走到点j走n步(哪怕来回往返走动也算一条路径)，有多少种走法。

## Balanced Search Trees

二叉搜索树的最坏情况的效率很差，可以使用以下两种方法避免：

* 当新插入的值使二叉搜索树“不平衡”时重新平衡该树
  * AVL树
  * 红黑树

* 允许搜索树的每个节点有多个键
  * 2-3树
  * 2-3-4树
  * B-树

### AVL树

定义：AVL树是一种二叉搜索树，其中，对于每个节点，其左右子树的高度差（称为平衡因子）最多为1（空树的高度定义为-1）

![image-20200810210738384](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-35-02-fd10d69ae40d4bd7c5de1111e315be53-image-20200810210738384-206c31.png)

a是AVL树，b不是

如果一个关键点的插入违反了某个节点的平衡要求，则该节点的根子树将通过四个旋转之一进行变换。（**最小不平衡子树**：距离插入节点最近的，且平衡因子的绝对值大于1的节点为根的子树。）

**情况一：左单旋转**

首先插入{4，5，6}，在插入元素6后出现不平衡的情况：

![img](data:image/png;base64,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)

当我们**在右子树插入右孩子导致AVL失衡**时，我们需要进行**单左旋**调整。旋转围绕最小失衡子树的根节点进行。 在删除新节点时也有可能会出现需要单左旋的情况。 左旋代码如下：

```c
AVLNode<T>* LeftRotation(AVLNode<T>* cur)
   {
     AVLNode<T>* prchid = cur->rightChild;
     cur->rightChild = prchid->leftChild;
     prchid->leftChild = cur;
     //改变了指向后，还要更新结点对应的高度
     cur->height = max(Height(cur->leftChild),Height(cur->rightChild))+1;
     prchid->height = max(Height(prchid->leftChild), Height(prchid->rightChild)) + 1;
     return prchid;
   }
```

结合例子进行分析：

1. 参数cur为最小失衡子树的根节点，在图四中为节点4
2. 若节点5有左子树，则该左子树成为节点4的右子树
3. 节点4成为节点5的左子树
4. 最后更新节点的高度值

**情况二：右单旋转**

我们继续插入元素{3，2}，此时二叉树为：

![img](data:image/png;base64,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)

插入3、2后出现了不平衡的情况。此时的插入情况是“**在左子树上插入左孩子导致AVL树失衡**”，我们需要进行**单右旋**调整。 单右旋代码为：

 ```c
AVLNode<T>* RightRotation(AVLNode<T>* cur)
    {
        AVLNode<T>* plchild = cur->leftChild;
        cur->leftChild = plchild->rightChild;
        plchild->rightChild = cur;
        cur->height = max(Height(cur->leftChild), Height(cur->rightChild)) + 1;
        plchild->height = max(Height(plchild->leftChild), Height(plchild->rightChild)) + 1;
        return plchild;
    }
 ```

结合例子进行分析：

1. 参数cur为最小失衡子树的根节点，在图四中为节点4
2. 若节点3有右子树，则该右子树成为节点4的左子树
3. 节点4成为节点3的左子树
4. 调整节点的高度值

**情况三：先右旋后左旋**

需要进行两次旋转的原因是第一次旋转后，AVL树仍旧处于不平衡的状态，第二次旋转再次进行调整。 我们继续插入元素{8，7}

![img](data:image/png;base64,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)

这种情况，总结起来就是**“在右子树上插入左孩子导致AVL树失衡"**,此时我们需要进行**先右旋后左旋**的调整。 调整的代码为：

```c
AVLNode<T>* RightLeftRotation(AVLNode<T> *cur)
    {
        cur->rightChild = RightRotation(cur->rightChild);
        return LeftRotation(cur);
    };

```

结合例子进行分析：

1. 首先对最小不平衡子树的根节点（也就是节点6）的右孩子（也就是8）进行右旋操作
2. 再对节点6进行一次左旋操作

**情况四：先左旋后右旋**

根据对称性原理，当我们**“在左子树上插入右孩子导致AVL树失衡"**,此时我们需要进行**先左旋后右旋**的调整。如果你不理解接着看图。 我们接着插入节点{0，1}

![img](data:image/png;base64,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L94gh2EYRl9MehwVJA6EzpDudwl5JRA6X+29FiapG4rP6qpsGDQCqYXOsbOrC63u0hQqLUBNlErAF8Pog456eLnkCl47dFM8zY2A3Am4za8wv1hj5L7Z66FU4ni4tRzid525MQxjUWDmGi1dCp1Tr4TSwgFGBpj60E243UmDZz0SpHh2vAj8sv5Ch2HkjOCwrgarET7tvMrOlukYWTQjUpUT2JWfyskwVmLTpk233nqr4qnVLi433XRTVFTURx99dOLECeGwkrKysmeeeYbOcVVCaX9//61btwonGcuZM2cWLVp01113rVix4sCBA3V1dcIBxhBMVTlEWM+HJgXPEjJKzC506vE/hrE6rdt44fXs8XOUBdA9pwvO9gjs5uJqHvPRwYXys9K4Y36eA8NYh6qqqscffzw7W1hZCXGzcuXKmTNn9unT54cffqBCkPf/7d0PfBT1nf/xzG7CH/kXsepZ8be7FUT5Z9Ra+eEP2eDjjkIFolautmrCzz7wX60o2N+pWKDqeXeGv/bxsKDnBT29nlclgIJ61axatVR9kHhgscHuboWC/yBB/iQhyf4+M59hGHaTzSbZDTK+npfH9PtvZnfPJfPOzHdmamtXrFhhV1w++ugj97Au2Llz5+TJk+++++6qqqrrr79+9OjRt9xyS0NDg92NjhiGEQwGi4uLs/Zremxw4l2TlrjnI0vQWb8l+ZrzrrP+bPb5jtynB+gBJwzse+F3R71TtaVhn/375fO/1n1c++nIsUO1mlM12zfapby8wf1O6eYdOAFkKD8/f9++fVKYPn36vHnz7rnnHtlfSlUap02btmPHDmuUOUwLMmCJZfHixeXl5cuXL587txOPKE/S2tp65513OhlLSZx69NFH7QoyEI/HI5FINv8Y1Wk67qDzwuZfp5+MXFhYKMsvdh55Jnl79OyA0X7K2W1tRDcIdEr67+H5E0fu+3Rf5DfvtDS3yM+ba96TxuHfOVN7k2T3e3jU6arTOV0F9BC/3z9s2DApzJgx47777rv//vt/+9vfOtdObt68WQsFBfZdaktLS2dbbr/99jlz5tx6663BYLvXDneourr6qaeeksJDDz30+eefy+vedJP5tPNf/epXjY2N1hBkyrySXP7zZOuZGqlB58mNy2s/s78QqYqKimS5e1cmKcdc+vztppwvdtXLUjcIdEr67+HIi4ZecuWFLzwWWXnXf8rPK0+/PXnmJWec1faU+ex+D92nq5Km+QPIqdNOO02We/bsOXTokLYcPHhQC4MH25cjOMdympubW1tbZdnU1NT9IPL666/LUnLSrFmzTjrppDFjxvz4xz+Wlg8s1hBkyldVVZV0V8BukqDz/fOPmoy84o0Hdx+wn7+TRHcGtZviWk3jlMBJZ18Y/MY3T7TrKbZZGyHloAvSfw8Nv6/k5kuvmTf9yz0HGhuaSueX/O0145xHwSXJ7vdwcL9T7JL1L8suAci9Pn3Me4T+5Cc/6dWr1znnnCOZY+zYsdIyatSokSNHWkPMyR9aGDFihN/vLygo6N27t6x4880362ThhoaGBx54YNKkScXFxePGjTv//PNl3VAoJDnp/vvvb+9qrLfffluWV1xxxcCB5n0rhHNk6C9/MZ+8hMzlZPrk2OBE92Tkg4f2S9CxK0cLh8OyzCTlyJ/Otyy5tt+gtm/eU7vJvD2J7Fp4kD26oMPvYUHvgou+O+Znj/149sNl3/7b0VK1O46W9e/hVYfvvZk0ux9Arjlno8TWrVudq6s2b97szCz2+drehz7yyCN1deaxYQkl8+bNe/nllyORiGSXTZs2ffDBB7FYbM+ePYsXL96/37xRRSo9I6anzJTzK2X37t1aQIaM3F3Z/+QflrtnFRSfNTXpgnNRX1+vMxgW/NetJ53W9akM//7Amo0b3p89e/aSJUvsJiBjfA8BJJk+ffratWuLi4tHjhxpXSTul+CyYcOGnTt39u/fX8LKGWecIXklFDIPst5xxx1DhgzRy8hlpPypM27cOGlvbW2tqKh4//338y2SnJzlxRbrpZKNHz/+d7/7XXl5+Zw5c7SlsbFRjy2tWrXquuuu00ak4Rxmy2HKEe5HDIo2H6Q8c+ZM+RKEr/rOlbdNsps66cCXDf9v8kNSkC8cT3xF1/A9BOA2derU559//tFHH9U5MeqTTz4577zzJOg888wzV111lZNyampqxowZo2OSNDc3S0bp3bu3BCDZ9Tp73zTuvvvuBx988LLLLluzZo2sJS21tbVnnWU+AKCqqkqPPSM95//POTlj5bhh/FEPKn9i4/KDh5IP0JWVlcly44aaTK60atOGx1+TpfyHZ9eCLuN7CMBNJ9ZoyHDs2rUr35pxrAcIOowsTU1NJSUl/fv3Lygo8Fv3DDz11FOHDh06evToefPmtTdPefz48bKUjLV69WopSE564oknpCDbGT58uDkCHZH/QNFoNBKJ5PZYjnh/x8YVvzsyKafN81aXX355ZWXl0PMCtz3c6QNxtZtiy299UgrV1dXnnpvuiZ5AenwPATgmTZr08ssv//CHP5Q/XXr16rV37953331X08aJJ564adMm+XtG9qPf+ta3pKW9YznOMZg2yeptXnAu6WfixIlvvfWWlGfMmCEp57nnnpPyvffe+4tf/MIagkzlPOUISTmSdexKW+et4vF4UVFRXV3d6PHDZz04w27NgOxaHr3rmYP7GpkJge7jewjAMXny5BdffNGuuJx55pmPP/74JZdcImX5paExZcuWLSNGjLD6jyJ72DfeeGPr1q0SXCSstFikcOjQIdnONddc097RIFlF/u6SpV3Py7v66qt/+ctfOhexI0NGJBKROBmLxbJ4MXmSg4f237tulnOuakhh6K5JyXuCVatW6SmDiyaPueae6dqY3vbaXctvfUJ2LbJnkk/B1VXoPr6HANSzzz5711131dfXf/qpeSeUiyxjx46dMmWK88+8tbVVxuzfv3/x4sW9evXSxmyR/fLq1asfe+yxAwcO3HzzzTfddFP//uYDZ9ApR1JkTg/qVP1p3W82/atdsS6LTX208muvvaaTqjI5ZfD+61v//R/Xyq5FVqmqqrJbgW7jewjATZ807vf77XrPkleXZdL0IGTuyH+23B3LEaGThr+/Y+PeBnte5/a66MSU5w4Gg8FQKFRZWbl7V/3G9TV9+/cZMqyN28t+sbPuueUvrV1R1dzUIn89v/jii3p9HZAVfA8BuBmGcQxDhrx6e2e1kIkeOpYjaj/bvPTVeXYlL+/7513f5gOWa2pqysrKqqurpTz4bwaNGT+874AjO4/ttZ/8zxsfalliWbYeTAEk4XsIAB7QcylHLK2aV/up/Uyrwf1Oue+ylVpOtWrVKtl5xGLmnWRTye5HerleF7nG9xAAjms9mnKSDue0OTvHbc2aNfrHtKOwsLCkpIT9CnoS30MAOL4YhiG/okOhUI+mHJH54RwAAIAucCYz9fSMqu+N+oFdysvbvf9T9310AAAAssgXDodLS0t7bPrksJNHDSk0n/qh3nY9zhMAACCLeuLex0l+H3v1yY3L7Upe3n1TVw4+4RS7AgAA0D3H7IyVGBuc6H6EZ812TloB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)

调整的代码:

 ```c
AVLNode<T>* LeftRightRotation(AVLNode<T> *cur)
    {
        cur->leftChild = LeftRotation(cur->leftChild);
        return RightRotation(cur);
    };
 ```

结合例子进行分析：

1. 首先对最小不平衡子树的根节点（也就是节点2）的左孩子（也就是0）进行左旋操作
2. 再对节点2进行一次右旋操作

**总结**

单左旋

在**右子树插入右孩子**节点，使得平衡因子绝对值由1增至2

单右旋

在**左子树插入左孩子**节点，使得平衡因子绝对值由1增至2

先左旋后右旋

在**左子树插入右孩子**节点，使得平衡因子绝对值由1增至2

先右旋后左旋

在**右子树插入左孩子**节点，使得平衡因子绝对值由1增至2

删除操作与插入操作互逆：

删除节点也可能导致AVL树的失衡，实际上删除节点和插入节点是一种互逆的操作：

1. 删除**右子树的节点**导致AVL树失衡时，相当于在左子树插入节点导致AVL树失衡，即情况情况二或情况四。
2. 删除**左子树的节点**导致AVL树失衡时，相当于在右子树插入节点导致AVL树失衡，即情况情况一或情况三。



AVL算法复杂度：

* AVL树的平均高度h≤1.4404log(n+2)-1.3277,平均高度：1.01 log2n+0.1（根据经验得出）
* 搜索和插入的复杂度是O（logn）
* 删除更复杂，但也是O（logn）

### 红黑树

红黑树满足以下5个特点：

**（1）每个节点或者是黑色，或者是红色。**
**（2）根节点是黑色。**
**（3）每个叶子节点（NIL）是黑色。 [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]**
**（4）如果一个节点是红色的，则它的子节点必须是黑色的。**
**（5）从一个节点到该节点的后继叶节点的所有路径上包含相同数目的黑节点。**

* 从定理5可以推出：
  * 如果一个节点只有一个子节点，那么这个子节点必定是一个红色子节点（只有一个子节点，子节点必为红）
  * 如果一个结点存在黑子结点，那么该结点肯定有两个子结点（有一个黑子节点，必有两个子节点）
* bh(x)，x结点的黑高，即当前节点到任一叶节点的路径上的黑色结点数目，不包括x
* 定理：**一棵含有n个节点的红黑树的高度至多为2log(n+1)**.

红黑树平衡靠三种操作：

* **左旋**：以某个结点作为支点(旋转结点)，其右子结点变为旋转结点的父结点，右子结点的左子结点变为旋转结点的右子结点，左子结点保持不变。如下图。

  ![image-20200810220902935](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-20-b94474887f86779f58465f9135f1daa7-image-20200810220902935-b7ce07.png)

* **右旋**：以某个结点作为支点(旋转结点)，其左子结点变为旋转结点的父结点，左子结点的右子结点变为旋转结点的左子结点，右子结点保持不变。如下图。

  ![image-20200810220931761](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-23-37f92c7c7066acfa9ad0b3af0c9b65ee-image-20200810220931761-37786e.png)

* **变色**：结点的颜色由红变黑或由黑变红。

1. **红黑树的插入操作：**

   插入操作包括两部分工作：一查找插入的位置；二插入后自平衡。查找插入的父结点很简单，跟查找操作区别不大。插入分为以下场景，无特别说明，插入结点的颜色为**红色**。

   ![image-20200810222418726](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-26-abf2356ee052a2d42b460f278dd6fcd9-image-20200810222418726-202e25.png)

   ![image-20200810221602802](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-30-77dc8ff37e06b398e317c246d97d365f-image-20200810221602802-ec5609.png)

**情景1：红黑树为空树**

最简单的一种情景，直接把插入结点作为根结点就行，但注意，根据红黑树性质2：根节点是黑色。还需要把插入结点设为黑色。

**情景2：插入结点的Key已存在**

插入结点的Key已存在，既然红黑树总保持平衡，在插入前红黑树已经是平衡的，那么把插入结点设置为将要替代结点的颜色，再把结点的值更新就完成插入。

**情景3：插入结点的父结点为黑结点**

由于插入的结点是红色的，当插入结点的黑色时，并不会影响红黑树的平衡，**直接插入即可**，无需做自平衡。

**情景4：插入结点的父结点为红结点**

回想下红黑树的性质2：根结点是黑色。**如果插入的父结点为红结点，那么该父结点不可能为根结点，所以插入结点总是存在祖父结点**。这点很重要，因为后续的旋转操作肯定需要祖父结点的参与。

情景4又分为很多子情景，

* **插入情景4.1：叔叔结点存在并且为红结点**

  从红黑树性质4可以，祖父结点肯定为黑结点，因为不可以同时存在两个相连的红结点。那么此时该插入子树的红黑层数的情况是：黑红红。显然最简单的处理方式是把其改为：红黑红。如下图所示。

![image-20200810235839177](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-34-24be133f1f26e4d56a987658bfbd802c-image-20200810235839177-02fb66.png)

​	**处理：**

​		 **将P和S设置为黑色**

​	 	**将PP设置为红色**

​	     **把PP设置为当前插入结点**

​		可以看到，我们把PP结点设为红色了，如果PP的父结点是黑色，那么无需再做任何处理；但**如果PP的父结点是红色**，根据性质4，此时红黑树已不平衡了，所以还**需要把PP当作新的插入结点，继续做插入操作自平衡处理**，直到平衡为止。

​		**试想下PP刚好为根结点**时，那么根据性质2，我们**必须把PP重新设为黑色**，那么树的红黑结构变为：黑黑红。换句话说，从根结点到叶子结点的路径中，黑色结点增加了。**这也是唯一一种会增加红黑树黑色结点层数的插入情景**。

* **插入情景4.2：叔叔结点不存在或为黑结点，并且插入结点的父亲结点是祖父结点的左子结点**

  单纯从插入前来看，也即不算情景4.1自底向上处理时的情况，**叔叔结点非红即为叶子结点(Nil)**。因为如果叔叔结点为黑结点，而父结点为红结点，那么叔叔结点所在的子树的黑色结点就比父结点所在子树的多了，这不满足红黑树的性质5。

  **插入情景4.2.1：插入结点是其父结点的左子结点**

  ​	**处理：**

  ​	**将P设为黑色**

  ​	**将PP设为红色**

  ​	**对PP进行右旋**

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-38-913496e7e5091262bd792c7e8f680721-image-20200811095618171-986ffc.png" alt="image-20200811095618171" style="zoom: 67%;" />

​	**插入情景4.2.1：插入结点是其父结点的左子结点**

​	这种情景显然可以转换为情景4.2.1，如下图所示

​	**对P进行左旋**

​	**把P设置为插入结点，得到情景4.2.1**

​	**进行情景4.2.1的处理**

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-41-4723df414efbf2f8bb27baa59832cfbd-image-20200811100419210-5d7f3b.png" alt="image-20200811100419210" style="zoom:67%;" />

* **插入情景4.3：叔叔结点不存在或为黑结点，并且插入结点的父亲结点是祖父结点的右子结点**

  该情景对应情景4.2，只是方向反转

  **插入情景4.3.1：插入结点是其父结点的右子结点**
  **处理：**

  **将P设为黑色**

  **将PP设为红色**

  **对PP进行左旋**

  <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-44-4e085ef22a948f5e2b3c2c926acb74b4-image-20200811100612370-e4a940.png" alt="image-20200811100612370" style="zoom:67%;" />

  **插入情景4.3.2：插入结点是其父结点的左子结点**
  **处理：**

  **对P进行右旋**

  **把P设置为插入结点，得到情景4.3.1**

  **进行情景4.3.1的处理**

  <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-47-13ccb5a2c65e5a7cbb8dbd8c8c8e32dd-image-20200811100723903-74ee9d.png" alt="image-20200811100723903" style="zoom:67%;" />

  **插入总结：**

  插入空树以及父节点为黑，无需考虑，因为插入节点为红色，不影响黑高。

  如果父节点为红，看叔叔节点，叔叔节点为红，则父辈节点（父亲+叔叔）变黑，祖父节点变红，继续递归上去检查。

  叔叔节点为黑（也就是不存在叔叔的情况），先通过旋转把插入节点，父节点，祖父节点变为同一侧的情况：

  ![image-20200811111848546](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-50-f2d73217b2dc09be1ebe6921fa85ef99-image-20200811111848546-e85387.png)

  ​	变成这种情况后就可以再旋转，然后把父节点当成根节点（祖父节点必黑，所以变黑），祖父节点变为父节点的孩子节点（变红）。

2. #### 红黑树删除

   红黑树的删除操作也包括两部分工作：一查找目标结点；二删除后自平衡。查找目标结点显然可以复用查找操作，当不存在目标结点时，忽略本次操作；当存在目标结点时，删除后就得做自平衡处理了。删除了结点后我们还需要找结点来替代删除结点的位置，不然子树跟父辈结点断开了，除非删除结点刚好没子结点，那么就不需要替代。

   一个重要的思路：**删除结点被替代后，在不考虑结点的键值的情况下，对于树来说，可以认为删除的是替代结点！**如下图所示：

   ![image-20200811101217934](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-53-d2f3564ff09a8bd6f61cc8a78617fb02-image-20200811101217934-1053c0.png)

   综上所述，**删除操作删除的结点可以看作删除替代结点，而替代结点最后总是在树末。**

   下面我们开始讨论修复操作（<u>下面的叶子节点都是指非NULL的叶子节点</u>）：

   A. **删除的是叶子节点且该叶子节点是红色**的 ---> **无需修复**，因为它不会破坏红黑树的5个特性

   B. **删除的是叶子节点且该叶子节点是黑色**的 ---> 很明显会破坏特性5，需要修复。

   C. **删除的节点（为了便于叙述我们将其称为P）下面有一个子节点 S**，对于这种情况我们通过**将P和S的值交换**的方式，巧妙的**将删除P变为删除S**，S是叶子节点，这样C这种情况就会转 换为A, B这两种情况：

   C1： P为黑色，S为红色 ---> 对应 A 这种情况

   C2: P为黑色或红色，S为黑色 --- > 对应 B 这种情况

   D. **删除的节点有两个子节点**，对于这种情况，我们通过**将P和它的后继节点N的值交换**的方 式，将**删除节点P转换为删除后继节点N**，而后继节点只可能是以下两种情况：

   D1: N是叶子节点 --- > 对应情况 A 或 B

   D2: N有一个子节点 ---- > 对应情况 C

   所以通过上面的分析我们发现，红黑树节点删除后的修复操作都可以转换为 A 或 B这两种情况，而A不需要修复，所以我们**只需要研究B这种情况如何修复**就行了。

   下面我们讨论如何修复B中情况：

![image-20200811103625953](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-38-57-bdb6525a280739f53e7c583cbbd63795-image-20200811103625953-d7aa91.png)

![image-20200811104128586](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-01-987560aa334d87d83fb6cc94311858f5-image-20200811104128586-118784.png)

![image-20200811104838197](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-05-5aedae51c6e8a07f61a61346134d9b78-image-20200811104838197-e061fe.png)

![image-20200811104847176](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-08-20ff8d33ccce0caf00495b8ba871a4bc-image-20200811104847176-c2422b.png)

​	删除总结：

​	**场景1** 如果删除的是叶节点，两种情况：红色直接删，黑色另考虑。

​	如果删除的节点**有一个子节点**，拼接子节点，相当于删除子节点，转为场景1

​	如果删除的节点**有两个子节点**，找后继节点，相当于删除后继节点，转为场景1

​	场景1中，删除节点为黑色的情况下，看其兄弟节点颜色。

​	兄弟节点为黑，且原理该节点一侧有子节点如下所示：

![image-20200811110241343](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-14-6bf3913c38870aa4f3f208322ae110a3-image-20200811110241343-242f08.png)

​	删除该节点后以父节点为支点左旋，兄弟节点的颜色变为父节点的颜色。旋转后的子节点颜色全变黑色（因为不知道父节点是什么颜色，如果是红色则子节点必要求全黑）

![image-20200811110619699](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-16-e3258f15b0d62bd37d3f5778c658c00e-image-20200811110619699-915ad1.png)

​	兄弟节点有两个结点或靠近一侧有子节点的情况，都转为该情况。

​	如果兄弟节点为红色，则它必有两个黑色子节点。因为删除节点为黑，比兄弟节点黑高多了1。

​	![image-20200811110934716](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-19-eee95e5e3ee01bcbe6c0581d6e99d2c3-image-20200811110934716-2407c1.png)

​	也是左旋，兄弟节点取父亲节点颜色（必为黑），兄弟节点的左子节点要借给父亲节点的右节点，故必须变为红色。

### 2-3树

定义：2-3树是一种搜索树满足以下特点：

* 可能有2个或3个子节点
* 高度平衡（所有叶节点都在同一水平面上）

2-3树是通过连续插入给定的键值来构造的，其中一个新的键总是插入树的一个叶节点中。如果叶节点有三个节点，则将其拆分为两个，中间节点提升为父节点

![image-20200811125150203](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-22-78ffb012013b69b367ef9a9e6e1f1a5c-image-20200811125150203-0c6b73.png)

复杂度分析：

高度范围：
$$
log_3(n+1)-1\leq h\leq log_2(n+1)-1
$$
查找、插入、删除操作的复杂度均为：Θ(logn)

2-3树可以拓展为多向搜索树

* Multiway Search Trees 

  定义：多向搜索树是一种允许在树的同一节点中有多个键的搜索树。如果搜索树的一个节点包含n-1个有序键，则称为n-node（将整个键范围划分为n个区间，这些区间由节点与其子节点的n个链接指向）：

![image-20200811134938719](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-27-1b165cadaac54ed21f9ca8f0251b2e10-image-20200811134938719-17a4e5.png)

**2-3树的插入操作：**

要在2-3树中插入I，首先找到I对应的叶节点的位置。

将I插入叶节点中。
如果叶节点只包含两个键，则完成操作。如果叶节点包含三个键，则必须拆分它，把中间的键提到父节点

**2-3树的删除操作：**

1. 找到要删除的键I的节点位置n

2. 如果节点n不是叶结点→将I与其最接近的后继节点的键值交换，然后将对应的叶节点执行删除操作（第3步）

3. 如果叶节点n包含其他键值，只需删除I键值。否则，尝试从同级节点中重新分布节点，如果不可能，则合并节点（请参见下一张幻灯片）

   重分布：

   ![image-20200811135839239](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-30-124547bdf3f4304da4f8053b3a7c7936-image-20200811135839239-76bdc7.png)

   合并节点：

   

![image-20200811135926718](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-32-47bc85ff986fa25bb0b29a19943f3c25-image-20200811135926718-e02d4f.png)

### 堆排序

定义：堆（英语：heap)是计算机科学中一类特殊的数据结构的统称。堆通常是一个可以被看做一棵树的数组对象。堆总是满足下列性质：

- 堆中某个节点的值总是不大于或不小于其父节点的值；
- 堆总是一棵完全二叉树。

特点：

* 给定n个节点，堆的高度为
  $$
  h=\lfloor log_2n\rfloor
  $$

* 根节点是最大值（或最小值）

* 子树也是一个堆

* 堆可以用数组表示

  由于堆存储在下标从0开始计数的数组中，因此，在堆中给定下标为i的结点时：

  如果 i = 0，结点 i 是根结点，无父结点；否则结点 i 的**父结点**为结点 **[(i - 2) / 2]  向上取整**
  如果 2i + 1 > n - 1，则结点 i 无左结点；否则结点 i 的**左子女**为结点 **2i + 1**
  如果 2i + 2 > n - 1，则结点 i 无右结点；否则结点 i 的**右子女**为结点 **2i + 2**

堆的构建（自底向上）：

1. 使用给定顺序的键初始化结构。
2. 从最后一个（最右边）节点的父节点(n/2)开始调整，如果堆不满足条件，则与子节点交换：注意，被调整的节点，还有子节点的情况，需要递归进行调整。
3. 调整下标-1的节点，重复第2步，直到达到根节点。

![image-20200811142502038](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-36-50eb7cc35e138ea8f5b5fc8c5364ed8d-image-20200811142502038-25c211.png)

* **堆排序：**

  算法步骤：

  1. 为给定的n个键列表构造一个堆
  2. 重复删除根节点n-1次：
     * 交换根节点和最后一个叶节点（最右边）的键值
     * 将堆的大小-1
     * 重新调整堆结构，直到满足堆条件

![image-20200811142847731](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-40-ecf243980ff018ea4cc46ad4f0de350c-image-20200811142847731-12cbea.png)

算法复杂度分析：

1. 给定n个元素构建堆的步骤，最差情况，每一个结点都要调整：

   ![image-20200811143119461](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-43-68e4b9d15ecdab2e736893cced212b6f-image-20200811143119461-f9ed47.png)

2. 重复移除根结点n-1次，最差情况：

   ![image-20200811143251686](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-47-6de2c75ba87f5b042346116011365a8b-image-20200811143251686-5cea28.png)

   平均情况和最差情况的效率都为：Θ(*n*log*n*)

### 优先队列

定义：优先队列中，元素被赋予优先级。当访问元素时，具有最高优先级的元素最先删除。优先队列具有最高级先出 （first in, largest out）的行为特征。

堆是实现优先级队列的一种非常有效的方法。

堆的插入操作：

* 在堆的最后一个位置插入新元素
* 比较插入元素与其父节点，如果不满足堆的条件，则交换
* 继续向上比较，直到堆条件满足

### 迷宫问题

![image-20200811144124478](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-51-74d0770944cc9de8ab30e990b9bcdae3-image-20200811144124478-30c3f0.png)

使用DFS方法比BFS更有效，DFS类似于右手规则（总是保持右手有墙）

# 时间和空间的权衡

对问题的部分或全部输入做预处理，获得额外的信息进行存储，以加速后面问题的求解，以空间换时间

## 计数排序

计数排序不是一个比较排序算法，该算法于1954年由 Harold H. Seward提出，通过计数将时间复杂度降到了`O(N)`。

算法思路：

![image-20200811150209105](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-39-57-0179db949eb690060efbe31baf0810e2-image-20200811150209105-d031c4.png)

​	创建一个额外的数组Count，大小与Array一样，初始化为0。从Array的第一个元素开始，依次与后续元素比较，如果比后续元素大，则对应的Count下标的值+1，比后续元素小，则该元素下标的Count值+1，每一个元素都比较一遍，最后Count记录的值则为Array正确排序的下标值。

算法代码：

![image-20200811150524402](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-00-1e9da8b7cef2971c30af0cb694ce0004-image-20200811150524402-818ab7.png)

A是待排序数组，B是输出的排序数组，C是辅助计数数组。

算法效率：O(n)，但是需要额外的空间，当待排数组元素较大时不适用。

## Horspool算法

描述：字符串匹配算法，是Boyer-Moore算法的简化版，Horspool 算法是一种基于后缀匹配的方法

算法思路：**算法把模式P和文本T的开头字符对齐，从模式的最后一个字符开始比较，如果尝试比较失败了，它把模式向后移。每次尝试过程中比较是从右到左的。**假设文本中，**对齐模式最后一个字符的元素是c**,Horspool算法**根据c的不同情况来确定移动距离**，**无论c是否和模式的最后一个字符相匹配**。一般来说，会存在下面四种情况。

![image-20200811152916172](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-04-da662c635494162367ee0340cc5abf23-image-20200811152916172-bf639d.png)

**情况1：**看第一行，模式中不存在c（此时c就是字母A），模式的移动长度就是它的全部长度，移到第二行所示的位置。

**情况2：**看第二行，c（此时c就是字符O）正好是模式的最后一个字符，但是从右向左比较时，有字符不匹配，比如此时的**A**和**E**不匹配。而且模式中的其他**m-1**个字符也不包含c。移动的情况类似情况1，移动的幅度等于模式的全部长度，移到第三行所示的位置。

![image-20200811153212341](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-08-88050370833e5a885c2519c40202b489-image-20200811153212341-53109b.png)

**情况3：**看第一行，模式中存在c（此时c就是字符L），但是它不是模式的最后一个字符，移动时应该把模式中最右边的c和文本中的c对齐，移到第二行所示的位置。

**情况4：**看第二行，c（此时c就是字符O）正好是模式的最后一个字符，但是从右向左比较时，有字符不匹配，比如此时的**A**和**E**不匹配。而此时模式中的其他**m-1**个字符包含c。移动的情况类似情况3，移动时应该把前**m-1**个字符中最右边的c和文本中的c对齐，移到第三行所示的位置

移动多少位可以根据移位表来查看，我们可以预先算出遇到某个字符要移动的距离，并把它存在一个表中。具体来说，对于每一个字符c，可以通过以下公式算出移动距离：

![image-20200811153518206](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-10-b0e2b250dedc4b579bf1d789a5090859-image-20200811153518206-d9e96b.png)

如对于模式**BARBER**，移动距离如下表所示：

![image-20200811153607469](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-12-bfbcc526af957cc2e5dc92354e7b9190-image-20200811153607469-392c70.png)

构建移位表伪代码：

![image-20200811153901575](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-14-f4450bae0690e88e002f2472f70610b8-image-20200811153901575-90d110.png)

Table为长度为26的数组，对应26个字母：

![image-20200811154455476](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-18-45faeb50c179b7cd40d36785eccf27a2-image-20200811154455476-35daf3.png)

Horspool匹配算法伪代码：

![image-20200811154520239](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-21-a251804fdfbe571a943c67d0fa03689b-image-20200811154520239-41fb3f.png)

算法复杂度：

最坏情况：**Cw = m(n −m +1)**，m是模式长度，n是匹配文本长度

最好情况：**Cb = m**

## Hash

算法思路：散列的思想是通过使用预定义的函数（称为散列函数）将大小为n的给定文件的key映射到大小为m的表（称为散列表）中。

* 冲突：

  如果h(K1)=h(K2)，则存在冲突。

  * 好的散列函数会导致较少的冲突，但一些冲突应该是可以预料的（birthday paradox）

  * 不同的散列模式处理冲突的方式不同：

    * 开散列：又名链地址法，先用哈希函数计算每个数据的散列地址，把具有相同地址的元素归于同一个集合之中，把该集合处理为一个链表，链表的头节点存储于哈希表之中。

    * 闭散列：当发生哈希冲突时，如果该哈希表还没有被填满，那么就把该元素放到哈希表的下一个空闲的位置。

      ​	线性探测法：找下一个空闲位置

      ​	二次散列：使用第二个散列函数来计算位置增量

* 开散列：

  若散列函数均匀分布key，则链表的平均长度为α=n/m，这个比值称为负载因子。

  平均探测成功数为S，不成功的查找数为U

  <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-28-cf0e476f28b06616208f4249bca32a8b-image-20200811161511968-a9caac.png" alt="image-20200811161511968" style="zoom:50%;" />

  当n>m时，开散列依然有效

* 闭散列：

  * 当n>m时，散列无效

  * 删除操作比较复杂

  * 查找/插入/删除键的探测数取决于负载因子α=n/m（哈希表密度）和冲突解决策略。

    对于线性探测：

    <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-32-80a6f4284cc9115f81f9c8016856167a-image-20200811161722817-739122.png" alt="image-20200811161722817" style="zoom: 67%;" />

## KMP算法

KMP 算法是 D.E.Knuth、J,H,Morris 和 V.R.Pratt 三位神人共同提出的，称之为 Knuth-Morria-Pratt 算法，简称 KMP 算法。该算法相对于 Brute-Force（暴力）算法有比较大的改进，主要是消除了主串指针的回溯，从而使算法效率有了某种程度的提高。

算法描述：

KMP算法的核心，是一个被称为**部分匹配表(Partial Match Table)**的数组。

对于字符串“abababca”，它的PMT如下表所示：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-40-45-60f160bc04d74b444010514444cd7d3a-image-20200811164848807-5f757a.png" alt="image-20200811164848807" style="zoom:67%;" />

解释一下字符串的前缀和后缀。如果字符串A和B，存在A=BS，其中S是任意的非空字符串，那就称B为A的前缀。例如，”Harry”的前缀包括{”H”, ”Ha”, ”Har”, ”Harr”}，我们把所有前缀组成的集合，称为字符串的前缀集合。同样可以定义后缀A=SB， 其中S是任意的非空字符串，那就称B为A的后缀，例如，”Potter”的后缀包括{”otter”, ”tter”, ”ter”, ”er”, ”r”}，然后把所有后缀组成的集合，称为字符串的后缀集合。要注意的是，字符串本身并不是自己的后缀。

**PMT中的值是字符串的前缀集合与后缀集合的交集中最长元素的长度**。例如，对于”aba”，它的前缀集合为{”a”, ”ab”}，后缀 集合为{”ba”, ”a”}。两个集合的交集为{”a”}，那么长度最长的元素就是字符串”a”了，长 度为1，所以对于”aba”而言，它在PMT表中对应的值就是1。再比如，对于字符串”ababa”，它的前缀集合为{”a”, ”ab”, ”aba”, ”abab”}，它的后缀集合为{”baba”, ”aba”, ”ba”, ”a”}， 两个集合的交集为{”a”, ”aba”}，其中最长的元素为”aba”，长度为3。

再来看如何使用这个表来加速字符串的查找，以及这样用的道理是什么。如下图所示，要在主字符串"ababababca"中查找模式字符串"abababca"。如果在 j 处字符不匹配，那么由于前边所说的模式字符串 PMT 的性质，**主字符串中 i 指针之前的 PMT[j −1] 位就一定与模式字符串的第 0 位至第 PMT[j−1] 位是相同的**。这是因为主字符串在 i 位失配，也就意味着主字符串从 i−j 到 i 这一段是与模式字符串的 0 到 j 这一段是完全相同的。而我们上面也解释了，模式字符串从 0 到 j−1 ，在这个例子中就是”ababab”，其前缀集合与后缀集合的交集的最长元素为”abab”， 长度为4。所以就可以断言，主字符串中i指针之前的 4 位一定与模式字符串的第0位至第 4 位是相同的，即长度为 4 的后缀与前缀相同。这样一来，我们就**可以将这些字符段的比较省略掉**。具体的做法是，**保持i指针不动，然后将j指针指向模式字符串的PMT[j −1]位即可**。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-30-099e64192d2e47b596a6cdae60f07047-image-20200811165047683-c07e14.png" alt="image-20200811165047683" style="zoom:67%;" />

有了上面的思路，我们就可以使用PMT加速字符串的查找了。我们看到如果是在 j 位 失配，那么影响 j 指针回溯的位置的其实是第 j −1 位的 PMT 值，所以为了编程的方便， 我们不直接使用PMT数组，而是**将PMT数组向后偏移一位。我们把新得到的这个数组称为next数组**。下面给出根据next数组进行字符串匹配加速的字符串匹配程序。其中要注意的一个技巧是，在把PMT进行向右偏移时，**第0位的值，我们将其设成了-1，这只是为了编程的方便**，并没有其他的意义。在本节的例子中，next数组如下表所示。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-33-0768e08087f62b20b1dfa3fd14dc8b10-image-20200811165445882-090095.png" alt="image-20200811165445882" style="zoom:67%;" />

具体程序如下：

```c++
int KMP(char * t, char * p) 
{
	int i = 0; 
	int j = 0;

	while (i < strlen(t) && j < strlen(p))
	{
		if (j == -1 || t[i] == p[j]) 
		{
			i++;
           	j++;
		}
	 	else 
           	j = next[j];
    }

    if (j == strlen(p))
       return i - j;
    else 
       return -1;
}
```



现在，我们再看一下如何编程快速**求得next数组**。其实，**求next数组的过程完全可以看成字符串匹配的过程**，即**以模式字符串为主字符串**，以**模式字符串的前缀为目标字符串**，一旦字符串匹配成功，那么当前的next值就是匹配成功的字符串的长度。具体来说，就是**从模式字符串的第一位(注意，不包括第0位)开始对自身进行匹配运算**。 在任一位置，能匹配的最长长度就是当前位置的next值。如下图所示。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-38-645f3ec49836d3c680869403e74f7934-v2-645f3ec49836d3c680869403e74f7934_720w-2b693a.jpg" alt="img" style="zoom:50%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-41-06477b79eadce2d7d22b4410b0d49aba-v2-06477b79eadce2d7d22b4410b0d49aba_720w-e323e4.jpg" alt="img" style="zoom:50%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-44-994a0383c78e8b5ba592a338c56ab9cb-v2-8a1a205df5cad7ab2f07498484a54a89_720w-ae59cf.jpg" alt="img" style="zoom:80%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-47-f2b50c15e7744a7b358154610204cc62-v2-f2b50c15e7744a7b358154610204cc62_720w-68c9e5.jpg" alt="img" style="zoom:50%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-49-bd42e34a9266717b63706087a81092ac-v2-bd42e34a9266717b63706087a81092ac_720w-f7c842.jpg" alt="img" style="zoom:50%;" />

求next数组值的程序如下所示：

```c++
void getNext(char * p, int * next)
{
	next[0] = -1;
	int i = 0, j = -1;

	while (i < strlen(p))
	{
		if (j == -1 || p[i] == p[j])
		{
			++i;
			++j;
			next[i] = j;
		}	
		else
			j = next[j];
	}
}
```

算法复杂度：O(n)

## Boyer-Moore算法

* Boyer-Moore算法是从模式串的尾字符开始**从右至左**做比较
* Boyer-Moore算法引申出来两条移动规则：好后缀移动（good-suffix shift）与坏字符移动（bad-character shift）。

1. **坏字符规则**

   后移位数 = 坏字符的位置 - 搜索词中的上一次出现位置（与Horspool算法一样）

   ![image-20200811194416746](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-56-a8e816afeb344960736b47d3b44dba83-image-20200811194416746-a93d43.png)

   以"P"为例，它作为"坏字符"，出现在搜索词的第6位（从0开始编号），在搜索词中的上一次出现位置为4，所以后移 6 - 4 = 2位

   ![image-20200811194455445](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-41-59-9949dd818dcc492ad49c2bbdcc6124b1-image-20200811194455445-70d082.png)

   "S"为例，它出现在第6位，上一次出现位置是 -1（即未出现），则整个搜索词后移 6 - (-1) = 7位。

2. **好后缀规则**

   后移位数 = 好后缀的位置 - 搜索词中的上一次出现位置

   举例来说，如果字符串"ABCDAB"的后一个"AB"是"好后缀"。那么它的位置是5（从0开始计算，取最后的"B"的值），在"搜索词中的上一次出现位置"是1（第一个"B"的位置），所以后移 5 - 1 = 4位，前一个"AB"移到后一个"AB"的位置。

   这个规则有三个注意点：

   （1）"好后缀"的位置以最后一个字符为准。假定"ABCDEF"的"EF"是好后缀，则它的位置以"F"为准，即5（从0开始计算）。

   （2）如果"好后缀"在搜索词中只出现一次，则它的上一次出现位置为 -1。比如，"EF"在"ABCDEF"之中只出现一次，则它的上一次出现位置为-1（即未出现）。

   （3）如果"好后缀"有多个，则除了最长的那个"好后缀"，其他"好后缀"的上一次出现位置必须在头部。比如，假定"BABCDAB"的"好后缀"是"DAB"、"AB"、"B"，请问这时"好后缀"的上一次出现位置是什么？回答是，此时采用的好后缀是"B"，它的上一次出现位置是头部，即第0位。这个规则也可以这样表达：如果最长的那个"好后缀"只出现一次，则可以把搜索词改写成如下形式进行位置计算"(DA)BABCDAB"，即虚拟加入最前面的"DA"。

   感觉有点像KMP的规则

**Boyer-Moore算法的基本思想是，每次后移这两个规则之中的较大值。**

## Sunday算法

Sunday算法和BM算法稍有不同的是，Sunday算法是**从前往后匹配**，在匹配失败时**关注的是主串中参加匹配的最末位字符的下一位字符**。

- 如果该字符没有在模式串中出现则直接跳过，即移动位数 = 模式串长度 + 1；
- 否则，其移动位数 = 模式串长度 - 该字符最右出现的位置(以0开始) = 模式串中该字符最右出现的位置到尾部的距离 + 1。

下面举个例子说明下Sunday算法。假定现在要在主串”substring searching”中查找模式串”search”。

刚开始时，把模式串与文主串左边对齐：

![image-20200811201817853](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-04-98ff124c511a79867d9c58960d4d63ac-image-20200811201817853-66a877.png)

结果发现在第2个字符处发现不匹配，不匹配时关注主串中参加匹配的最末位字符的下一位字符，即标粗的字符 `i`，因为模式串search中并不存在`i`，所以模式串直接跳过一大片，向右移动位数 = 匹配串长度 + 1 = 6 + 1 = 7，从 `i` 之后的那个字符（即字符`n`）开始下一步的匹配，如下图：

![image-20200811201914760](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-08-919f0ce78f81adde94e307f6e03d0a84-image-20200811201914760-e62f3a.png)

结果第一个字符就不匹配，再看主串中参加匹配的最末位字符的下一位字符，是’r’，它出现在模式串中的倒数第3位，于是把模式串向右移动3位（m - 3 = 6 - 3 = r 到模式串末尾的距离 + 1 = 2 + 1 =3），使两个’r’对齐，如下：

![image-20200811201959643](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-10-a366123741d2df00a25700acb367fd41-image-20200811201959643-af4f82.png)

## 动态哈希

定义：动态散列的方法：在hash表的元素增长的同时，动态的调整hash桶的数目。

动态hash不需要对hash表中所有元素进行再次插入操作(重组)，而是在原来基础上，进行动态的桶扩展。有多种方法可以实现：多hash 表、可扩展的动态散列和线性散列。

* 可拓展的动态散列

基本思想：为桶引入一间接层，即用一个指向块的指针数组来表示桶，而不是用数据块本身组成的数组来表示桶。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-14-79d1dab9a15bc9a1c548f96b75cd861c-image-20200811203459755-81c967.png" alt="image-20200811203459755" style="zoom:67%;" />

指针数组能增长，其**长度总是2的幂**。因而**数组每增长一次，桶的数目就翻倍**。不过，并非每个桶都有一个数据块；如果**某些桶中的所有记录可以放在一个块中，则这些桶可能共享一个块。**

**散列函数h为每个键计算出一个K位二进制序列**，该K足够大，比如32。但是桶的数目总是使用从序列**第一位或最后一位算起**的若干位，此位数小于K,**比如说i位**。也就是说，**当i是使用的位数时，桶数组将有2i个项**。

![在这里插入图片描述](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-24-7c6f43b17687ff03958ed2565c1756e8-20191120155920179-486d37.png)

![在这里插入图片描述](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-28-4c712b50b34f1f30efa9169e501ca457-20191120155952330-102a56.png)

![在这里插入图片描述](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-32-154a6628eabfd92402c448021fde7039-20191120160032715-bf0f60.png)

![在这里插入图片描述](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-36-f00a0079261c3fd09e2f4799dcdc99b3-20191120160114875-ac48c9.png)

![在这里插入图片描述](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-40-1a7f1c381a3e2f646a08da69e2792540-20191120160155504-3d4d0b.png)

* 可拓展散列优点：

  性能不会随着文件的增长而降低

  最小空间开销

* 可拓展散列缺点：

  查找记录需要额外的间接寻址

  Bucket的地址表本身可能会变得非常大（大于内存）

  更改bucket地址表的大小开销非常大

## B+树

B+树满足以下特征：

* 从根结点到叶结点的所有路径长度相同
* 内部节点拥有<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082615-32-59-ac40c51609b4d3056dd98cc4b7727b1c-image-20200811205254174-2ef3fb.png" alt="image-20200811205254174" style="zoom:50%;" />到n个子节点
* 叶节点拥有<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082615-33-01-e81d5314d1de98935684a361202e8843-image-20200811205341937-ce94d8.png" alt="image-20200811205341937" style="zoom:50%;" />到n-1个值
* 如果根节点不是叶节点，则至少有两个孩子 
* 如果根节点是叶节点，则它拥有0~n-1个值

- 所有的叶子结点中包含了全部元素的信息，及指向含这些元素记录的指针，且叶子结点本身依关键字的大小自小而大顺序链接。
- 所有的**中间节点元素都同时存在于子节点**，**在子节点元素中是最大（或最小）元素**

1）B+树包含2种类型的结点：内部结点（也称索引结点）和叶子结点。根结点本身即可以是内部结点，也可以是叶子结点。根结点的关键字个数最少可以只有1个。

2）B+树与B树最大的不同是内部结点不保存数据，只用于索引，所有数据（或者说记录）都保存在叶子结点中。

3） m阶B+树表示了**内部结点最多有m-1个关键字**（或者说内部结点最多有m个子树），阶数m同时限制了**叶子结点最多存储m-1个记录**。

4）内部结点中的key都按照从小到大的顺序排列，对于内部结点中的一个key，左树中的所有key都**小于**它，右子树中的key都**大于等于**它。叶子结点中的记录也按照key的大小排列。

5）每个叶子结点都存有相邻叶子结点的指针，叶子结点本身依关键字的大小自小而大顺序链接。



​	从根节点开始，查找大于k的最小值，顺着索引一直往下。如果不存在，则沿着节点最大的值往下，一直到叶节点。沿着叶节点指针一直往右，直到找到对应的k值

查找效率：查找的路径不会超过<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082615-33-06-f9a0aa6d77d3235d95bcf57325de12d7-image-20200811211028079-da828a.png" alt="image-20200811211028079" style="zoom:67%;" />，K是数据总数

**B+树的插入操作：**

1）若为空树，创建一个叶子结点，然后将记录插入其中，此时这个叶子结点也是根结点，插入操作结束。

2）针对叶子类型结点：根据key值找到叶子结点，向这个叶子结点插入记录。插入后，**若当前结点key的个数小于等于m-1，则插入结束**。**否则将这个叶子结点分裂成左右两个叶子结点**，**左叶子结点包含前m/2个记录**，右结点包含剩下的记录，**将第m/2+1个记录的key进位到父结点中**（内部节点存最小值的情况）（父结点一定是索引类型结点），进位到父结点的key左孩子指针向左结点,右孩子指针向右结点。将当前结点的指针指向父结点，然后执行第3步。

3）针对索引类型结点：若当前结点key的个数小于等于m-1，则插入结束。否则，将这个索引类型结点分裂成两个索引结点，左索引结点包含前(m-1)/2个key，右结点包含m-(m-1)/2个key，将第m/2个key进位到父结点中，进位到父结点的key左孩子指向左结点, 进位到父结点的key右孩子指向右结点。将当前结点的指针指向父结点，然后重复第3步。

![image-20200811212419924](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-47-ab8573e23773e9a770285b29a6e43855-image-20200811212419924-6d1f11.png)

**B+树的删除：**

如果叶子结点中没有相应的key，则删除失败。否则执行下面的步骤

1）删除叶子结点中对应的key。删除后若结点的key的个数大于等于Math.ceil(m-1)/2 ，删除操作结束,否则执行第2步。

2）若**兄弟结点key有富余**（大于Math.ceil(m-1)/2），**向兄弟结点借一个记录**，同时用借到的key替换父结（指当前结点和兄弟结点共同的父结点）点中的key，删除结束。否则执行第3步。

3）**若兄弟结点中没有富余的key,则当前结点和兄弟结点合并成一个新的叶子结点**，**并删除父结点中的key**（父结点中的这个key两边的孩子指针就变成了一个指针，正好指向这个新的叶子结点），将当前结点指向父结点（必为索引结点），执行第4步（第4步以后的操作和B树就完全一样了，主要是为了更新索引结点）。

4）若索引结点的key的个数大于等于Math.ceil(m-1)/2 – 1，则删除操作结束。否则执行第5步

5）若兄弟结点有富余，父结点key下移，兄弟结点key上移，删除结束。否则执行第6步

6）当前结点和兄弟结点及父结点下移key合并成一个新的结点。将当前结点指向父结点，重复第4步。

注意，通过**B+树的删除操作后，索引结点中存在的key，不一定在叶子结点中存在对应的记录。**

![image-20200811212912840](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-42-54-bb7adb4e2f10585f23c4fd4fddd9eec0-image-20200811212912840-070383.png)

# 贪心算法

* 通过局部最优（贪婪）选择可以得到全局最优解
* 最优解包含子问题的最优解。

贪心算法，在求解过程中，并不追求全局最优解，而是追求每一步的最优，所以贪心算法也不保证一定能够获得全局最优解，但是贪心算法在很多问题却额可以求得最优解。

算法思路：

​	贪心算法的基本思路是从问题的某一个初始解出发一步一步地进行，根据某个优化测度，每一步都要确保能获得局部最优解。每一步只考虑一个数据，他的选取应该满足局部优化的条件。若下一个数据和部分最优解连在一起不再是可行解时，就不把该数据添加到部分解中，直到把所有数据枚举完，或者不能再添加算法停止

## 活动选择问题

问题描述：假定一个有n个活动(activity)的集合S={a1,a2,....,an}，这些活动使用同一个资源（例如同一个阶梯教室），而这个资源在某个时刻只能供一个活动使用。每个活动ai都有一个开始时间si和一个结束时间fi，其中0<=si<fi<正无穷。如果被选中国，任务ai发生在半开时间区间[si,fi)期间。如果两个活动ai和aj满足[si,fi)和[sj,fj)不重叠，则称它们是兼容的。也就说，若si>=fj或sj>=fi，则ai和aj是兼容的。在活动选择问题中，我们希望选出一个最大兼容活动集。假定活动已按结束时间fi的单调递增顺序排序。

算法思路：假设：Sij表示在ai结束之后，在aj开始之前的活动的集合。Aij表示Sij的一个最大相互兼容的活动子集。那么只要Sij非空，则Aij至少会包含一个活动，假设为ak。那么可以将Aij分解为：Aij = Aik+ak+Akj。假设Cij为Aij的大小，那么有Cij=cik+ckj+1。但是我们并不知道具体是k等于多少的时候，可以让ak一定位于Aij中，所以我们采用动态规划的方式，遍历所有可能的k值，来取得。于是有：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-43-04-1cc40fc6c462bd098740aa9e469104e2-image-20200812090039805-4a6c6c.png" alt="image-20200812090039805" style="zoom:67%;" />

贪心算法却要简单许多，贪心算法直接在每一步选择当前看来最好的选择。比如在一开始的时候，我们要选择在Aij中的第一个活动，我们**选择活动结束时间最早的那个活动**，这样能够给其他活动尽可能的腾出多余的时间。而后每一步都在剩下的活动中选取，也遵循类似的原则。由于获取已经按照fi排序好，所以这里第一个选择的活动就是a1。但是问题来了，我们能否确定a1一定在Aij中呢？可以证明：

假设Aij是Sij的某个最大兼容活动集，假设Aij中，最早结束的活动是an，分两种情况：

​    ①如果an=a1，则得证

​    ②如果an不等于a1，则an的结束时间一定会晚于a1的结束时间，我们用a1去替换Aij中的an，于是得到A`，由于a1比an结束的早，而Aij中的其他活动都比an的结束时间开始 的要晚，所以A`中的其他活动 都与a1不想交，所以A`中的所有活动是兼容的，所以A`也是Sij的一个最大兼容活动集。

​    于是证明了命题。

## 最小生成树

连通图G的生成树：包含G所有顶点的连通无环子图

加权连通图G的最小生成树：G的最小总权生成树，即将给出的所有点连接起来（即从一个点可到任意一个点），且连接路径之和最小的图叫最小生成树。

无向图中，如果任**意两个顶点之间都能够连通**，则称此无向图为连通图。例如，图 2 中的无向图就是一个连通图，因为此图中任意两顶点之间都是连通的

两种算法求最小生成树：**Prim算法**和**Kruskal算法**

### Prim算法

Prim算法构建最小生成树的过程是：先构建一棵只包含根结点V1的树A，然后每次**在连接树A结点和图G中树A以外的结点的所有边中**，**选取一条权重最小的边**加入树A，**直至树A覆盖图G中的所有结点**。

算法复杂度：邻接矩阵表示时为O(n^2)，邻接表表示时为O(mlogn)，m表示边数，n表示顶点数

算法伪代码：

![image-20200812091938004](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-43-48-4b93d406ca94411f1bc3d2116fffe7e4-image-20200812091938004-4c88e9.png)

###  Kruskal算法

Kruskal算法的思想是令**T的初始状态为|V|个结点而无边的非连通图**，T中的**每个顶点自成一个连通分量**。接着，每次**从图G中所有两个端点落在不同连通分量的边中，选取权重最小的那条**，将该边加入T中，如此往复，**直至T中所有顶点都在同一个连通分量上**。

算法难点在于如何判断一条边的两个端点是否落在不同的连通分量上：递归查找边的父端点，如果遇到了自己，则位于同一个连通分量上

算法复杂度：O(mlogn)，图有m条边。

算法伪代码：

![image-20200812092514143](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-43-53-bca747852f43c098d56afe9b3d63954b-image-20200812092514143-4a80e9.png)

## 单源最短路径问题

单源最短路径问题：给定一个加权连通图G，求从源顶点s到其他每个顶点的最短路径

### Dijkstra算法

算法描述：设G=(V,E)是一个带权有向图，把图中**顶点集合V分成两组**，**第一组为已求出最短路径的顶点集合**（用S表示，初始时S中只有一个源点，以后每求得一条最短路径 , 就将加入到集合S中，直到全部顶点都加入到S中，算法就结束了），**第二组为其余未确定最短路径的顶点集合（用U表示）**，**按最短路径长度的递增次序依次把第二组的顶点加入S中**。在**加入的过程中，总保持从源点v到S中各顶点的最短路径长度不大于从源点v到U中任何顶点的最短路径长度。**此外，每个顶点对应一个距离，**S中的顶点的距离就是从v到此顶点的最短路径长度**，U中的顶点的距离，是从v到此顶点只包括S中的顶点为中间顶点的当前最短路径长度。

算法步骤：

​	a.初始时，S只包含源点，即S＝{v}，v的距离为0。U包含除v外的其他顶点，即:U={其余顶点}，若v与U中顶点u有边，则<u,v>正常有权值，若u不是v的出边邻接点，则<u,v>权值为∞。

​	b.**从U中选取一个距离v最小的顶点k**，把**k加入S中**（该选定的距离就是v到k的最短路径长度）。

​	c.**以k为新考虑的中间点**，**修改U中各顶点的距离**；若从源点v到顶点u的距离（经过顶点k）比原来距离（不经过顶点k）短，则修改顶点u的距离值，**修改后的距离值为顶点k的距离加上边上的权**。

​	d.重复步骤b和c直到所有顶点都包含在S中。

算法图解：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-01-ce228277d17917ada7d1acc9d9df2cb3-2012073019540660-bd16a7.gif)

![image-20200812094541747](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-04-72574746e0eab98ee5fdb749aa5433b4-image-20200812094541747-9f84a1.png)

* 不适用于具有负权重的图
* 无向图和有向图都适用

算法复杂度：用邻接矩阵表示的图，复杂度为O(|V|²)，用邻接表及最小堆优化后复杂度为O(ElogV)

## 最优二路归并树

问题描述：给定n个有序文件，每个文件的记录数分别为w1~wn，请给出一种两两合并的方案，使得总合并次数最少。

1. 外排序算法是将多个有序文件合并成一个有序文件的过程。
2. 在一次合并的过程中，两个文件中的所有记录都需要先从文件中读入内存，再在内存中排序，最后将排序的结果写入文件中。
3. 假设两个待排序文件记录数分别为n、m，那么将这两个文件合并成一个有序的文件需要进行n+m次读写。

问题转化：n个文件两两合并的过程可以用一棵扩充二叉树来表示。因为扩充二叉树只有度为2或0的节点，没有度为1的节点，这符合两两合并的过程。

![image-20200812100554379](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-11-8ded518473f98a704298e9fc0fa171d9-image-20200812100554379-7891de.png)

在这棵扩充二叉树中：

1. 方形节点（外界点）表示原始的文件，圆形节点（内节点）表示合并过程中的文件；
2. 节点的权值表示文件的记录数

因此，n个文件合并过程的总读写次数为带权外路径长度之和，即非叶节点的权值和（15+35+95+60）。要求最小的合并次数即为求最小的带权外路径长度之和。
因此，问题就转化为『如何求扩充二叉树的最小加权路径』。这个问题可以用哈夫曼算法解决。

生成该二叉树的复杂度为：O(nlogn)

### Huffman编码问题

**哈夫曼（Huffman）编码算法**是基于二叉树构建编码压缩结构的，它是数据压缩中经典的一种算法。算法根据文本字符出现的频率，重新对字符进行编码。因为为了缩短编码的长度，我们自然希望**频率越高的词，编码越短**，这样最终才能最大化压缩存储文本数据的空间。

示例：![image-20200812101738753](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-15-2a15516775109f0ba20f0047d45e79d4-image-20200812101738753-acb97d.png)

每次都归并权值最小的结点，最后对生成的树进行编码，对应的编码就是最优的压缩编码，可以用优先队列来实现该算法。

算法复杂度：O(nlogn)

算法伪代码：

![image-20200812102019484](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-18-fb7bda2fd95a28a13e928c542dcfee9d-image-20200812102019484-69d9e3.png)

C是带频率的字符队列，Q是一个优先队列。

## 最小圈基（cycle basis）问题

圈基：是图G的一个循环边（圈）的集合，这个集合中的圈经过⊕操作可以得到图G的任一圈。A⊕B=(A∪B)-(A∩B)。

![image-20200812104734918](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-22-5287e151315802946e75953f1e37aeb5-image-20200812104734918-8e3a45.png)

最小圈基就是求**边权重和最小的圈基**

算法步骤：

1. 确定最小圈基的大小，设为k

   k=|E|-(|V|-1)

2. 找到所有圈基，按权重大小排序

3. 将圈基以此加入最小圈基集中，如果已加入的圈基可以通过⊕得到新加入的圈基，则删除新加入的圈基

   可以用高斯消除来解决这一步：

   ![image-20200812105354894](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-26-3665d77c347735497cd1b1d908e87649-image-20200812105354894-47d5c5.png)

4. 当最小圈基中的圈基数达到k时结束

算法示例：

![image-20200812105718752](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-29-64e9100c8d3ea47ce2aad5aa39f61554-image-20200812105718752-b40775.png)

# 动态规划

定义：动态规划算法是通过拆分问题，**定义问题状态和状态之间的关系**，使得问题能够以递推（或者说分治）的方式去解决。
动态规划算法的基本思想**与分治法类似**，也是**将待求解的问题分解为若干个子问题（阶段），按顺序求解子阶段，前一子问题的解，为后一子问题的求解提供了有用的信息**。在求解任一子问题时，**列出各种可能的局部解，通过决策保留那些有可能达到最优的局部解，丢弃其他局部解。**依次解决各子问题，最后一个子问题就是初始问题的解。

算法思想：由于动态规划解决的问题多数有重叠子问题这个特点，为减少重复计算，对每一个子问题只解一次，将其不同阶段的不同状态保存在一个二维数组中。

## 最优二叉搜索树

问题描述：给出各个节点和各个节点的被查找概率，然后构造一棵各个节点平均被查找比较次数最小的树，则该问题可以用动态规划来解决。

![image-20200812132315160](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-33-ce10c684bd4219a06d191176a3695a8f-image-20200812132315160-0bd3a9.png)

例如：

![image-20200812132339121](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-36-cb67c6d7a9fe923bd0c3178f72dab88d-image-20200812132339121-96c0c8.png)

推广到一般的情况，并设T(i, j)是由记录{ri, …, rj}(1≤i≤j≤n)构成的二叉查找树，C(i, j)是这棵二叉查找树的平均比较次数，有下列分析

![image-20200812132418367](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-38-fd93f30ee0eb9981ffedbebb09f3f3ac-image-20200812132418367-4e2e19.png)

![image-20200812132455180](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-41-578644eda69a4c4c04d3a678d2b8dc40-image-20200812132455180-0669cd.png)

最后一步，使用了递归的方法来化简。因此，得到以下动态规划函数：

![image-20200812133032436](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-44-ac88e1585fb6518293d3c670fa660a81-image-20200812133032436-5fae9b.png)

C(i,i) = pi, C(i, i-1) = 0

![image-20200812133154782](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-46-db101a7243af14b2e1e618f88366d650-image-20200812133154782-97fe27.png)

![image-20200812133324871](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-49-5f433843a707b5fec4fea7319fbc321a-image-20200812133324871-1f2c6d.png)

![image-20200812133836245](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-52-349c7fab319fb18b3bacf16acef4d523-image-20200812133836245-6d81ef.png)

![image-20200812134259828](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-55-b0bf764968647ca462ef662f95f7402b-image-20200812134259828-f83c43.png)

观察这个表，可知可知**左边的表的第一行的第四列就是我们要求的最优平均比较次数**，而**右边的表我们可以知道在c（i ,j）得到最优解，即平均查找次数最小的根节点**，比如一共四个节点，则我们从右边的R(1,4)的值即3是这四个节点构成的树的根节点。则树的左子树变为c(1,2),他的根节点是r(1,2)=2，然后2又有左节点1，而4则是3的右节点。则树的样子便出来了。

算法伪代码：

![image-20200812134818633](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-44-58-c78e76f78ee4c43c3b3c9f129efb9cf8-image-20200812134818633-3f81c8.png)

复杂度：Θ(n^2)

## 背包问题

题目：有一个容量为 V 的背包，和一些物品。这些物品分别有两个属性，体积 w 和价值 v，每种物品只有一个。要求用这个背包装下价值尽可能多的物品，求该最大价值，背包可以不被装满。 

0-1背包问题：在最优解中，每个物品只有两种可能的情况，即在背包中或者不在背包中（背包中的该物品数为0或1），因此称为0-1背包问题。

算法步骤：

1. **找子问题：**子问题必然是和物品有关的，对于每一个物品，有两种结果：能装下或者不能装下。第一，包的容量比物品体积小，装不下，这时的最大价值和前i-1个物品的最大价值是一样的。第二，还有足够的容量装下该物品，但是装了不一定大于当前相同体积的最优价值，所以要进行比较。由上述分析，子问题中物品数和背包容量都应当作为变量。因此**子问题确定为背包容量为j时，求前i个物品所能达到最大价值**。
2. **确定状态：**由上述分析，“状态”对应的“值”即为背包容量为j时，求前i个物品所能达到最大价值，设为dp\[i\]\[j\]。初始时，dp\[0\]\[j\](0<=j<=V)为0，没有物品也就没有价值。
3. **确定状态转移方程：**由上述分析，第i个物品的体积为w,价值为v，则状态转移方程为

- **j<w，dp\[i\]\[j\] = dp\[i-1\]\[j\]** //背包装不下该物品，最大价值不变
- **j>=w, dp\[i\]\[j\] = max{ dp\[i-1\]\[j-list\[i\].w\] + v, dp\[i-1\]\[j\] }** //和不放入该物品时同样达到该体积的最大价值比较

示例：

![image-20200812140408097](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-11-0af8f7e0f33561f9755ea6fcd78d0812-image-20200812140408097-25b450.png)

算法伪代码：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-16-69be62e558a80d8d72c2ba721889b228-image-20200812140846321-6df932.png" alt="image-20200812140846321" style="zoom:67%;" />

观察状态转移方程的特点，我们发现dp\[i\]\[j\]的转移只与dp\[i-1\]\[j-list\[i\].w\]和dp\[i-1\]\[j\]有关，即**仅与二维数组本行的上一行有关**。因此，我们**可以将二维数组优化为一维数组**。不过这里要注意两点：1.j<w的状态转移方程不再需要了。2.为保证每个物品只能使用一次，我们**倒序遍历所有j的值**，这样在更新dp[j]的时候，dp\[j-list\[i\].w\]的值尚未被修改，就不会出现一个物品重复使用的问题。

优化后的算法伪代码：

![image-20200812142739075](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-19-959cfe54cc29887d6cf8775f05df22db-image-20200812142739075-c6d7fb.png)

## 传递闭包算法

传递闭包：存在一个有向图，能用布尔邻接矩阵表示（1、0）。存在一个矩阵，它能够给定图的顶点之间是否存在任意长度的有向路径，这种矩阵称为有向图的传递闭包，是我们能够**在常数时间内判断第j个顶点是否可从第i个顶点到达。**

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-35-3ffbb72963faceacf988a68fb21b0df4-image-20200812143039606-c60d39.png" alt="image-20200812143039606" style="zoom:67%;" />

### Warshall算法

算法描述：存在 (a,b)=1 (b,c)=1，则(a,c)=1，以此为基础遍历完整个矩阵，如图。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-38-f93d9d0a6209838f732b8bbb326ab2bb-11546496-97d6be5b2f0eb4ab-79f3ce.png" alt="img" style="zoom:67%;" />

状态转移关系为：![image-20200812143930170](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-50-173a7cb8e70827ef44fe3874f3a3befa-image-20200812143930170-2416da.png)

因此，从R(k-1)到R(k)主要遵循以下两点：

1. 如果i行和j列中的元素在R（k-1）中为1，则在R（k）中保持1
2. 如果R（k-1）中i行和j列中的某个元素为0，则当且仅当其第i行和第k列中的元素及其第k行和第j列中的元素在R（k-1）中都是1时，R（k）中将其值更改为1

算法伪代码：

![image-20200812145037645](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-54-62e0322ecee44f830716010df5cfe022-image-20200812145037645-ea81bb.png)

复杂度：Θ(n^3)

### Floyd算法

若一个点到另外一个点没有路径，则把它的值视为无穷大，

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-45-58-255266cd66d8682f6e07e28cdad066e5-11546496-84973c4ccfbe7943-575d83.png" alt="img" style="zoom:67%;" />

状态转移方程：![image-20200812145547445](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-00-b73b60649d8db7bf909ba26023443eaa-image-20200812145547445-060295.png)

算法伪代码：

![image-20200812145631966](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-03-2855c81870268ec4d1cbc383a17bc613-image-20200812145631966-af2882.png)

复杂度：Θ(n^3)

## 动态规划与分治

* 这两种方法都基于将问题实例划分为同一问题的较小实例。
* 通常，分治将一个实例分成**没有交集**的较小实例，而动态规划则处理较小实例重叠的问题
* 因此，分治算法不会存储较小示例的解决方案而动态规划算法会这样做。

## 动态规划与贪心算法

* 动态规划以自下而上的方式使用最优子结构，首先找到子问题的最优解，在解决了子问题之后，我们找到了问题的最优解。
* 贪婪算法以自上而下的方式使用最优子结构，首先做一个选择，这个选择是最优解，然后求出这子问题的最优解

## 多段图问题

问题描述：若存在一个有向加权图G，且G能分出**起点**和**终点**以及中间的**n的阶段**，求起点到终点的**最短（长）距离**。

![image-20200812150405123](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-07-d2a35bd07abd71a45b68d40f7f44b04b-image-20200812150405123-2b648f.png)

贪心算法求得S-A-D-T ->1+4+18 = 23，而实际最短路径为S-C-F-T ->5+2+2=9

动态规划解决该问题的主要思想：n个阶段的大问题很难求解，可以将其进行划分成子问题：n-1个阶段的多段图……1个阶段的子问题容易解决，就能解决整个问题。也就是说每一个阶段都是整个问题的一个**最优子结构**。

如果从点s到点t的最短路径经过点vi，则vi到t也是最短距离。

* 逆向推理解决该问题：

  ![image-20200812151420646](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-11-929dcee6701755f125ec9556f3996c83-image-20200812151420646-3b27d9.png)

![image-20200812151600592](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-14-6b72150f5024d2f5efdb2603e28c8408-image-20200812151600592-824882.png)

* 正向推理解决该问题：

  ![image-20200812151623971](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-21-3be0e5299dd345dad97237ddcd40a67c-image-20200812151623971-98c183.png)

![image-20200812151658375](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-25-020a55d40486c135c603416399ece152-image-20200812151658375-d9e407.png)

总结：综上所述，如果一个问题可以用多级图来描述，那么它就可以用动态规划来求解。

例如：

* 最短路径问题：如果i，i1，i2，…，j是从i到j的最短路径，那么i1，i2，…，j一定是从i1到j的最短路径

## 最优化原则

动态规划适用于**问题的最优解包含所有子问题的最优解**，最优性原则并不是说如果所有子问题都是最优解，那么你可以把它们组合起来得到一个全局最优解

例如：找最长的路径

![image-20200812152512468](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-28-81f09a43415f1214aea14144b884066c-image-20200812152512468-ecd85b.png)

从A到D的最长路径是A-B-C-D，然而A-B的最长路径并不是A-B，而是A-C-B，因此不能用动态规划来解决这个问题。

## 最长公共字符子序列（LCS）

问题描述字符序列的子序列是指从给定字符序列中随意地（不一定连续）去掉若干个字符（可能一个也不去掉）后所形成的字符序列。令给定的字符序列X=“x0，x1，…，xm-1”，序列Y=“y0，y1，…，yk-1”是X的子序列，存在X的一个严格递增下标序列<i0，i1，…，ik-1>，使得对所有的j=0，1，…，k-1，有xij=yj。例如，X=“ABCBDAB”，Y=“BCDB”是X的一个子序列

思路：考虑最长公共子序列问题如何分解成子问题，设A=“a0，a1，…，am-1”，B=“b0，b1，…，bn-1”，并Z=“z0，z1，…，zk-1”为它们的最长公共子序列。不难证明有以下性质：

（1） 如果am-1=bn-1，则zk-1=am-1=bn-1，且“z0，z1，…，zk-2”是“a0，a1，…，am-2”和“b0，b1，…，bn-2”的一个最长公共子序列；

（2） 如果am-1!=bn-1，则若zk-1!=am-1，蕴涵“z0，z1，…，zk-1”是“a0，a1，…，am-2”和“b0，b1，…，bn-1”的一个最长公共子序列；

（3） 如果am-1!=bn-1，则若zk-1!=bn-1，蕴涵“z0，z1，…，zk-1”是“a0，a1，…，am-1”和“b0，b1，…，bn-2”的一个最长公共子序列。

这样，在找A和B的公共子序列时，如有am-1=bn-1，则进一步解决一个子问题，找“a0，a1，…，am-2”和“b0，b1，…，bm-2”的一个最长公共子序列；如果am-1!=bn-1，则要解决两个子问题，找出“a0，a1，…，am-2”和“b0，b1，…，bn-1”的一个最长公共子序列和找出“a0，a1，…，am-1”和“b0，b1，…，bn-2”的一个最长公共子序列，再取两者中较长者作为A和B的最长公共子序列。

求解：引进一个二维数组c\[\]\[\]，用c\[i\]\[j\]记录X\[i\]与Y\[j\] 的LCS 的长度，b\[i\]\[j\]记录c\[i\]\[j\]是通过哪一个子问题的值求得的，以决定搜索的方向。
我们是自底向上进行递推计算，那么在计算c\[i,j\]之前，c\[i-1\]\[j-1\]，c\[i-1\]\[j\]与c\[i\]\[j-1\]均已计算出来。此时我们根据X\[i\] = Y\[j\]还是X\[i\] != Y\[j\]，就可以计算出c\[i\]\[j\]。

问题的递归式写成：

![image-20200812154303273](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-32-292d28fd5226db9f3e42784ed67a3692-image-20200812154303273-28077f.png)

算法伪代码：

![image-20200812154643072](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-34-ac9a8856487e2acd4228deef6d956f53-image-20200812154643072-62df4c.png)

示例：

![image-20200812154811424](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-37-1d9271f5e5492edc661101d642a0ed83-image-20200812154811424-737291.png)

时间复杂度：O(mn)，空间复杂度O(mn)

回溯算法伪代码：

![image-20200812155044156](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-41-32cfd3ecce0ab08eddb2c838223990d8-image-20200812155044156-2c402b.png)

复杂度O(m+n)

## 装配线调度问题(ALS)

问题描述：假设一个汽车底盘加工有两个装配线，如下如所示，每个装配线都有n个配件站，用于给底盘安装不同的零件，配件站用S表示，如S(2,3)表示第二条装配线的第三个配件站。两条装配线同一个配件站的工作相同，但是时间不同，用a表示所花费的时间，如a(2,3)表示装配站2的第三个配件站完成工作的时间。底盘进入装配线的时间用e表示，离开装配线的时间用x表示。

![image-20200812155457998](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-44-600ecfc1cda9472515303c0291daf3b4-image-20200812155457998-56e3fc.png)

思路：

1. 第一步其实就是要描述最优解结构的特征，假设底盘到达配件站S(1,j)的路径是最短的，那么到达这个配件站的前一个配件站可以是S(1,j-1)或者是S(2,j-1)，当然，到达S(1,j-1)或者S(2,j-1)的路径同样是最短的。**到达S(1,j)的最优解包含了到达S(1,j-1)或者是S(2,j-1)的最优解**，我们称这个性质为最优子结构，这也是能否使用动态规划的一个标志。

   下面就是利用最优子结构来进行说明，用子问题的一个最优解来构造原问题的最优解。通过配件站S(1,j)的最快路线，必然经过装配线1或者2的j-1的配件站，因此，通过S(1,j)的最快路线只能是以下两种选择：

   ·通过配件站S(1,j-1)，然后直接到达S(1,j)。

   ·通过配件站S(2,j-1)，然后从装配线2到装配线1，最后到达S(1,j)。

   当然，对于配件站S(2,j)的路径是对称的。所以，要解决**到达某一个配件站的最短时间，就要解决两个子问题的最短时间问题**，而通过对子问题的求解就可以构造出问题的最优解。

2. 在动态规划的第二步中，就是通过对子问题的最优解来递归定义一个最优解，在装配线问题中，我们就选择达到配件站j的最快路线的问题作为子问题，令fi\[j\]表示底盘从起点到达配件站S(i,j)的最快时间，而我们的目的就是确定底盘到达最后出口的最短时间，记为f*，每条装配线的配件站的数目都是n，于是有：

![image-20200812155832899](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-48-cfa0bd49e1406a8c5cea648a988811f2-image-20200812155832899-a56ef4.png)

​	对于f(1,1)和f(2,1)的推导也是很容易：

![image-20200812155933487](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-50-f92c131669b7ae813c2235853231f7b7-image-20200812155933487-c32589.png)

​	现在的问题就是该如何计算f(i,j)，而f(i,j)就是之前所述的直接通过同一装配线的上一个配件站或者通过不同装配线然后到达S(i,j)，于是就有：

​	![image-20200812155959232](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-52-21a9d33efc40bfa1f654395fac873296-image-20200812155959232-647cea.png)

f(i,j)的值就是子问题最优解的值，为了有助于跟踪最优解的构造过程，定义l(i,j)为装配线的编号（1或2），其中的配件站j-1被通过配件站S(i,j)的的最快路径所使用。举个例子:

![image-20200812160439808](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-46-58-4899c66dfdc8f3ae8896faaa7602a842-image-20200812160439808-6276e0.png)

计算步骤伪代码：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-01-a15297184abc9f628baae01ec96f25ca-20130808153539781-8952c0)

回溯步骤伪代码：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-05-05dd04fa5af3ed400b44ce4852435d5c-20130808154257500-121d9e)

## 矩阵链乘法

问题描述：求解矩阵链相乘问题时动态规划算法的另一个例子。给定一个n个矩阵的序列（矩阵链）<A1,A2,...,An>，我们希望计算它们的乘积  A1A2...An，为了计算表达式，我们可以先用括号明确计算次序，然后利用标准的矩阵相乘算法进行计算。

以矩阵链<A1,A2,A3>为例，来说明不同的加括号方式会导致不同的计算代价。假设三个矩阵的规模分别为10×100、100×5和5×50。

* 如果按照((A1A2)A3)的顺序计算，为计算A1A2(规模10×5)，需要做10\*100\*5=5000次标量乘法，再与A3相乘又需要做10\*5\*50=2500次标量乘法，共需**7500**次标量乘法。
* 如果按照(A1(A2A3))的顺序计算，为计算A2A3(规模100×50)，需100\*5\*50=25000次标量乘法，再与A1相乘又需10\*100\*50=50000次标量乘法，共需**75000**次标量乘法。因此第一种顺序计算要比第二种顺序计算**快10倍**。

**矩阵链乘法问题**(matrix-chain multiplication problem)可描述如下：给定n个矩阵的链<A1,A2,...,An>，矩阵Ai的规模为p(i-1)×p(i) (1<=i<=n)，求完全括号化方案，使得计算乘积A1A2...An所需标量乘法**次数最少**。

算法步骤：

1. **最优括号化方案的结构特征**：在矩阵链乘法问题中，我们假设A(i)A(i+1)...A(j)的最优括号方案的分割点在A(k)和A(k+1)之间。那么，继续对“前缀”子链A(i)A(i+1)..A(k)进行括号化时，我们应该直接采用**独立求解**它时所得的最优方案。我们已经看到，一个非平凡(i≠j)的矩阵链乘法问题实例的任何解都需要划分链，而任何最优解都是由子问题实例的最优解构成的。**为了构造一个矩阵链乘法问题实例的最优解**，我们可以将问题**划分为两个子问题(A(i)A(i+1)...A(k)和A(k+1)A(k+2)..A(j)的最优括号化问题)**，**求出子问题实例的最优解，然后将子问题的最优解组合起来**。我们必须保证在确定分割点时，已经考察了所有可能的划分点，这样就可以保证不会遗漏最优解。

2. **递归求解方案**：对于矩阵链乘法问题，我们可以将对所有1<=i<=j<=n确定A(i)A(i+1)...A(j)的最小代价括号化方案作为子问题。令m[i,j]表示计算矩阵A(i..j)所需标量乘法次数的最小值，那么，原问题的最优解—计算A(1..n)所需的最低代价就是m[1,n]。

   我们可以递归定义m[i,j]如下。对于**i=j时**的平凡问题，矩阵链只包含唯一的矩阵A(i..j)=A(i)，因此**不需要做任何标量乘法运算**。所以，对所有i=1,2,...,n，**m[i,i]=0**。若i<j，我们利用步骤1中得到的最优子结构来计算m[i,j]。我们假设A(i)A(i+1)...A(j)的最优括号化方案的**分割点**在矩阵A(k)和A(k+1)之间，其中i<=k<j。那么，**m[i,j]就等于计算A(i..k)和A(k+1..j)的代价加上两者相乘的代价的最小值**。由于矩阵**Ai的大小为p(i-1)\*pi**，易知A(i..k)和A(k+1..j)相乘的代价为p(i-1)p(k)p(j)次标量乘法运算。因此，我们得到

   ![image-20200812162033907](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-11-81057c89d7bd25d0cfc94636f7eac45e-image-20200812162033907-2ad10b.png)

   m[i,j]的值给出了子问题最优解的代价，但它并未提供足够的信息来构造最优解。为此，我们用**s[i,j]保存最优括号化方案的分割点位置k**，即使得m[i,j]=m[i,k]+[k+1,j]+p(i-1)p(k)p(j)成立的k值。

3. **计算最优代价**：注意到，我们需要求解的不同子问题的数目是相对较少的：每对满足1<=i<=j<=n 的i和j对应一个唯一的子问题，共有n^2(最少)。递归算法会在递归调用树的不同分支中多次遇到同一个子问题。这种**子问题重叠**的性质是应用动态规划的另一标识(第一个标识是最优子结构)。

   我们采用自底向上表格法代替递归算法来计算最优代价。此过程假定矩阵Ai的规模为p(i-1)×pi(i=1,2,...,n)。它的输入是一个序列p=<p0,p1,...,pn>，其长度为p.length=n+1。过程用一个辅助表m[1..n,1..n]来保存代价m[i,j]，用另一个辅助表s\[1..n-1,2..n\](s\[1,2\]..s\[n-1,n\]这里i<j)记录最优值m[i,j]对应的分割点k。我们就可以利用表s构造最优解。

   对于矩阵A(i)A(i+1)...A(j)最优括号化的子问题，我们认为其规模为链的**长度j-i+1**。因为j-i+1个矩阵链相乘的最优计算代价m[i,j]只依赖于那么少于j-i+1个矩阵链相乘的最优计算代价。因此，算法应该**按长度递增**的顺序求解矩阵链括号化问题，并按对应的顺序填写表m。

   <img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-14-407e9dc114c8954a13119b1551c7c5ea-image-20200812164742921-c72041.png" alt="image-20200812164742921" style="zoom:67%;" />

   伪代码：

   ![image-20200812164827176](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-16-50b9d85b6a38dc10dc5cbc142a0562ef-image-20200812164827176-41c548.png)

4. **造最优解**：虽然MATRIX_CHAIN_ORDER求出了计算矩阵链乘积所需的最少标量乘法运算次数，但它并未直接指出如何进行这种最优代价的矩阵链乘法计算。表s[i,j]记录了一个k值，指出A(i)A(i+1)...A(j)的最优括号化方案的分割点应在A(k)和A(k+1)之间。因此，我们A(1..n)的最优计算方案中最后一次矩阵乘法运算应该是以**s[1,n]为分界**的A(1..s[1,n])*A(s[1,n]+1..n)。我们可以用相同的方法**递归**地求出更早的矩阵乘法的具体计算过程，因为s[1,s[1,n]]指出了计算A(1..s[1,n])时应进行的最后一次矩阵乘法运行；s[s[1,n]+1,n]指出了计算A(s[1,n]+1..n)时应进行的最后一次矩阵乘法运算。



![image-20200812164809124](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-19-3c9f0903691dcb7263116f729b73c41b-image-20200812164809124-dde0cf.png)

​	代码：

![image-20200812164935229](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-23-e1403e066a4d28f011d65f3b1e421c26-image-20200812164935229-d61c16.png)

# 树搜索策略

许多实际应用中的计算问题均是NP-完全问题，这些问题不存在多项式时间算法，除非NP=P。**当NP-完全问题具有较小的输入规模**时，**可以穷举问题的解空间**。而我们常常将问题的解空间表示成搜索树。

## **广度优先搜索（BFS）**

使用队列(Queue)

> 1. 构造仅含树根节点的队列Q;
> 2. 若队列Q的第一个节点x是目标节点，则输出节点x对应的解，算法结束
> 3. 删除队列Q的第一个节点x，如果绑定删除B(x)判定以x为根的子树可能存在解，则将x的所有孩子节点加入队列Q的末尾；
> 4. 如果队列Q为空，则问题无解，算法结束，否则，转到第2步；

### 八数码难题

![image-20200812184936645](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-26-7d8e14a6a3f32aebf00814c2645aa788-image-20200812184936645-fea2f7.png)

## **深度优先搜索（DFS）**

使用栈(Stack)

> 1. 构造仅含树根节点的栈S;
> 2. 如果栈顶元素x是目标节点，则输出节点x对应的解，算法结束；
> 3. 弹出栈顶元素x，如果绑定函数B(x)判定以x为根的子树可能存在解，则将x的所有孩子一次压入栈
> 4. 如果栈S为空，则问题无解，算法结束，否则，转到第2步；

### 子集和

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-29-7bb5cac4603eef078d0828dc19f0a0bc-image-20200812185422005-cd6bcd.png" alt="image-20200812185422005" style="zoom:67%;" />

## **爬山法**

深搜+贪心，该方法选择局部最优节点进行扩展。

> 1. 构造仅含树根节点的栈S;
> 2. 如果栈顶元素x是目标节点，则输出节点x对应的解，算法结束
> 3. 弹出栈顶元素x，如果绑定函数B(x)判定以x为根的子树可能存在解；则将x的所有孩子节点按数值P()从大到小依次压入栈
> 4. 如果栈S为空，则问题无解，算法结束，否则，转到第2步-

两个关键点：

1. 使用的基础搜索算法是*深度优先（DFS）* 。
2. 使用一个*测度函数*，排序孩子节的遍历顺序（入栈顺序）。

测度函数往往是根据启发式规则来制定。

例如：

8-魔方问题使用爬山法解决，测度函数可以定义为，选择的状态与最终想要的状态的不同的个数。越小越优先选择（当然越后入栈）。

*爬山法能够在测度函数度量下最快得到局部最优解。（贪心）*所以往往可以将爬上法再加上后面要说的*分支界限法*

### 八数码难题

每次都选择最优的结点，在该问题中，评估一个结点的函数为：f(n) = d(n) + w(n)，d(n)是结点的深度,w(n)是节点中错误的位置数，每次选取f(n)最小的结点进行深度搜索

![image-20200812201441820](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-33-17956ccfa53ec0db7ca414152fe0c836-image-20200812201441820-35082a.png)

爬山法的步骤：

1. 构建一个由根节点组成的堆栈。
2. 测试堆栈中的顶层元素是否为目标节点。如果是，请停止；否则，转至步骤3
3. 从堆栈中移除顶层元素并展开元素。将元素的后代添加到按求值函数排序的堆栈中
4. 如果堆栈为空，则失败。否则，转至步骤2。

## **最佳优先搜索(best-first-search)**

结合了深度优先搜索和广度搜索的优点
使用堆

> 1. 构造仅含树根节点的堆Q;
> 2. 如果堆顶元素x是目标节点，则输出节点x对应的解，算法结束；
> 3. 抽取堆顶元素x，如果绑定函数B(x)判定x以x为根的子树可能存在解，则将x的所有孩子节点插入堆
> 4. 如果堆Q为空，则问题无解，算法结束，否则，转到第2步；

**Best-First策略**是根据一个**评价函数**，在目前产生的**所有节点中选择具有最小评价函数值的节点进行扩展**。它结合了**深度优先搜索**和**广度优先搜索**的优点，具有**全局化观念**，而爬山法仅仅具有局部优化观念。

### 八数码难题

![image-20200812201924452](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-36-2a52f3e98c23341574b7ec76189880cb-image-20200812201924452-8d8e1c.png)

与爬山法的解决思路相比，先列出子节点的所有可能解，再取最好的子节点继续使用深度优先搜索

算法步骤：

1. 形成一个由根节点组成的单元素列表
2. 从列表中删除第一个元素。展开第一个元素。如果第一个元素的子元素之一是目标节点，则停止；否则，将子元素添加到列表中
3. 根据某个估值函数的值对整个列表进行排序
4. 如果列表为空，则失败。否则，转至步骤2。


## 回溯法

回溯法解题时通常包含3个步骤：

1. 针对所给问题，定义问题的解空间；

2. 确定易于搜索的解空间结构；

3. 以**深度优先方式**搜索解空间，并**在搜索过程中用剪枝函数避免无效搜索**。

伪代码：

![image-20200812194305535](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-40-ce03a5b989e860a5035f32fd6998cb3a-image-20200812194305535-5fd3cd.png)

### n-皇后问题

把n个皇后放在一个n乘n的棋盘上，确保不会有两个皇后出现在在同一行、同一列或同一个对角线上

解法：

![image-20200812185717147](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-43-361c3cec47a555f8c84ce7b1c5b8c3d5-image-20200812185717147-9e2cec.png)

### 哈密顿回路

对于一个有向图G=(V,E),如果G中的圈C恰好经过每一个顶点一次，则称圈C是一个哈密顿圈。即，哈密顿圈构成一条经过所有的顶点，没有重复的“路线”。如图是一个含有哈密顿圈的图。

![image-20200812185940439](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-46-0d66587b63dc5824e548b879dbf9b319-image-20200812185940439-9aea54.png)

解法：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-48-7637992bdf8735d09fd03b46d1319ddb-image-20200812191540226-bdf491.png" alt="image-20200812191540226" style="zoom:67%;" />

### 子集和

![image-20200812194125573](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-47-51-7c1356c5e06b403e88abe3040f81ce71-image-20200812194125573-ad15ce.png)

## **分支限界法**

### 分支限界法与回溯法的区别

回溯法（深搜+剪枝）
 1）（求解目标）回溯法的求解目标是找出解空间中满足约束条件的一个解或所有解。
 2）（搜索方式深度优先）回溯法会搜索整个解空间，当不满条件时，丢弃，继续搜索下一个儿子结点，如果所有儿子结点都不满足，向上回溯到它的父节点。

分支限界法（广搜+剪枝）
 1）（求解目标）分支限界法的目标一般是在满足约束条件的解中找出在某种意义下的最优解，也有找出满足约束条件的一个解。
 2）（搜索方式）分支限界法以广度优先或以最小损耗优先的方式搜索解空间。
 3）常见的两种分支界限法
 a.队列式（FIFO）分支界限法（广度优先）：按照队列先进先出原则选取下一个结点为扩展结点
 b.优先队列式分支限界法（最小损耗优先）：按照优先队列规定的优先级选取优先级最高的结点成为当前扩展结点



前面的集中搜索策略虽然可用于求解许多问题，但难以用于高效地求解优化问题。而分支限界搜索法，是**求解优化问题的最有效的搜索策略**。

用于求解最小化问题（最大化需要转为最小化问题），各种组合优化问题（如人员分配问题，旅行商问题）

> 1. 爬山法得到问题的一个可行解
> 2. 继续利用爬山法搜索，搜索时剪除不能取得优化解的分支

**分支界限搜索**的基本思想是**用某种策略**(比如爬山法、Best-First策略)产生分支，**发现优化解的一个界限**，缩小解空间，从而提高求解的效率。说白了就是**剪枝**呗！



### 多段图问题

![image-20200812202758521](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-00-6fbbb54b08caf12afecfdf649f0c6de9-image-20200812202758521-fb5446.png)

构建解空间：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-03-f43e191506c6516772d383f397cc1984-v2-10c51cb1a7fde72f0e028673bfa1ac6b_720w-26159a.jpg)

使用爬山法找出cost=5的一个可行解作为上界：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-05-4faa651f2c485e7d8dc10e3c166f0119-v2-0c4f5b71746ad5edb109e6508b18560a_720w-8b196e.jpg)



### 人员分配问题

问题描述：分配问题要求将n个任务分配给n给人，每个人完成任务的代价不同，要求分配的结果最优

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-08-413f29eadc822eb5cca4c5cda843224a-v2-02dd35a1d6c11b2a87c92e3c456a8bf7_720w-f0fec8.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-11-03794ba9d2924e67f323ce3ca06383bc-v2-03794ba9d2924e67f323ce3ca06383bc_720w-4785b4.jpg" alt="img" style="zoom:67%;" />

该问题的解空间是所有拓扑排序的序列集合，每个序列对于一个可能的解。将问题的解用树表示如下：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-14-b50b3bb8ade0ea3159a6fff1dc9b2e33-v2-8e9898fb291156e3fe02fd774e9bc9cb_720w-993def.jpg" alt="img" style="zoom:67%;" />

接下来需要计算解的代价的下界：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-17-e1f6b5ce15131a25b245574307ef4b00-v2-4977e22c8e122ef6e2fcad5b601702c0_720w-131d63.jpg" alt="img" style="zoom:67%;" />

例如：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-20-33d647efd0de24aec0b6d3ab208f9969-v2-6c37167c8a6779040c8fb0b5a3f7862f_720w-e342c0.jpg" alt="img" style="zoom:67%;" />

由此得到解的加权树表示如下：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-23-951e70b4a697cf17fa75c807906bfb09-v2-f5b85313c43da8c233646246ba22e1cc_720w-180c84.jpg" alt="img" style="zoom:67%;" />

使用爬山法策略进行分支界限搜索算法如下：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-26-f94a80aad0cd05a42e223b093152d145-v2-c92193698bdf3a38be281d12f4db4ba2_720w-e7b74b.jpg)

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-30-a637c4762d82aca1560ab10175ef322d-v2-7d647d0175cc524c2e68b2efd6659b0a_720w-f1c709.jpg" alt="img" style="zoom:67%;" />

### 旅行商问题

问题描述：旅行商从 a 开始周游下图所有的城市一次，然后回到 a，城市之间的旅行代价在图中标明。请选择一个最优的行走顺序使得周游所有城市的代价最小

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-33-e2f66698896564d0c139fe72ff95d96b-884670-20161119123158685-2113704031-26bc88.png)

算法步骤：

随便怎么周游，对于一个城市来说，一定有一条进的路和一条出的路。

对于每个城市来说，**暂时都选取代价最小的两条路来作为理想的路线，就算这些路不合理**。

比如对于 a 来说，选择 a<->c(1) & a<->b(3) ；对于 e 来说，选择 e<->c(2) & e<->d(3)。

把所有的这些值加起来除以2，

本题即 lb=[(a<->c+a<->b)+(b<->a+b<->c)+(c<->a+c<->e)+(d<->e+d<->c)+(e<->c+e<->d)]/2=[(1+3)+(3+6)+(1+2)+(3+4)+(2+3)]/2=14 .

把这个值当成是理想的最小代价，然后接下来搜索解空间树的时候，都在该基础上进行。

下面画出搜索解空间树的过程，其中方框上面的是点的名称，下面是假设的理想周游代价，方框头顶是搜索顺序：

刚开始从 a 走![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-37-39bcc410182bb137b6dfd2e377f6771a-884670-20161119124802295-483262793-de7abe.png)

从 a 可以到达 b、c、d、e，

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-40-b0ca26653728e4d3bc5e79dda573c6d4-884670-20161119125725935-1141914772-adc607.png)

这里有一个小细节，就是图中的 2 节点。想想如果周游路线 a<->b<->d<->e<->a 和 a<->e<->d<-><->c<->b<->a ，这两条路线其实是一样的，但是如果不加处理的话，可能两条路线会在搜索的时候都被搜索过，这样浪费了时间。因此，我们这里做个小约定，约定 b 要在 c 之前出现。因此，图中节点 2 就被抛弃了。

继续上面的，从 a 走到那些点后，怎么计算理想代价呢，也就是说怎么计算 lb 呢。

我们用 a 到 d 来举例子吧。

最开始 lb 是选取每个点的代价最小的两条路， lb=[(a<->c+a<->b)+(b<->a+b<->c)+(c<->a+c<->e)+(d<->e+d<->c)+(e<->c+e<->d)]/2。a 走的是c 和 b 这两个，d 走的是e 和 c 。

现在我们选择 a<->d ，那 d 的一条路要被改成 a 了。我们选择将原来的 a<->b 改成 a<->d，因为要使代价最小，所以选择代价大的来替换。那么 d<->c 就被替换成 d<->a了。这时再计算就可以得到新的 lb 了。　　

我们应该选择 lb 最小的往下搜索，较大的等下再搜索。于是

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-44-b46e9cb408ba67fd3d9fa23a76a317e7-884670-20161119131917373-380912253-1ef934.png)

继续向下：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-47-c17300b2b92577f7c96398bab6e7b512-884670-20161119132434232-1366082634-055f1e.png" alt="img" style="zoom:80%;" />

这时已经找出两个周游路程了（因为最后肯定要回到 a 就没向下画了），我们继续搜索，看看有没再好点的解。向上退一层，就是 节点 6 了，所以

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-51-953661a7f6d35f0210b364c0034c3d6d-884670-20161119132912279-2013407485-654255.png" alt="img" style="zoom:80%;" />

继续搜索，发现节点 7 才走到 e 就要 19，而 节点 11 走完了只需要 16，所以把它抛弃，继续退回，发现 3，4 都不行

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-53-79d9385e28b59257f2fa21975d85575f-884670-20161119133429232-401883521-b618fa.png" alt="img" style="zoom:80%;" />

### 0-1背包问题

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-48-59-305814dd92ce27af5d0f64382798ba0c-v2-d6869a2dddfaf9b546c33d5182b363ad_720w-c844fa.jpg)

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-02-42d1984ce4066d99140d4d4be0ec9792-v2-6e4528b280ee307194e512a0ad6351ff_720w-14f9d3.jpg)

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-06-cc8f95019af26a10917cb5997c94979e-v2-50d5a1c3a3489191eb3cfa6c233a21b1_720w-e7cee1.jpg)

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-09-b3beab7e394d2a82d4b1b2f0c53946a2-v2-1dbcf758357f5404312b9ba38302143a_720w-8977e3.jpg)

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-12-555574418fb661f328386fee673fe0b7-v2-037ea06d0e6175e4fa5580da7ac9446b_720w-d4ce0b.jpg)

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-15-9295436f2699e024d1b72dcb7904c2c4-v2-6bbf70d3350361455ab8f8b384126729_720w-32b86a.jpg)

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-19-ed5ab3fbcc7cd93f322ec4a35e42ca2a-v2-fcb6f8893d9f8e349cff36aaf1541217_720w-afc0f0.jpg)

### A*算法

**A\*算法**是利用**启发式函数**、**Best-first策略**进行搜索从而**更早的发现优化解**的一种算法，它的核心告诉我们在某些情况下得到的解一定是最优解，算法可以停止！与**分支界限**不同的是：分支界限是为了剪掉不能达到优化解的分支。

A*算法的关键是启发式函数的选取！对于任意一个节点n ，我们有：

- g(n)= 从树根到n的代价
- h*(n) = 从n到目标节点的优化路径的代价
- f*(n)=g(n)+h\*(n) 是节点n的代价

上述式子中g(n)好理解，可是h*(n)怎么计算呢？一般的，可以使用某种方法来估计 ![[公式]](https://www.zhihu.com/equation?tex=h%5E%2A%28n%29) ，用 h (n)表示 ![[公式]](https://www.zhihu.com/equation?tex=h%5E%2A%28n%29) 的估计，**使得h(n)≤h\*(n) 总为真**，通常把 ![[公式]](https://www.zhihu.com/equation?tex=h%28n%29) 叫做**启发式函数**。

算法步骤如下：

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-22-652209c098e959adfb770bfa64132f44-v2-f790c445be6e62ad222673cc969108a1_720w-8c6a10.jpg)

例子：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-25-b2a79af82166d87c13e4a7255df8536f-v2-b2a79af82166d87c13e4a7255df8536f_720w-55b2c1.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-28-3f9b0145cd3152b2ccf7f01b0a75fb49-v2-90771dd74a38d2231ec9125e845b5433_720w-afbc04.jpg" alt="img" style="zoom:67%;" />

接下来拓展到V1

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-41-37362df9a5fac88a4c5b1632c794b894-v2-b95b6278916bcf7ad780e7aa5406d357_720w-40cad7.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-45-34e071d41c088de00804ba4b2e374576-v2-220297b87750a30d99f7212fdb8c6051_720w-6f492f.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-49-53-3ab6006a81638c2e7334c66e0d4675ea-v2-a9758b5558c5655162d34ae42bfc41a3_720w-70cc96.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-00-98896936d81104c3c2e67e5f5b4fb676-v2-875da1654fbb72d9771300f4c40d7327_720w-87a368.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-03-7fe37a828c7eaff62471f0559b9e758f-v2-e25140b891c0d5842fdb19970c98a88f_720w-43660d.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-06-7fe37a828c7eaff62471f0559b9e758f-v2-e25140b891c0d5842fdb19970c98a88f_720w-536779.jpg" alt="img" style="zoom:67%;" />

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-09-fc51f27a18dd89333f7e7b4fc0418aae-v2-8d143f765c442b440bcbf3a32d061720_720w-174f59.jpg" alt="img" style="zoom:67%;" />

# 迭代改进

* 另一种最优问题的解决方案 
* 从某些可行解开始，通过重复使用一些简单的步骤来不断改进它，最终使目标函数最优。
* 需要一个初始可行解;
* 判断可行解改变后是否最优;
* 如何改进是局部解或全局解最优。

## 单纯形法

线性规划的几何解释：

![image-20200813101410555](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-12-8965cb9c7bf3c96962c3195dbf0cd9a4-image-20200813101410555-4bc371.png)

* 极点定理：极点定理可行区域非空的任意线性规划问题有最优解，而且，最优解总是能够在其可行区域的一个极点上找到。
* 单纯形法：只需检测可行区域极点中的一小部分就能找到最优点。现在可行区域中找一个极点，然后检查是否能在邻接极点处取得更佳，如果不是，当前极点就是最优值;如果是，继续往下处理。

单纯形法应用于线性规划问题的标准形式：

* 必须是一个最大化问题
* 所有约束必须用线性方程形式表示
* 所有变量都要求是非负的

因此，单纯形法解决线性规划的通用形式是：

![image-20200813102206339](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-15-2a39f9064b7181e38de2526a11e5c468-image-20200813102206339-6e4384.png)

举例：

![image-20200813102306537](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-18-1b1ce3cc56b5043424f7936370ea30d6-image-20200813102306537-292006.png)

变量u和v将不等式约束转化为等式约束，称为松弛变量。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-21-2628447c3bc3908fdd00f85081961eb4-image-20200813102409652-7719cc.png" alt="image-20200813102409652" style="zoom:67%;" />

### 单纯形表法步骤

垃圾单纯形表，弄了好久才懂，下面举一个例子详细说明单纯形表法的步骤：

![image-20200813144011924](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-24-b4485ea399ab9443bb38a14a0cb3ff40-image-20200813144011924-884792.png)

由于必须满足线性规划的标准型，所以把上面的式子转为下面的式子：

![image-20200813144055387](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-26-c487ac585fc247702607f786839eef9a-image-20200813144055387-7be296.png)

可以总结标准型的三个基本要素：

![image-20200813144122688](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-29-9a486d4342272ee56f003fccab388388-image-20200813144122688-de02bc.png)

三个基本要素：**1. 一个明确的目标函数；2. 等式约束; 3. 决策变量非负。**



对于标准型我们有两个基本假设：
 **1. 系数矩阵A的行向量线性无关。**
 **2. 系数矩阵A的列数大于其行数，即n>m。因为如果n<m，那么不满足1, 如果n=m，那么该线性规划问题有唯一解，既然有唯一解，那就没有优化的必要了。所以，必有n>m。**

上述式子可以写成：

![image-20200813144219785](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-33-7062f39ba5191a412c6692161da48d3f-image-20200813144219785-073284.png)

其中，x3，x4，x5在三个等式中只出现一次，且系数为1，所以称为**基变量**，其余的x1，x2为**非基变量**。可以将其构建成一个单纯形表：

![image-20200813144818641](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-36-15cf7239cb096b9eac251ec6a07ffd2c-image-20200813144818641-c63d35.png)

其中，第一列代表当前的基变量，每一行代表一个约束方程，数字代表对应变量的系数，最后一行表示当前的目标函数，最后一列表示对应的常数项。

方法的**目标是使最后一行的系数项全变为负数**，此时对应最优解。上图为例，x1和x2的系数都为正，因此需要变基（选一个非基变量变为基向量，对应的有一个基向量变为非基向量）怎么选？

首先，**选系数最大的为进基变量**，如上图是x1。

退基变量选RHS/（进基变量的系数）最小且非0的，如上图，15/0=无穷，24/6=4,5/1=5，所以选x4是退基变量。

变基的过程就是要把x4对应的那行中x1对应的系数变为1，其余行的x1系数变为0，同时更新目标函数（使其非0的系数仅为非基变量），第一次变基单纯形表如下所示：

![image-20200813160650375](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-39-78d3c81d8bef2563cd323b3d7857f7d8-image-20200813160650375-03e60f.png)

可以看到，目标函数中，x2的系数还是正数，因此接下来x2进基，15/5=3,4/(1/3)=12,1/(2/3)=3/2，因此x5退基。

第二次变基后单纯形表如下：

![image-20200813160752395](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-42-a599827015b29a2d891802c6866e548f-image-20200813160752395-7d3ad4.png)

求得z的最优解为8.5

## 最大流问题

![image-20200815160918805](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-46-8328c572829289cbb6f5f50b00b7447d-image-20200815160918805-b83eab.png)

边上的数字为该条边的容量，即在该条边上流过的量的上限值。最大流问题就是在满足容量限制条件下，使从起点s到终点t的流量达到最大。

### Ford-Fulkerson方法

根据图和各条边上的流可以画出一幅图的残存网络如下所示。**左图为流网络**，**右图为残存网络**，其中流网络中边上的数字分别是流量和容量，如10/12，那么10为边上的流量，12为边的容量。残存网络中可能会存在一对相反方向的边，**与流网络中相同的边代表的是流网络中该边的剩余容量**，在流网络中不存在的边代表的则是其在流网络中反向边的已有流量，这部分流量可以通过“回流”减少。例如，右图残存网络中，边<s,v1>的剩余容量为4，其反向边<v1.s>的值为12，即左图流网络中的边<s,v1>的流量。在残存网络中，值为0的边不会画出，如边<v1,v2>。

![image-20200815161143974](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-51-5a93b51bdd555c200d39ac5e8dad1a14-image-20200815161143974-2dfa74.png)![image-20200815161150108](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-50-54-9597992f863d33b7e9b84144e8cb951c-image-20200815161150108-05f41f.png)

残存网络描述了图中各边的剩余容量以及可以通过“回流”删除的流量大小。在Ford-Fulkerson方法中，正是**通过在残存网络中寻找一条从s到t的增广路径，并对应这条路径上的各边对流网络中的各边的流进行修改。**如果路径上的一条边存在于流网络中，那么对该边的流增加，否则对其反向边的流减少。增加或减少的值是确定的，就是该增广路径上值最小的边。

算法伪代码：

其中<u,v>代表顶点u到顶点v的一条边，<u,v>.f表示该边的流量，c是边容量矩阵，**c(i,j)表示边<i,j>的容量**，当边<i,j>不存在时，c(i,j)=0。**e为残存网络矩阵**，e(i,j)表示边<i,j>的值，当边<i,j>不存在时，e(i,j)=0。E表示边的集合。f表示流网络。

```
Ford-Fulkerson
 
    for <u,v> ∈ E
 
        <u,v>.f = 0
 
    while find a route from s to t in e
 
        m = min(<u,v>.f, <u,v>  ∈ route)
 
        for <u,v> ∈ route
 
            if <u,v>  ∈ f
 
                <u,v>.f = <u,v>.f + m
 
            else
 
                <v,u>.f = <v,u>.f - m
```

Ford-Fulkerson方法首先对图中的所有边的流量初始化为零值，然后开始进入循环：如果在残存网络中可以找到一条从s到t的增广路径，那么要找到这条这条路径上值最小的边，然后根据该值来更新流网络。

Ford-Fulkerson有很多种实现，主要不同点在于如何寻找增广路径。最开始提出该方法的Ford和Fulkerson同学在其论文中都是使用广度优先搜索实现的，其时间复杂度为O(VE)，整个算法的时间复杂度为O(VE^2)。

算法对于增广路径的选择可能会影响算法的性能，如下图所示的例子
<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-01-b4e8fcb738da727d07860de9650ad1a5-image-20200815162703060-b3476e.png" alt="image-20200815162703060" style="zoom:50%;" />

假设U是一个很大的正数，算法要执行2U次循环才找到最大流。

### Edmonds-Karp 算法（Shortest-Augmenting-Path Algorithm）

Shortest-Augmenting-Path Algorithm采用BFS的方法来找增广路径，伪代码如下：

![image-20200815164939295](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-04-56357e314d2b05aa8325cca3ab46be09-image-20200815164939295-6e834b.png)

![image-20200815164713377](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-07-b6a64ff14a21091a430bec9561600f3c-image-20200815164713377-5923ad.png)

算法对每一个顶点进行标记，如果

* 下一个顶点没被标记，且当前流是正向流，j被标记为min{li, rij}，li是i顶点的标记值，rij是当前未使用的正向流容量。
* 下一个顶点没被标记，且当前流是反向流，j被标记为min{li,xij},li是i顶点的标记值,xij是当前正向流的流量。

算法示例：

![image-20200815170221654](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-12-0d481dac39b876615df38c59b0fe70da-image-20200815170221654-ba7802.png)

![image-20200815171405049](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-16-1e93e9fc3f5f31443b8d6cc98556b138-image-20200815171405049-4d95ef.png)

ps：就是采用BFS遍历而已，并检查下一个顶点与当前顶点存在的边是正向边还是反向边，并给顶点做标记，访问到流终点后根据标记倒推回流起点，根据标记记录的值和正负号决定是要给增加正向流的流量还是反向流的容量。

算法复杂度：

增广路径的数目不会超过nm/2，n是顶点数，m是边数。对于邻接表表示的图，寻找增广路径的时间复杂度为O(n+m)=O(m),因此总的时间效率为O(nm^2)

### 最大流最小割定理

割：减掉一些边，使得起点O到达不了终点T，O能到达的顶点位与O一起组成一个顶点集合，余下的顶点凑成另一个集合。割的大小为减掉的边的大小总和。

最大流最小割定理提供了对于一个网络流，**从源点到目标点的最大的流量等于最小割的每一条边的和**。即对于一个如果移除其中任何一边就会断开源点和目标点的边的集合的边的容量的总和。

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-20-268f356e67e2a8b212d1a849ada2215f-image-20200815183825756-a0866a.png" alt="image-20200815183825756" style="zoom:67%;" />

如上图，最小割是当顶点集只包括1的时候，所包括的边，即1-2和1-3，和为5，也就是图的最大流的值。Shortest-Augmenting-Path Algorithm算法的最后一次迭代，标记点和未标记点可以得出最小割。算法最后得出最大流的值。

## 二分图

二分图：顶点可分为两个不相交的集合V和U，大小不一定相同，V中的顶点边都连接到U中顶点。图是二分图当且仅当它没有奇数长度的圈。

**匹配**：在图论中，一个「匹配」（matching）是一个**边的集合**，其中**任意两条边都没有公共顶点**。例如，图 3、图 4 中红色的边就是图 2 的匹配。设一个匹配为M，M中未包含的顶点称为自由顶点，已包含的顶点叫匹配点

![image-20200815185649787](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-24-9910e82def6103e259f082684ccb29f1-image-20200815185649787-442a10.png)

**最大匹配**：一个图所有匹配中，所含匹配边数最多的匹配，称为这个图的最大匹配。图 4 是一个最大匹配，它包含 4 条匹配边。

**交替路**：从一个未匹配点出发，依次经过非匹配边、匹配边、非匹配边…形成的路径叫交替路。

**增广路**：从**一个未匹配点出发**，走交替路，如果**途径另一个未匹配点**（出发的点不算），则这条交替路称为增广路（agumenting path）。例如，图 5 中的一条增广路如图 6 所示（图中的匹配点均用红色标出）：

M是最大匹配当且仅当对M不存在增广路径。

![image-20200815190501821](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-28-7fabdce34c097b7b1b31292b49e61196-image-20200815190501821-f4d331.png)![image-20200815190508816](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082616-55-17-08e64080681f71b28e22d2f37566176d-image-20200815190508816-658dc2.png)

通过增广路求最大匹配：

* 从初始匹配开始
* 找到一条增广路径并沿着该路径增加当前匹配（可以用BFS）
* 当找不到增广路径时，终止并返回最后一个匹配，就是最大匹配

### 基于BFS的增广路径算法

算法伪代码：

![image-20200815201854352](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-32-0d8d82919833f79cd03bfc171342309b-image-20200815201854352-d42d5f.png)

算法步骤：

* 使用其中一个集合中的所有自由顶点初始化队列Q（比如V）

* 当Q不为空时，删除队列第一个顶点w，并为每个与w相邻的未标记顶点u添加标记，按如下情况：

* 情况1（w在V中）遍历每个与w相邻的顶点u，如果u是自由顶点，则把边<w,v>添加到M中，并沿u的标记往后遍历，交替地向M删除和添加对应标记的边，最后删除所有的标记，重新初始化队列Q为V中的自由顶点；

  如果u不是自由顶点，若<w,u>未包含在M中，且u未标记，则用w标记u（表示前一个顶点），并且把u进入队列

* 情况2（w在U中，已经包含在M中）标记M中w所在边的另一个端点v（用w标记），并且把v进入队列

* 当Q为空时，找到了最大匹配M

算法示例：

![image-20200815204656647](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-38-7af5dea8419c818e7c69042a7875b9b1-image-20200815204656647-7455f0.png)

每个顶点都用它到达的顶点来标记。出队列的元素由箭头指示。为了清晰起见，在U中找到的自由顶点被着色并标记；

![image-20200815204817507](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-41-e9d9e247211e44d05db679b9f3e6c578-image-20200815204817507-749bde.png)

![image-20200815204833718](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-44-1bb2a7e3b0b1d7af38156666059540e8-image-20200815204833718-7ce123.png)

![image-20200815205558247](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-48-9bb8c4784438a7010fa030192679e1c2-image-20200815205558247-f8b206.png)

复杂度分析：每次迭代（最后一次除外）匹配两个自由顶点（V和U各一个）。因此，迭代次数不会超过ceil{n/2}+1，其中n是图的顶点数。每次迭代所花费的时间复杂度为O（n+m），其中m是图中的边数。因此，总时间复杂度为O（n（n+m））

## Gale-Shapley算法-稳定婚姻

问题描述：有一个集合Y={m1，…，mn}由n个男人组成，X={w1，…，wn}由n个女人组成。每个男人对每个女人都有一个喜好程度表，同样每个女人也有。当一个男人与一个女人匹配时，用(mi，wj)表示。当存在一对匹配(m,w)属于匹配集合M中，但是m和w各自都有更心仪的对象还没有属于M中，则成M是不稳定的，否则称为稳定的，找到一个稳定的匹配集合M。

算法步骤：

1. 从所有男人和女人都未匹配开始
2. 随意挑选其中一个未匹配（单身）的男人，并完成以下步骤：
   * 求婚：被选中的单身男人m按照他的偏好列表向一个女人w求婚，
   * 答复：如果w是单身的，她接受m的求婚。如果她不是单身，她会将m与她当前的伴侣进行比较。如果她更喜欢m，她会接受m的建议，让她的前伴侣恢复单身；否则，她拒绝m的求婚，m依然是单身

3. 返回n个匹配对的集合

算法示例：

<img src="https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-52-3b697082740aab8ca6f76f5281b8ddb6-image-20200815211330133-62e44a.png" alt="image-20200815211330133"  />

![image-20200815211450578](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-56-88ddbd71a504fe9fc98ddd2363798973-image-20200815211450578-b4f1cc.png)

![image-20200815211458417](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-51-59-6257ee58b3e21da5996a7ffbe051443a-image-20200815211458417-c95ad4.png)

算法分析：

* 该算法在不超过n^2次迭代后终止，并输出稳定的婚姻匹配
* 该算法产生的稳定匹配始终是男性优先的：在任何稳定的婚姻关系下，每个男性都会得到其列表中排名最高的女性。反之通过让女人向男人求婚，可以获得女人优先的最佳匹配

## 棒球赛淘汰问题

问题描述：给定比赛情况如下表所示

![image-20200815221832193](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-52-02-e0affe329e57e6b8a34448951c5b9e95-image-20200815221832193-22a3c6.png)

求Harvard队是否有夺冠可能

问题分析：

* Harvard队最多能赢W = 29 + 4 = 33场
* 假设Harvard队剩下的比赛全赢，当其余队的胜场如下情况时，Harvard队可能夺冠：
  *  Brown 队接下来最多赢u(B) = W-w(B) = 33-27 = 6场
  *  Cornell队接下来最多赢u(C) = W-w(C) = 33-28 = 5场
  *  Yale队接下来最多赢 u(Y) = W-w(Y) = 33-33 = 0场

* 令P为除了Harvard队的其余队的集合P = {Y, C, B}
* 令Q为所有P集合中可能的组合Q = { (Y,C), (Y,B), (C,B) }
* Q中所有组合的比赛场数为G = 6+1+1 = 8；

使用最大流来求解棒球赛淘汰问题，构建如下最大流图：

![image-20200815222533535](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-52-06-12646e64f77f33db04baa0c214d092aa-image-20200815222533535-796662.png)

O为起点，T为终点，O和Q集合中的组合队相连，边的权重为对应的比赛场次，Q中的结点又与对应的队伍相连，权重为各队伍余下的比赛场次，各队伍与T相连，权重为最多能赢的场次。

* 寻找O到T的最大流
* 如果最大流的值=G（余下队伍的所有比赛场次），则Harvard队有机会夺冠

当前例子中，最大流为7<8，因此Harvard队无机会夺冠：

![image-20200815222957198](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-52-09-097189b14cc92414195f29d2000b8b16-image-20200815222957198-818119.png)

使用最小割来分析Harvard的夺冠问题：

![image-20200815223051229](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-52-12-ccdb762ba2ca08a39a36ed0ef7e89baa-image-20200815223051229-55c983.png)

* 包含起点的最小割为 {O, (Y,C), Y, C}
* 最小割包含Y队和C队，Y队和C队的剩余比赛场次为6，而Y队和C队接下来只需赢0+5=5场，Harvard队就没有夺冠可能，因此Harvard淘汰

从另一个角度分析Harvard队的夺冠问题：

* 最小割包含的Y队和C队总共的胜利场次为： (33 + 28) + 6 = 67

  平均每队的胜利场次为：67 / 2 = 33.5，总会有一个队的胜利场次>34，而Harvard队最多胜利33场，所以Harvard队没有希望夺冠

* 通常，设有0,1...，n队，如果队伍集合R={1,…,n}有

![image-20200815223627020](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082614-52-16-d2b15df7e37bad88730d0f6d18729551-image-20200815223627020-8f1e1c.png)

g(R)是R集合剩余的比赛场次总数，w(i)是队伍i的胜利场次，则Harvard队没有夺冠可能