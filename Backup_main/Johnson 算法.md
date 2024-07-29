### 前言  
大杂烩！  
### 前置知识  
[Dijstra 算法](https://www.luogu.com.cn/blog/home-sunningyi/dijstra-suan-fa)  
[Spfa 算法](https://www.luogu.com.cn/blog/home-sunningyi/spfa-suan-fa)  
## 1. 原理  
以往的最短路算法大多只能以较低的时间复杂度处理单源最短路问题。使用动态规划思想的 Floyd 算法虽然可以求解出全源最短路，但是时间和空间消耗巨大，有没有一种算法可以集各家之长呢？  
容易发现，跑 $n$ 次 Dijstra 算法就可以在 $O(nm\log n)$ 的时间复杂度内求解全源最短路。但是 Dijstra 算法针对无负权边的图。  
Johnson 算法就是一种可以对任意图求解全源最短路的算法，而且其复杂度为 $O(nm\log n)$，可以说十分优秀。而且还可以判断负环。  
在目前的最短路算法中仅有 Dijstra 算法的时间复杂度是稳定的 $O(m\log m)$ 那么我们可不可以对原来的图稍作改动，使其没有负权边，进而可以使用 Dijstra 算法快速求解呢？  
我们很容易想到对每一条边的权都加上一个值让边权中没有负权边，但这是错误的，[Hack](https://oi-wiki.org/graph/shortest-path/#johnson-全源最短路径算法)。  
Johnson 算法则是先创建一个超级源点，向各个顶点链接一条权值为 $0$ 的边，求解出这个超级源点到每一个节点的最短路，对于节点 $i$，这样的最短路记为 $dist_i$。  
接下来根据原图的数据另外创建一个图，新图中的边权记为 $nw(i,j)$，原图的边权记为 $w(i,j)$，其中 $i,j$ 为边连接的两点编号，则：  
$$nw(i,j)=w(i,j)+dist_i-dist_j$$  
之后使用 Dijstra 算法遍历每个点求解全源最短路，任意两点间在新图中的最短路记为 $dis(i,j)$，则其在原图中的最短路为：  
$$dis(i,j)-dist_i+dist_j$$  
为什么这样的算法正确呢？我们先看看为什么要这样处理边权：  
考虑在原图中任意两点 $i,j$ 中的最短路，设经过 $k$ 个点，路径中的点编号为 $k_i$，则其最短路长度可以这样不严谨地表示，其中 $w(i,j)$ 的定义如上文：  
$$w(i,k_1)+\sum_{i=1}^{k-1}w(k_i,k_{i+1})+w(k_k,j)$$  
在新图中任意两点 $i,j$ 中的最短路也可以这样表示： 
$$nw(i,k_1)+\sum_{i=1}^{k-1}nw(k_i,k_{i+1})+nw(k_k,j)$$  
根据我们对新图边的操作，带人回去可得：  
$$w(i,k_1)+dist_i-dist_{k_1}+\sum_{i=1}^{k-1}\left(w(k_i,k_{i+1})+dist_{k_i}-dist_{k_{i+1}}\right)+w(k_k,j)+dist_{k_k}-dist_j$$  
大胆展开它：  
$$w(i,k_1)+dist_i-dist_{k_1}+w(k_{1},k_2)+dist_{k_1}-dist_{k_2}+\cdots+w(k_k,j)+dist_{k_k}-dist_j$$  
我们发现，其中的 $dist_{k_i}$ 项都会被约掉，最后只剩：  
$$w(i,k_1)+\sum_{i=1}^{k-1}w(k_i,k_{i+1})+w(k_k,j)+dist_i-dist_j$$  
也就是说，新图的两点之间最短路其实就是求出了这个式子的值，那我们把这个式子 $-dist_i+dist_j$ 就可以求出原图的最短路了！  
但是，为什么新图可以直接使用 Dijstra 算法呢？  
这也就是要证明：新图中不存在负权边。  
首先考虑一个很显然的式子：  
$$dist_j\le dist_i+w(i,j)$$  
原因很显然：如果 $j$ 点的最短路大于 $i$ 点的最短路与这两点之间边的权值的和，那么 $j$ 点目前的“最短路”就不是最短路，一定会在求最短路过程中被松弛掉。  
之后我们把这个不等式移项：  
$$0\le dist_i-dist_j+w(i,j)$$  
等会，不等式右边不正是我们对新图的边权做的操作吗？  
于是可以证明新图中所有边的权值均非负，可以直接使用 Dijstra 算法。  
## 2. 实现  
初始的求解 `dist` 数组一般使用 SPFA 算法实现，因为 SPFA 的时间复杂度最坏只有 $O(nm)$，而这个问题的时间复杂度瓶颈在于遍历每一个节点的 Dijstra。   
Johnson 算法可以在 SPFA 的过程中判断负环。  
示例代码如下：  
```cpp
int main(){
	
	cin>>n>>m;
	for(int i=1;i<=m;i++){
		int u=rd,v=rd,w=rd;
		st[i].u=u;
		st[i].v=v;
		st[i].w=w;
		edge1[u].push_back(edges{v,w});//建初始图并保存边的信息来建新图 
	} 
	for(int i=1;i<=n;i++){//建超级源点 
		edge1[0].push_back(edges{i,0});
	} 
	spfa(0);//spfa 模板  
	//函数求解了 dist[i] 
	if(fuh){//有负环 
		cout<<-1;//无解 
		return 0; 
	}
	for(int i=1;i<=m;i++){
		int u=st[i].u,v=st[i].v,w=st[i].w;
		edge[u].push_back(edges{v,w+dist[u]-dist[v]});//建新图 
	}
	for(int i=1;i<=n;i++){
		dij(i);//Dijstra 模板 
		for(int j=1;j<=n;j++){
			if(i==j)continue;
			//dis[j]-dist[i]+dist[j] 就是从 i 到 j 的最短路了~ 
		}
	}
	
	return 0;
}
```
## 3. 最短路算法一览表  
| 算法名 | Dijstra | Floyd | Spfa | *Johnson* |
| -----------: | ------------: | -----------: | -----------: | -----------: |
| 最短路类型 | 单源最短路 | 全源最短路 | 单源最短路 | 全源最短路|
| 适用的图 | 无负权图 | 任意图 | 任意图 |任意图|
| 可以判断负环 | 不可以 | 可以 | 可以 | 可以 |
| 时间复杂度 | $O(m\log m)$ | $O(n^3)$ | $O(nm)$ | $O(nm\log m)$ |
