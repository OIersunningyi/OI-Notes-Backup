### 前言    
万年老图
![](https://pica.zhimg.com/70/v2-4c9799ea33fb1fafe422e886674e067c_1440w.awebp?)  
## 1. 原理  
Spfa 是经过队列优化的 Bellman-Ford 算法，它的本质是 BFS，它的时间复杂度最好是 $O(V+E)$，最坏是 $O(VE)$，其中 $V$ 为节点数，$E$ 为边数。  

为什么要有这样一个时间复杂度这么不稳定的算法呢？因为它可以求出**有负权边的图**的最短路，并可以对最短路不存在的情况进行判断（即负环）。  

什么是负环呢？简单来说，负环一条边权之和为负数的回路。因此，当我们求解最短路时，可以无休止的经过同一个负环来让最短路变得更小，一直到无穷小，此时我们认为最短路不存在。  
从定义我们可以发现，负环**并不是环上有负数的环**。   

怎样实现呢？先来明确数组的定义。  
- 首先，Spfa 有一个同 Dijstra 算法一样的 `dist` 数组用于存储从原点出发的最短路。  
- 不同的，Spfa 虽然是 BFS 的方法，但是其 `vis` 数组并不是记录其是否被访问过，而是记录其是否在队列中。为什么？因为每个节点的松弛操作仅需要一次遍历，多次对同一个节点进行同样的松弛操作是无用的，因此加入这个数组来优化算法时间复杂度。 
- 不同的，Spfa 加入了一个新的 `cnt` 数组记录每一个节点被遍历到的次数，这个数组用来判断负环。什么原理？很显然，对于一个有 $E$ 条边的图，从一个点到另一个点的最短路的长度最多为 $E-1$ 条边。因此，如果一个节点被重复走了大于 $E-1$ 次，这个图从**开始的点出发**可以到达一个负环，可以用这样的方法来判断负环。  
  
队列的实现并无不同，根据 BFS 的基础可以写出如下代码。  
```cpp
memset(dist,0x3f,sizeof dist);//初始化所有数组 
memset(cnt,0,sizeof cnt);
memset(vis,0,sizeof vis); 
q.push(k);
vis[k]=1;//开始的节点先入队列，对有关的数组更新 
cnt[k]=0;
dist[k]=0;
while(q.size()){//开始 Spfa 
	int t=q.front();//取出队头节点 
	q.pop();
	vis[t]=0;//由于每一个节点只可能在队列中存在一次，所以取出这个节点后直接取消标记 
	for(int i=0;i<edge[t].size();i++){//开始遍历每一个邻接点 
		int v=edge[t][i].v,w=edge[t][i].w;
		if(dist[v]>dist[t]+w){//松弛操作 
			dist[v]=dist[t]+w;//更新最小值 
			cnt[v]=cnt[t]+1;//更新走过点的次数 
			if(cnt[v]>n-1){//出现了一个点被走了过于多的次数 
				cout<<"YES"<<'\n';//有负环 
				ansed=1;
			}
			else if(!vis[v]){//如果被遍历到的节点在队列中，则不用压入队列 
				q.push(v);//如果没有，压入队列并打上标记 
				vis[v]=1;
			}
		}
	}
}
```  
## 2. 应用  
### 2.1 判断负环  
