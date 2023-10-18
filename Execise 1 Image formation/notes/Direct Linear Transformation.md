# Direct Linear Transformation

这里是对DLT的一些理解，顺便结合代码进行分析。

这里是一个CMU CS关于DLT的详细例子解释帮助理解:[10.2 2D Alignment - DLT (cmu.edu)](https://www.cs.cmu.edu/~16385/s17/Slides/10.2_2D_Alignment__DLT.pdf)

参考例子设法，设一对对应点如下$\tilde x = (x,y,w)^T,\tilde x^\prime=(x^\prime,y^\prime,w^\prime)^T$

设H矩阵如下:

$$
h = \left[\begin{array}{c} h_1 &h_2&h_3\\h_4 &h_5 & h_6 \\ h_7 &h_8&h_9 \end{array} \right]
$$

我们令

$$
\tilde h_1^T = [h_1,h_2,h_3]\\\tilde h_2^T = [h_4,h_5,h_6]\\\tilde h_3^T = [h_7,h_8,h_9]
$$

满足如下等式

$$
\tilde x^\prime = h\tilde x
$$

可以写出三个等式如下

$$
x^\prime = h_1x+h_2y+h_3w \quad 1\\y^\prime = h_4x+h_5y+h_6w \quad 2\\w^\prime=h_7x+h_8y+h_9w \quad 3
$$

可以任意用两个等式联立生成一个等式。例如

2，3联立

$$
y^\prime w^\prime = h_4xw^\prime+h_5yw^\prime+h_6ww^\prime \\y^\prime w^\prime=h_7y^\prime x+h_8y^\prime y+h_9y^\prime w \\ \rightarrow -h_4xw^\prime-h_5yw^\prime-h_6ww^\prime +h_7y^\prime x+h_8y^\prime y+h_9y^\prime w = 0 \\ \rightarrow [0,0,0,-w^\prime x,-w^\prime y,-w^\prime w,y^\prime x,y^\prime y, y^\prime w] \left[ \begin{array}{l} h_1\\ h_2\\h_3\\h_4\\h_5\\h_6\\h_7\\h_8\\h_9 \end{array}\right] \\ \rightarrow [\mathbf{0^T,-w^\prime \tilde x^T, y^\prime \tilde x^T} ]\left[ \begin{array}{l}\mathbf{ \tilde h_1} \\\mathbf{ \tilde h_2 }\\ \mathbf{\tilde h_3} \end{array}\right]
$$

这里就获得了DLT中$A_i$矩阵的第一行表达式，因此，两两组合就可以得到lec中的$A_i$ (1,3获得第二行，1，2获得第三行)

最终结果如下:

![Untitled](Direct%20Linear%20Transformation%20659f9350528b482aa47995edd835710d/Untitled.png)

对于一组对应点，实际上有效的等式只有2组，例如1，2，因为秩是2，任意一行可以通过剩余两行进行线性组合获得。因此对于N组的对应点，我们只需要构造一个$2N\times9$的矩阵即可。

代码如下，其实比较好理解，能够推导出上述公示后，然后取前两个等式进行组合即可。

```jsx
###########################
#### Exercise Function ####
###########################
def get_Ai(xi_vector, xi_prime_vector):
    ''' Returns the A_i matrix discussed in the lecture for input vectors.
    
    Args:
        xi_vector (array): the x_i vector in homogeneous coordinates
        xi_vector_prime (array): the x_i_prime vector in homogeneous coordinates
    '''
    assert(xi_vector.shape == (3,) and xi_prime_vector.shape == (3,))

    # Insert your code here
    zero_vector = np.zeros((3,), dtype=np.float32)
    xi, yi, wi = xi_prime_vector
    
    Ai = np.stack([
        np.concatenate([zero_vector, -wi*xi_vector, yi*xi_vector]),
        np.concatenate([wi*xi_vector, zero_vector, -xi*xi_vector]),
    ])
    assert(Ai.shape == (2, 9))
    return Ai

###########################
#### Exercise Function ####
###########################
def get_A(points_source, points_target):
    ''' Returns the A matrix discussed in the lecture.
    
    Args:
        points_source (array): 3D homogeneous points from source image
        points_target (array): 3D homogeneous points from target image
    '''
    N = points_source.shape[0]

    # Insert your code here
    correspondence_pairs = zip(points_source, points_target)
    
    A = np.concatenate([get_Ai(p1, p2) for (p1, p2) in correspondence_pairs])
    assert(A.shape == (2*N, 9))
    return A

```

[矩阵（二）：为什么Ax=0的解为最小奇异值对应的向量？_矩阵最小奇异值-CSDN博客](https://blog.csdn.net/jdy_lyy/article/details/117171934)

这个博客比较详细解释了为什么是取V对应最小的奇异值的奇异向量。

The solution to the above optimization problem is the singular vector corresponding to the smallest singular value of A (i.e., the last column of V when decomposing $A =UDV^T$. The resulting algorithm is called Direct Linear Transformation.