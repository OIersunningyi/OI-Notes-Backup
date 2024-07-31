二进制贪心时一般从高位到低位  
有关阶乘的整除问题尝试将几个阶乘合并。  
正着推被卡了就反着推，不管是在思路上还是代码上。  
看到取模推公式想转化：$a \bmod b=a-\left \lfloor\dfrac{a}{b}\right \rfloor b $。  
图论中不同时段开放的点想 floyd 性质。  
数学题全开 `long long` 包括输入数据。  
数论分块注意求和右边界 [P2261 [CQOI2007] 余数求和](https://www.luogu.com.cn/problem/P2261)  
偏模拟类题目可以直接求出某些种类的答案满足的模拟条件极限之后可以根据这样极限进行选择或者多次询问 [CF 某题](https://vjudge.net/contest/638234#problem/E)。  
试试将输入数据分开处理 [NOI 2015 并查集](https://www.luogu.com.cn/problem/P1955)  
在过程中的某种行为可以尝试推到终点去做，无需在意发生的位置前提下。（幻兽对战）   
线段包括问题可以统一右端点后看左端点性质（或者反过来）。  
选几个数（可能带权）的最值，转换为前缀，后缀最大值+枚举  
看到区间改想差分。  
注意重边卡人，建议使用链式前向星存边：  
[被卡 1](https://www.luogu.com.cn/discuss/872078)，[被卡2](https://www.luogu.com.cn/discuss/805599)  
区间 dp 消除后效性可以假设两边都已经被处理完 [洛谷 P1622](https://www.luogu.com.cn/problem/P1622)  
某些题目可能不需要关系性质，只需要根据答案和总的性质枚举就可以。  
$\bmod 2$ 怎么怎么样 $\rightarrow$ 只关心二进制最低位 $\rightarrow$ 位运算操作。  
用 `lgr` 常量记得加上几位：  
![被次小生成树硬控2h.png](https://cdn.luogu.com.cn/upload/image_hosting/xy88rqz5.png)
