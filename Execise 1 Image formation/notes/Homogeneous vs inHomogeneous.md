# Homogeneous vs inHomogeneous

齐次向量和非齐次向量这方面知识是第一次接触，对其进行一些解释。

非齐次向量→通常表现在一个二维平面的一个点，那么其表示方式为

$$
\mathbf{x} = (\begin{array}{l} x \\y\end{array}) \in \mathbb{R}^2
$$

因为他是在一个平面上，所以他包含的维度为2，这个很好理解。那么非齐次向量的理解可以认为是一组等价的投影点，这些投影点在各个切面中的投影点，如下:

![Untitled](Homogeneous%20vs%20inHomogeneous%2058718e9f7fb946a7bed07f78c5ac7289/Untitled.png)

因此，齐次向量可以看作是在三维空间中，验证观测点发射的一条射线，有无数个点(因为有无数个切面)，他们均等价，特别的，当非齐次向量进行增广后，增广第三维为1时(可以看作在标准距离1的平面投影点)，这时就将非齐次向量转为了齐次向量(可以乘以比例进行缩放)，而对于齐次向量转为非齐次向量只需除以第三维度的大小，取出前两维，则反映其在标准距离1平面的投影点。

下面给出齐次向量的表示

$$
\mathbf{\tilde x} = (\begin{array}{l}\tilde x \\ \tilde y \\ \tilde w\end{array}) \in \mathbb{R}^2
$$

此外，特别的，当$\tilde w$为0时，称为无穷远处的点。当齐次转为非齐次时候，除以第三维后,x和y变为无穷大，意味着在投影面的点为无穷远。

2D lines

特别的在作业中出现一个当初始点距离的考察，这个其实比较简单，对于一条线表示如下

$$
\{\bar x |\tilde l^T \bar x = 0\} \leftrightarrow\{x,y|ax+by+c=0\}
$$

将其a,b进行normalize后的l表示如下

$$
\{\bar x |\tilde l^T \bar x = 0\} \leftrightarrow\{x,y|n_xx+n_yy+d=0\}  \rightarrow \tilde l = (n_x,n_y,d)^T = (\mathbf{n},d)^T
$$

其中d为初始点到2Dline的距离，这个初中知识，求出垂线的交点，然后计算两点距离易得。

对于一条特殊的直线$\tilde l_\infty = (0,0,1)^T$它意味着所有无穷远处的直线，表示所有无穷远点都经过这条直线。可以进行验证，设无穷远点的齐次向量为$x_\infty = (x,y,0)^T$，带入可得

$$
\tilde l_\infty x_\infty = 0
$$

任意x,y无穷远处都在该直线上，证毕。

下面介绍一下cross product

我将从我的代数理解对cross product进行一些分析。

首先设定两个向量$a = (a_1,a_2,a_3), b=(b_1,b_2,b_3)$

参考lec中两向量叉乘后的结果如下

$$
a\times b =[a]_\times b = \left[\begin{array}{c}0 \quad -a_3 \quad a_2\\a_3 \quad 0\quad -a_1 \\ -a_2 \quad a_1 \quad 0 \end{array}\right]\left(\begin{array}{c} b_1\\b_2\\ b_3 \end{array}\right) = \left(\begin{array}{c} a_2b_3-a_3b_2\\a_3b_1-a_1b_3\\a_1b_2-a_2b_1 \end{array}\right)
$$

其中$[a]_x$矩阵表示为一个反对称矩阵，即$[a]_x = -[a]_x^T$，显然对角线要全为0，但是刚开始不太理解这个是怎么得来的，我以自己的角度理解一下。

其实cross product也可以写为如下形式：

$$
a\times b = \left[\begin{array}{c} i \quad j \quad k \\ a_1 \quad a_2 \quad a_3 \\ b_1 \quad b_2 \quad b_3 \end{array}\right] = \left[\begin{array}{c}  a_2 \quad a_3 \\  b_2 \quad b_3 \end{array}\right]i - \left[\begin{array}{c} a_1 \quad  a_3 \\ b_1 \quad  b_3 \end{array}\right]j+\left[\begin{array}{c}  a_1 \quad a_2  \\ b_1 \quad b_2  \end{array}\right]k
$$

其对应的i，j，k的值即对应向量的各个值。如果对于是矩阵和向量的的乘积，举个例子，对于$b_1$可以看作是和

$$
\left[\begin{array}{c} 1 \quad 1 \quad 1 \\ a_1 \quad a_2 \quad a_3 \\ b_1 \quad b_2 \quad b_3 \end{array}\right] \rightarrow^{b_{1}}\left[\begin{array}{c}  1 \quad 1 \\  a_2 \quad a_3 \\ \end{array}\right]\rightarrow b_1(a_3 - a_2)
$$

对应于矩阵和向量相乘，$b_1$只会和反对称矩阵中的第一列相乘然后求和，因此分配系数$0，a_3,-a_2$其他类似，可以有意构造这样一个反对称结构。

对两条线进行cross product的结果对应于两条线的交点，可以很快发现，若两条直线平行，那么他们的$a_1b_2 - a_2b_1$一定是为0的，所以交点在无穷远处，这个很好理解，此外如果需要验证cross product后的结果是两条直线的交点，那么只需要验证将交点带入至两条直线(作内积即可)。

对两个点进行cross prodcuct的结果为这条直线，这个也好理解，将两个点进行cross product，然后将这两个点再与求出直线进行内积，内积为0则说明两点在这条直线上，即可验证成功。这里需要说明的是，对于两个无穷远处的点，我们将其进行cross product 其结果表示无穷远线，因为$a_3 = 0, b_3 =0$则求出直线中的前两项一定为0，满足无穷远处的直线表示形式$\tilde l_\infty = (0,0,1)^T$,最后一项可进行归一化获得。