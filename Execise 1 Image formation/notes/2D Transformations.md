# 2D Transformations

在计算机视觉中，齐次向量和非齐次向量之间的转换可以通过将齐次向量的前n个元素除以第n+1个元素来实现，其中n是向量的维数。这个过程被称为“去齐次化”（dehomogenization）。

![Untitled](2D%20Transformations%2077ae38f4269a4115999e8136b9b52a45/Untitled.png)

x DoF x个自由度

- Translation(平移):2D Translation of the Input,2 DoF 对增广向量进行操作。

$$
x' =x +t \\ \bar x' = \left [\begin{array}{c} I \quad t \\0^T \quad 1 \end{array}\right ] \bar x \\ \left [\begin{array}{c} 1 \quad 0 \quad t_x \\0 \quad 1 \quad t_y \\0 \quad 0 \quad 1 \end{array}\right ] 
$$

这里的自由度是2，显然 存在$t_x,t_y$两个自由度，这个也挺显然的，对于x，y两个维度进行平移

- Euclidean(欧氏距离):2D Translation + 2D Rotation，3 DoF

旋转是一个自由度(这里可以认为是旋转的角度$\theta$ 本质上是绕z轴旋转，因此如果再升一维度，其旋转自由度应该有三个 x，y，z)，还有平移的两个自由度，总共三个自由度。

$$
x' =Rx +t \\ \bar x' = \left [\begin{array}{c} R \quad t \\0^T \quad 1 \end{array}\right ] \bar x \\
$$

旋转矩阵满足$RR^T = I \quad and \quad det(R) = 1$

- Similarity:(2D Translation + Scaled 2D Rotation,4 DoF)

$$
x' =sRx +t \\ \bar x' = \left [\begin{array}{c} R \quad t \\0^T \quad 1 \end{array}\right ] \bar x \\
$$

相比于Euclidean 多了一个缩放的系数  s

- Affine(2D Liear Transformation, 6DoF)

$$
x' =Ax +t \\ \bar x' = \left [\begin{array}{c} A \quad t \\0^T \quad 1 \end{array}\right ] \bar x \\
$$

仿射变换， A中有四个自由度，一个A矩阵，实际上是对其进行变换，之前的直观感受 就是左乘一个矩阵，就是对这个向量进行平移和旋转，但是其实如果这个矩阵一般话，那么他就是对其进行仿射变换。A一般性$A \in \mathbb{R} ^{2 \times 2}$，这个仿射变换保留性质，平行线在仿射变换后仍然平行。

- Perspective (Homography, 8 DoF)

透视变换，将不保留上述平行线变换后仍然平行的性质，存在8个自由度。具体如下：

$$
\tilde x' =\tilde H \tilde x\quad (\bar x = \frac{1}{\tilde w}\tilde x) \\ \tilde H = \left[\begin{array}{c} h_{1,1},h_{1,2},h_{1,3}\\h_{2,1},h_{2,2},h_{2,3}\\h_{3,1},h_{3,2},1\end{array} \right]  
$$

tilde表示在原始图像坐标系下的坐标，而prime表示在变换后的图像坐标系下的坐标

$$
\tilde l'^T\tilde x' = \tilde l'^T \tilde H \tilde x = (\tilde H ^T\tilde l')^T \tilde x = \tilde l ^T \tilde x = 0 \\Hence\quad  \tilde l' = \tilde H^{-T} \tilde l
$$

对比一下 3D的

![Untitled](2D%20Transformations%2077ae38f4269a4115999e8136b9b52a45/Untitled%201.png)

参考资料

[4810 - Homogeneous Vectors and Transformations (virginia.edu)](https://www.cs.virginia.edu/luther/4810/F2021/math2.html)

[特征值和特征向量 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/95836870)