# SVM

## 基础

- SVM 用来做什么？
  - 解决 **二分类** 问题
    - 二分类：把数据分为两类
    - 前提：数据集 **线性可分**
      - 线性可分：可以用一条直线（二维），一个平面（三维），一个超平面（多维）将数据分为两类
        - 典型的非线性可分数据集就是 **异或**
        - 你会发现 $(0,0)\rightarrow0$，$(0,1)\rightarrow1$，$(1,0)\rightarrow1$，$(1,1)\rightarrow0$ 这四个样本永远无法用一条直线分类
      - 如果不能怎么办？
        - 可以使用 **核函数**，我们会在下面谈到
- SVM 怎么把数据分为两类？
  - 为了理解简单，我们可以设想数据是二维的情况
  - 我们假设两类数据各自抱成一团，没有重叠，那么你很容易就能够画出很多条直线把它们分隔开
  - 但是我们的结果只能是一条直线，此时我们就要问一个问题：那条直线是最好的呢？
  - SVM 认为：**最大化间隔** 是最好的
    - 间隔：想象这条直线的左右两侧通过两条平行线划定了两个“无人区”，这个区域里不允许存在数据点，这个区域就叫做间隔
    - 最大化：当间隔被最大化的时候，很容易理解，说明两类数据被最大程度地分开了
  - 现在，我们的任务变成了：根据数据集求得最大化间隔的直线
    - 当然，我们不能一直叫它“直线”，因为对于超过二维的数据，这样的分界是平面或者超平面
    - 所以，抛开数据的维度，我们给它们一个统一的名字：**线性分类器**
- SVM 怎么求线性分类器？
  - 假设现在有数据集 $(x_1,y_1),...(x_n,y_n)$
    - $x_i$ 为样本特征，是一个 $n$ 维列向量
    - $y_i$ 为样本标签，是 $-1|+1$（因为我们研究的是二分类问题）
  - 超平面
    - 一个超平面可以用 $x^TW+b=0$ 表示
      - $W$ 是一个列向量，称为法向量
      - $b$ 是一个数字，称为截距
      - $x$ 以供将样本特征 $x_i$ 代入
    - 和二维平面的直线方程一样，$x^TW+b>0$ 就代表 $x^TW+b=0$ 这个超平面的“上方”（严格来说是法向量所指方向）
      - 那么，我们就可以把两类数据点按照上文所述，分别归到两条“平行线”的上方和下方
      - 因为它们必须与 $x^TW+b=0$ 平行，于是我们选择 $x^TW+b=1$ 和 $x^TW+b=-1$
        - 这里的 $1$ 和 $-1$ 其实并没有特殊意义，可以任意选择数字
      - 所以现在有
        - $x^TW+b>1$ 则 $y=+1$
        - $x^TW+b<-1$ 则 $y=-1$
  - 距离最大化
    - 这两条平行线之间的距离是 $\frac{2}{||W||}$（别问为什么，类比二维就好）
    - 我们要最大化间隔，就是要使 $\frac{2}{||W||}$ 最大，等价于使 $\frac{1}{2}||W||^2$ 最小（二分之一是为了方便后面求导而添加的）
    - 现在我们的任务变成：求 $W$ 和 $b$ 使得 $\frac{1}{2}||W||^2$ 的值最小，当然还有约束条件
      - 对所有 $x_1...x_n$ 有：$x^TW+b>1$ 时 $y=+1$；$x^TW+b<-1$ 时 $y=-1$
      - 简化即：$y_i(x_i^TW+b)\geq1$
  - 数学计算
    - 看不懂
- 为什么叫支持向量机？
  - 因为线性分类器的确定只和 **支持向量** 有关
    - 支持向量：恰好处于上文所述直线 $x^TW+b=1/-1$ 上的数据点
    - 很容易看出，支持向量就是各类数据中离线性分类器最近的数据点
    - 当数据集给定时，支持向量其实也已经隐性地确定了，除了支持向量以外的数据，无论如何更改，哪怕删除，都不会对线性分类器的确定造成任何影响
