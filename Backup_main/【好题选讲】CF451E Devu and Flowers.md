### 前言  
**前置知识**：[容斥原理](https://www.luogu.com.cn/article/zwjnfivy)，[组合计数，不定方程正整数解计数](https://www.luogu.com.cn/article/dd8qkaxu)。  
**重要思想**：正难则反的思想。  
[原题洛谷链接](https://www.luogu.com.cn/problem/CF451E)  
## 题面  
>Devu 有 $n$ 个花瓶，第 $i$ 个花瓶里有 $f_i$ 朵花。他现在要选择 $s$ 朵花。
>
>你需要求出有多少种方案。两种方案不同当且仅当两种方案中至少有一个花瓶选择花的数量不同。
>
>答案对 $10^9+7$ 取模。
>
>$1\le n\le 20,0\le f_i\le 10^{12},0\le s\le 10^{14}$  
## 形式化题面  
求不定方程 $x_1+x_2+\cdots+x_n=s \ \ (0\le x_i\le f_i,x_i\in \mathbb{Z})$ 解的种类数，答案对 $10^9+7$ 取模。  
## 思路详解  
这个题目在以往不定方程解计数的问题上加入了上界的限制，这导致问题不易从正面入手。但别忘了，我们可以求出所有不带上界的情况数。这启发我们：可不可以再求出所有不合法（一个或多个 $x_i>f_i$）的情况数，与总情况数相减进而得到答案呢？  
我们先考虑特殊情况：如何计算当 $x_i$ 不合法时的情况数呢？   
既然 $x_i$ 不合法，这表明 $x_i$ 在那不合法的解的组合中一定至少是 $f_i+1$。  
这样就变成了不定方程正整数解计数问题中规定下界的 （$x_i$ 下界为 $f_i+1$ ，其他 $x$ 为 $0$）问题了，可以直接用公式计算结果。  
但是，这样算出的情况数并未只保证解集中有且只有 $x_i$ 不合法。我们需要求出的是每一个 $x_i$ 不合法的情况的并集的数量。  
求一些相互包含的集合的并集的元素数量？这不正是容斥原理的应用？  
但在使用容斥之前，我们还要考虑：如何求出多个集合的交集？也就是求出多个 $x_i$ 不合法时的方案数。  
很简单，这也是一个规定了下界的不定方程正整数解计数问题，我们选择要计算那些 $x_i$ 的交集，就把这些 $x_i$ 的下界设为 $f_i+1$，其他的设为 $0$ 即可，也可以用公式快速解决。  
至此，思路大致理清，设计一下代码过程：  
1. 通过公式 $\dbinom{s+n-1}{n-1}$ 计算出所有可能的解的情况。  
2. 使用容斥模板，通过 $\dbinom{s-\sum_{i=1}^n a_i +n-1}{n-1}$ ，计算交集情况数，记录答案求出不合法情况并集数量。  
3. 总数 $-$ 不合法数 即为合法的解的数量。  
## 代码  
```cpp
#define rd read()
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll mod=1e9+7;
ll num[25];
ll ksm(ll a,ll b){
	ll ans=1;
	while(b){
		if(b&1==1){
			ans=(ans%mod*a%mod)%mod;
		}
		a=(a%mod*a%mod)%mod;
		b>>=1;
	}
	return ans%mod;
}
ll inv=1;
/*
使用第二种求解组合数的方法（前置芝士里），因为这里s+n-1 与 n-1 相接近，
且 b 一直等于 n-1，可以预处理 n-1 的逆元加快一些速度 
*/ 
ll C(ll a,ll b){
	if(a<b)return 0;
	ll jc1=1;
	for(ll i=a;i>a-b;i--){
		jc1=(jc1%mod*(i%mod))%mod;
	}
	return (jc1%mod*inv%mod)%mod;
}
int main(){

	ll n,s;
	cin>>n>>s;
	for(ll i=0;i<n;i++){
		cin>>num[i];
	}
	ll jc=1;
	for(ll i=1;i<=n-1;i++){
		jc=(jc%mod*i%mod)%mod;
	}
	inv=ksm(jc,mod-2)%mod;//计算组合数一直要用的逆元，预处理一下 

	ll total=C(s+n-1,n-1)%mod; //所有可能情况 
	ll ng=0;//not good 不合法情况数 
	for(ll i=1;i<(1<<n);i++){//容斥模板 
		ll bs=s+n-1;
		int cnt=0;//集合数量计数 
		for(int j=0;j<n;j++){
			if((i>>j)&1){
				cnt++;
				bs-=num[j]+1;
			}
		}
		if(cnt&1){//奇正偶负 
			ng=(ng%mod+C(bs,n-1)*1%mod)%mod;
		}
		else{
			ng=(ng%mod+C(bs,n-1)*-1%mod)%mod;
		}
	}
	cout<<(total-ng+mod)%mod;//总的减去不合法的即为答案 
	
	return 0;
}
```  
## 总结 $+$ 鲜花  
如果对组合计数和容斥用的比较熟练的化这题还是比较好想的，显然我用的还不熟练。  
作者第一次写的时候把总情况和不合法情况安在一个变量里算了，像这样：  
$$Ans=T-NG$$  
$NG$ 的容斥公式是这样：  
$$NG=S_1+S_2-S_1\cap S_2+\cdots $$  
然后作者就天真的用奇正偶负的规则直接对 $Ans$ 计算了...  
但其实这样是完全反的，因为 $NG$ 前是个减号：  
$$\begin{aligned}
    Ans&=T-NG\\
    &=T-(S_1+S_2-S_1\cap S_2+\cdots)\\
    &=T-S_1-S_2+S_1\cap S_2+\cdots\\
\end{aligned}$$  
因此，要想直接对 $Ans$ 加减，规则应是奇负偶正。  
这告诉我们做数学题先写好公式在敲代码是多么重要！
