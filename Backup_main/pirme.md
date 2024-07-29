## 前言  
对于这篇文章中提到的方法的证明见[这里](https://www.luogu.com.cn/blog/home-sunningyi/shuo-xue-yu-shuo-lun-P1)。  
迁移自[云剪贴板](https://www.luogu.com.cn/paste/jqk2vtmq)。  
**板子敲错见祖宗。**  
### 1. $O(n)$ 朴素做法  
对于一个数 $n$，枚举从 $2 \sim n-1$ 的每一个数，如果有一个数能整除 $n$，即代表 $n$ 是合数而非质数。平均时间复杂度为 $O(n)$。其正确性和时间复杂度的证明十分显然。  
板子：  
```cpp
bool isprime(int n){
	for(int i=2;i<n;i++){
		if(n%i==0)return 0;
	}
	return 1;
}
```  
### 2. $O(\sqrt n)$ 浅浅优化后做法  
枚举 $\sqrt n \sim n-1$ 的这些数都是没用的，因此可以将时间复杂度优化为 $O(\sqrt n)$。  
板子：  
```cpp  
bool isprime(int n){
	for(int i=2;i*i<=n;i++){//用 i*i<=n 以避免 sqrt(n) 产生的浮点误差
		if(n%i==0)return 0;
	}
	return 1;
}
```  
### 3. $O(n\log {\log n})$ 埃氏筛做法  
证明见前言。  
全称为埃拉托尼斯筛。过程是将某个质数的倍数标记为合数并重复这一过程，直到筛出所有没有被标记过的数，即为质数。  
运用了离线思想，可用 $O(n\log {\log n})$ 预处理 $1 \sim n$ 范围内所有质数，$O(1)$ 查询。
```cpp  
bool isnt_prime[10000005];//有时写作 is_prime，但用处相同，都用于记录是否是质数或合数。 
void Eratonis_Sieve(int n){//n 为可能的最大质数所在的范围
	for(int i=2;i*i<=n;i++){
		if(!isnt_prime[i]){//如果其不是合数，则标记其倍数
			for(int j=i+i;j<=n;j+=i){//注意不要标记自己!!! j=i+i
				isnt_prime[j]=1;//标记
			}
		}
	}
}
```  
### 4. $O(n)$ 欧拉筛做法  
证明见前言。  
与埃氏筛相同的标记倍数的思想，但其复杂度可做到 $O(n)$。  
可用 $O(n)$ 预处理 $1 \sim n$ 范围内所有质数，$O(1)$ 查询。  
```cpp  
bool is_prime[1000005];//标记列表
int prime_list[1000005];//质数列表
int num=0;//质数列表的个数

void Euler_Sieve(int n){
	memset(is_prime,1 ,sizeof is_prime);//初始化，所有数都是质数，可以改变标记方式以省去 
	is_prime[1]=0;// 1既不是质数也不是合数 
	for(int i=2;i<=n;i++){
		if(is_prime[i])prime_list[++num]=i;//加入素数列表 
		for(int j=1;j<=num&&i*prime_list[j]<=n;j++){
			is_prime[i*prime_list[j]]=0;//同埃氏筛一样用倍数法标记 
			if(i%prime_list[j]==0)break;//退出避免重复标记 
		}
	}
}
```