### 前言  
回顾一下初中所学，并且练习打字速度和 $\LaTeX$。  
### 1.整式  
#### 1.1 整式的定义  
整式是单项式与多项式的统称。  
#### 1.2 单项式的定义    
都是数与字母的乘积的式子叫做单项式，单项式中，数字因数为这个单项式的**系数**，所有字母的**指数和**叫做这个单项式的**次数**。  
特殊地，单独一个数和一个字母也是单项式。  
#### 1.3 多项式的定义  
几个单项式的和叫做多项式。  
在多项式中，每个单项式叫做这个多项式的**项**。不含字母的项叫做**常数项**。次数最高项的次数，叫做这个多项式的次数。  
#### 1.4 同类项及其合并   
所含字母相同，并且相同字母的指数也相同的两个项叫做同类项。  
同类项合并时，把同类项的系数相加，字母和字母的指数不变。  
依据：乘法分配律。  
#### 1.5 整式的加减  
- 整式在加减时，要先去括号，在合并同类项。  
- 去括号时，前面是 $+$，则直接去；前面是 $-$，则括号内的项在去掉括号时都要变号。  
  
#### 1.6 整式的乘除  
#### 1.6.1 同底数幂的乘除  
同底数幂相乘，底数不变，指数相加。  
$$ a^m \times a^n=a^{m+n} \ \ \ (m,n\in \mathbb{Z}^+) $$  
同底数幂相除，底数不变，指数相减。注意除数不为 $0$。
$$ a^m \div a^n=a^{m+n} \ \ \ (a \neq 0,\ \  m,n\in \mathbb{Z}^+, \ \ m>n) $$  
#### 1.6.2 幂的乘方与积的乘方  
幂的乘方，底数不变，指数相乘。  
$$ (a^m)^n=a^{mn}\ \ \ (m,n\in \mathbb{Z}^+)$$  
积的乘方，等于把积的每一个因式分别乘方，再把所得的幂相乘。  
$$ (ab)^n=a^nb^n\ \ \ (n \in \mathbb{Z}^+)$$  

### 3.根式  
#### 3.1 根式的定义  
一般地，形如 $\sqrt[n]{a} \ \ \ (n \in \mathbb{Z}^+,a \ge 0)$ 的式子称为根式。  
特殊地，在 $\sqrt[n]{a} \ \ \ (a \ge 0)$ 中，当 $n=2$ 时，原式可写为 $\sqrt{a} \ \ \ (a \ge 0)$。这样的式子叫**二次根式**。  
**时刻注意根式下值的范围，可能存在负数因此在实数范围内无解的情况**  
#### 3.2 先乘方后开方与先开方后乘方  
对于根式 $(\sqrt[n]{a})^n$ 和 $\sqrt[n]{a^n}$ **当 $(a\in \mathbb{R})$**，有：  
$$(\sqrt[n]{a})^n=a\ \ \  (n\in \mathbb{Z^*})$$  
$$\sqrt[n]{a^n}=\begin{cases}
      a & n=2k+1,k\in \mathbb{Z^+}\\
      |a| & n=2k,k\in \mathbb{Z^+}
\end{cases}$$  
则对于二次根式  $(\sqrt{a})^2$ 和 $\sqrt{a^2}\ \ \ (a\in \mathbb{R})$，有  
$$(\sqrt{a})^2 =a$$  
$$\sqrt{a^2} = |a|$$
#### 3.3 二次根式的性质   
 - 二次根式具有**双重非负性**，即对于二次根式 $\sqrt{a}$，$\sqrt{a} \ge 0$ 且 $a\ge 0$。此时二次根式 $\sqrt{a}$ **在实数范围内**有意义。  
 - 当 $a\ge 0$ 时，$\sqrt{a^2}=a$，$(\sqrt{a})^2=a$。  
 - 被开方数不含分母，也不含开的尽的因数或因式，这样的二次根式叫做最简二次根式。  
  
#### 3.4 化简二次根式  
 - $\sqrt{ab} = \sqrt{a} \cdot \sqrt{b} \ \ \ (a\ge 0,\ b\ge 0)$  
  $\sqrt{a} \cdot \sqrt{b} = \sqrt{ab} \ \ \ (a\ge 0,\ b\ge 0)$  
  积的算术平方根等于积中各因式的算术平方根的积。  
 - $\sqrt{\frac{a}{b}}=\frac{\sqrt{a}}{\sqrt{b}}\ \ \ (a\ge 0,\ b> 0)$  
  $\frac{\sqrt{a}}{\sqrt{b}}=\sqrt{\frac{a}{b}}\ \ \ (a\ge 0,\ b> 0)$  
  商的算术平方根等于被除式的算术平方根除以除式的算术平方根。  
 - 化简被开方数是分数的二次根式  
   同乘配方，根号上移。  
   e.g.  
   >化简 $\sqrt{\frac{1}{2}}$。  
   同乘配方，得：$\sqrt{\frac{1 \times 2}{2 \times 2}}=\sqrt{\frac{2}{2^2}}$  
   根号上移，得：$\sqrt{\frac{2}{2^2}}=\frac{\sqrt{2}}{2}$  
   化简完成。  
 - 化简分母有根式的分式（分母有理化）  
   同乘凑整，约分化简。  
   e.g.  
   >化简 $\frac{1}{5\sqrt{2}}$。  
   同乘凑整，得：$\frac{1 \times 5\sqrt{2}}{5\sqrt{2} \times 5\sqrt{2}}=\frac{5\sqrt{2}}{50}$  
   约分化简，得：$\frac{5\sqrt{2}}{50}=\frac{1}{10}\sqrt{2}$  
   化简完成。  
 
#### 3.5 二次根式的混合运算  
如果两个二次根式的被开方数相同，那么这几个二次根式是**同类二次根式**。  
- 二次根式相加减时，要先将每个二次根式化简为最简二次根式，然后合并同类二次根式。  
不是同类二次根式不能合并。  
- 进行混合运算时可以按照单项式乘多项式和多项式乘多项式的法则进行计算。  
  
e.g.  
>  
> $计算\ (2\sqrt{3}-2)(3\sqrt{6}+\sqrt{2})$.  
$由多项式乘以多项式法则，有$  
>$$\begin{aligned}
      (2\sqrt{3}-2)(3\sqrt{6}+\sqrt{2}) &= 2\sqrt{3}\cdot 3\sqrt{6}+2\sqrt{3}\cdot \sqrt{2}-2\cdot 3\sqrt{6}-2\sqrt{2}
\end{aligned}$$  
>$合并每一项并化简为最简二次根式，得$  
>$$\begin{aligned}
      原式 &= 18\sqrt{2}+2\sqrt{6}-6\sqrt{6}-2\sqrt{2}\\
           &=16\sqrt{2}-4\sqrt{6}
\end{aligned}$$  

#### 3.X 秦九韶公式到海伦公式的推导  
**命题**：对于三角形的三边 $a,b,c$，设 $p=\frac{a+b+c}{2}$，证明：  
$$\sqrt{\frac{1}{4}\left[a^2b^2-\left(\frac{a^2+b^2-c^2}{2}\right)^2\right]}=\sqrt{p(p-a)(p-b)(p-c)}$$  
**证明**：  
$由平方差公式和完全平方公式，有$  
$$
\begin{aligned}
      \sqrt{\frac{1}{4}\left[a^2b^2-\left(\frac{a^2+b^2-c^2}{2}\right)^2\right]} &= \sqrt{\frac{1}{4}\left(ab+\frac{a^2+b^2-c^2}{2}\right)\left(ab-\frac{a^2+b^2-c^2}{2}\right)}\\  
      &=\sqrt{-\frac{1}{4}\left(\frac{a^2+b^2+2ab-c^2}{2}\right)\left(\frac{a^2+b^2-c^2}{2}-ab\right)}\\
      &= \sqrt{-\frac{1}{4}\left(\frac{a^2+b^2+2ab-c^2}{2}\right)\left(\frac{a^2+b^2-2ab-c^2}{2}\right)}\\  
      &= \sqrt{-\frac{1}{4}\left[\frac{(a+b)^2-c^2}{2}\right]\left[\frac{(a-b)^2-c^2}{2}\right]}\\  
      &=\sqrt{-\frac{1}{4}\left[\frac{(a+b+c)(a+b-c)}{2}\right]\left[\frac{(a-b+c)(a-b-c)}{2}\right]}\\  
\end{aligned}
$$  
$将根号下三项相乘，可得$  
$$
\begin{aligned}
      \sqrt{-\frac{1}{4}\left[\frac{(a+b+c)(a+b-c)}{2}\right]\left[\frac{(a-b+c)(a-b-c)}{2}\right]} &= \sqrt{-\frac{(a+b+c)(a+b-c)(a-b+c)(a-b-c)}{16}}\\  
\end{aligned}
$$    
$拆开根号下的式子$  
 $$
\begin{aligned}
      \sqrt{-\frac{(a+b+c)(a+b-c)(a-b+c)(a-b-c)}{16}} &= \sqrt{-\frac{a+b+c}{2}\cdot\frac{a+b-c}{2}\cdot\frac{a-b+c}{2}\cdot\frac{a-b-c}{2}}\\  
\end{aligned}
$$  
$将每一项配以\frac{a+b+c}{2}$  
 $$
\begin{aligned}
      \sqrt{-\frac{a+b+c}{2}\cdot\frac{a+b-c}{2}\cdot\frac{a-b+c}{2}\cdot\frac{a-b-c}{2}} &= \sqrt{-\frac{a+b+c}{2}\cdot\left(\frac{a+b+c}{2}-c\right)\cdot\left(\frac{a+b+c}{2}-b\right)\cdot\left(a-\frac{a+b+c}{2}\right
      )}\\  
\end{aligned}
$$  
$代入 \  p=\frac{a+b+c}{2}\ 得$  
 $$
\begin{aligned}
      \sqrt{-\frac{a+b+c}{2}\cdot\left(\frac{a+b+c}{2}-c\right)\cdot\left(\frac{a+b+c}{2}-b\right)\cdot\left(a-\frac{a+b+c}{2}\right
      )} &= \sqrt{-p(p-c)(p-b)(a-p)}\\  
      &=  \sqrt{p(p-a)(p-b)(p-c)} 
\end{aligned}
$$
$原式得证$