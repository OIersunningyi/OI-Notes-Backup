## 已知：  
三个整数 $n,m,p$，其中 $p$ 为质数。  
## 求证：  
$$\binom{m}{n} \equiv\binom{m \bmod p}{n\bmod p}\times \binom{\lfloor \frac{m}{p} \rfloor}{\lfloor \frac{n}{p} \rfloor}\ \ \ ( \operatorname{mod}p)$$  
## 证明：  
我们使用多项式系数比较的方式证明。  
首先引入二项式定理：  
$$(a+b)^p=\sum_{i=0}^p\binom{p}{i}a^{p-i}b^i$$  
把它放在一个同余式中：  
$$(a+b)^p \equiv\sum_{i=0}^p\binom{p}{i}a^{p-i}b^i\ \ \ (\operatorname{mod}p)$$   
由定理 2.6，显然我们可以化简这个同余式。  
考虑到 $\sum_{i=0}^p\binom{p}{i}a^{p-i}b^i$ 中首项和末项的系数都是 $1$，较为特殊。我们把它们拿出来，原式就变成这样：  
$$(a+b)^p \equiv a^p+b^p+\sum_{i=1}^{p-1}\binom{p}{i}a^{p-i}b^i\ \ \ (\operatorname{mod}p)$$   
由定理 2.6，$\sum_{i=1}^{p-1}\binom{p}{i}a^{p-i}b^i$ 中的每一项显然都可以被 $p$ 整除。  
于是原式变成这样：  
$$(a+b)^p \equiv a^p+b^p\ \ \ (\operatorname{mod}p)$$  
那么对于 $p^2$，这个式子成不成立呢？我们推导一下：  
$$(a+b)^{p^2} \equiv (a^p+b^p)^p\ \ \ (\operatorname{mod}p)$$  
$$(a+b)^{p^2} \equiv (a^{p^2}+b^{p^2})\ \ \ (\operatorname{mod}p)$$  
于是，通过数学归纳法，我们可以证明，对于任意的 $i\in\mathbb{Z}^+$，都有：  
$$(a+b)^{p^i} \equiv (a^{p^i}+b^{p^i})\ \ \ (\operatorname{mod}p)$$  
**我自己**把这个式子称为**二项式定理在同余下的的推广**。   
接下来进入真正的证明，~~偷懒就不写同余式了~~。  
考虑一个整数 $m$，把它分解为 $p$ 进制，则 $m$ 可以写成：  
$$m=\sum_{i=0}^{s}m_0p^i$$  
考虑式子 $(1+x)^m$，将 $m$ 的 $p$ 进制分解代入得：  
$$(1+x)^m=(1+x)^{\sum_{i=0}^{s}m_ip^i}$$  
由同底数幂的乘除知识可得：（同底数幂相乘，底数不变，指数相加，这里反过来）  
$$\begin{aligned}
    (1+x)^{\sum_{i=0}^{s}m_ip^i}=\prod_{i=0}^{s}(1+x)^{m_ip^i}
\end{aligned}$$  
再由幂的乘方知识可得：  
$$\begin{aligned}
    \prod_{i=0}^{s}(1+x)^{m_ip^i}=\prod_{i=0}^{s}\left((1+x)^{p^i}\right)^{m_i}
\end{aligned}$$  
把 $(1+x)^{p^i}$ 用上文证明过的二项式定理在同余下的的推广变换得：  
$$\begin{aligned}
    \prod_{i=0}^{s}\left((1+x)^{p^i}\right)^{m_i}&=\prod_{i=0}^{s}\left(1^{p^i}+x^{p^i}\right)^{m_i}\\
\end{aligned}$$  
再次对 $\left(1^{p^i}+x^{p^i}\right)^{m_i}$ 应用二项式定理：  
$$\begin{aligned}
   \prod_{i=0}^{s}\left(1^{p^i}+x^{p^i}\right)^{m_i}&=\prod_{i=0}^{s}\sum_{j=0}^{m_i}\dbinom{m_i}{j}x^{p^ij}1^{p^i(m_i-j)}\\
   &=\prod_{i=0}^{s}\sum_{j=0}^{m_i}\dbinom{m_i}{j}x^{p^ij}
\end{aligned}$$  
反过头来考虑 $(1+x)^m$ 的二项式展开：  
$$\begin{aligned}
    (1+x)^m&=\sum_{i=0}^{m}\dbinom{m}{i}x^i1^{m-i}\\
    &=\sum_{i=0}^{m}\dbinom{m}{i}x^i
\end{aligned}$$  
由于两式子并未发生任何变化，只是展开方式不同，因此两式子中每一项对应的幂应该是**一样的**。  
考虑 $x^n$ 这一项，我们也把 $n$ 写成 $p$ 进制的形式：  
$$n=\sum_{i=0}^{k}n_ip^i$$  
观察 $\prod_{i=0}^{s}\sum_{j=0}^{m_i}\dbinom{m_i}{j}x^{p^ij}$ 的结构，我们发现它是一个累乘套着一个累加的形式，即形如：  
$$(a_1+a_2+\cdots+a_{m_0})(b_1+b_2+\cdots+b_{m_1})\cdots$$  
那么，对于 $x^n$ 这一项，其在这个式子中一定是由 $s+1$（因为 $i$ 的值从 $0$ 开始）个 $x^{p^ij_i}$ 相乘凑出来的，我们把 $x$ 省略，由同底数幂的乘除知识可得：  
（同底数幂相乘，底数不变，指数相加。）  
$$n={\sum_{i=0}^{s}p^ij}$$  
也就是说：  
$$\sum_{i=0}^{k}n_ip^i={\sum_{i=1}^{s}p^ij_i}$$  
由于 $n$ 在 $p$ 进制下仅可能有一种表示方法，所以在等号的左右两边 $k=s$，$n_i=j_i$  
**仔细理解这个过程！**  
那么，我们把每一个 $j_i$ 对应的 $n_i$ 代入回去可得：  
$$x^n=\prod_{i=0}^{s}x^{p^in_i}$$  
同时，由于 ”$x^{p^ij_i}$ 相乘凑出来“ 的过程中也将系数相乘了，我们将原式的系数加入进式子中：  
$$ \dbinom{m}{n}x^n=\prod_{i=0}^{s}\dbinom{m_i}{n_i}x^{p^in_i}$$  
用乘法结合律把求积符号分开：  
$$ \dbinom{m}{n}x^n=\prod_{i=0}^{s}\dbinom{m_i}{n_i}\prod_{i=0}^{s}x^{p^in_i}$$  
由于 $x^n=\prod_{i=0}^{s}x^{p^in_i}$，因此：  
$$\dbinom{m}{n}=\prod_{i=0}^{s}\dbinom{m_i}{n_i}$$  
证完了？证完了！  
它和我们的求证有啥关系？  
回想我们学习进制转换的过程，十进制转二进制用的是 “除 $2$ 取余法”。  
那么在这里，$n,m$ 首先对 $p$ 取余，在除以 $p$ 继续递归计算的过程，不正是求解 $n,m$ 的 $p$ 进制的过程吗？  
而我们求证的式子正好是使用了 $n,m$ 的 $p$ 进制求解的组合数，因此可以证明原式。  
[返回博客](https://www.luogu.com.cn/blog/home-sunningyi/zu-ge-shuo-yu-zu-ge-ji-shuo)  




