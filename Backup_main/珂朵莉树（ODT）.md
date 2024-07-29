### 前言 & 鲜花 
暴力美学，骗分神器   
珂朵莉树来自[这道题](https://codeforces.com/problemset/problem/896/C)。  
这篇文章将其作为模板题。    
## 1. 原理  
珂朵莉树本质还是暴力，但是它将每个数值相同的区间使用一个块独立保存，优化了一些时间。  
很显然，如果保证样例在处理过程中没有两个相邻的数字是一样的，珂朵莉树就和暴力没区别了，甚至还有比暴力更高的复杂度。  
由于上面的限制，珂朵莉树适合处理有 “区间推平” 操作的题目，比如我们的模板题中提到的操作 $2$：  
>$2$  $l$  $r$  $x$ ：将$[l,r]$ 区间所有数改成$x$。   

大致来说，珂朵莉树使用 `set` 维护相同值组成的块组成的序列，对于每一次操作，首先使用 `set` 查找可能的左右区间并对这两个区间进行分离（）

因为是数据结构，先来说说 `struct` 怎么写：  
珂朵莉树维护相同值的块，因此其中存储着区间左端点 `int l`，区间右端点 `int r`，区间值 `mutable long long v`：  
```cpp
#define rd read()
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll read(){
	ll x=0,f=1;
	char c=getchar();
	while(c>'9'||c<'0'){if(c=='-') f=-1;c=getchar();}
	while(c>='0'&&c<='9'){x=(x<<3)+(x<<1)+(c^48);c=getchar();}
	return x*f;
}
const int OTHER=1e9+7;
int idx=0;
struct seg{
	int ls;
	int rs;
	int lzt;
	int v;
	seg(){
		ls=0;
		rs=0;
		lzt=OTHER;
		v=0;
	}
};
vector<seg> segtree;
int rt;
void pushdown(int p,int l,int r){
	if(!segtree[p].ls){
		segtree[p].ls=++idx;
		segtree.push_back(seg());
	}
	if(!segtree[p].rs){
		segtree[p].rs=++idx;
		segtree.push_back(seg());
	}
	if(segtree[p].lzt==OTHER){
		return;
	}
	int mid=(l+r)>>1;
	int k=segtree[p].lzt;
	int ls=segtree[p].ls;
	int rs=segtree[p].rs;
	segtree[ls].v=k*(mid-l+1);
	segtree[rs].v=k*(r-(mid+1)+1);
	segtree[ls].lzt=k;
	segtree[rs].lzt=k;
	segtree[p].lzt=OTHER;
}
void add(int &p,int l,int r,int tl,int tr,int op){
		
	if(!p){
		segtree.push_back(seg());
		p=++idx;
	}
	if(tl<=l&&r<=tr){
		segtree[p].v=op*(r-l+1);
		if(op==0) 
			segtree[p].lzt=0;
		else 
			segtree[p].lzt=op;
		return;
	}
	pushdown(p,l,r);
	int mid=l+r>>1;
	if(tl<=mid){
		add(segtree[p].ls,l,mid,tl,tr,op);
	}
	if(mid<tr){
		add(segtree[p].rs,mid+1,r,tl,tr,op);
	}
	segtree[p].v=segtree[segtree[p].ls].v+segtree[segtree[p].rs].v;
	
}
int main(){
	
//	freopen("dp.in","r",stdin);
	int n,q;
	cin>>n>>q;
	segtree.push_back(seg());
	for(int i=1;i<=q;i++){
		int l=rd;
		int r=rd;
		int op=rd;
	//	cin>>l>>r>>op;
		if(op==1){
			add(rt,1,n,l,r,1);
		}
		else if(op==2){
			add(rt,1,n,l,r,0);
		}
		cout<<n-segtree[1].v<<'\n';
	}
	
	return 0;
}

```
