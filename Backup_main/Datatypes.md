### 1.`int` 类型  
数据范围 ：$2^{31}-1 \sim -2^{31}$，约正负 $21$ 亿，$4$ 个字节。   
其它如 `long`，`int long`，`long int` 均与 `int` 类型有着同样的取值范围。  
没啥好说的了qwq。  
### 2.`long long` 类型  
数据范围：$2^{63}-1 \sim -2^{63}$，最大值约 $10^{18}$，$8$ 个字节。  
**十年OI一场空，不开 long long 见祖宗**  
在数据范围明确提示需用 `long long` 或是内存允许的情况下尽量用 `long long`。  
与 `__int64` 的大小相同。  
### 3.`short` 类型  
数据范围：$2^{15}-1 \sim -2^{15}$，最大值 $32767$，$2$ 个字节。  
还是用int，某些时候的计算不如 `int` 快，没必要省空间。  
### 4.`char` 类型  
表面上是存的字符，实际上存储字符的`ASCII`码。  
数据范围：$2^{7}-1 \sim -2^{7}$，最大值 $127$，$1$ 个字节。  
字符型支持逻辑运算，如 `&&` ，`||` 等，也支持普通运算（加减乘除模且自动转 `int` 类型），但不支持位运算。  
### 5.`bool` 类型  
数据范围：$0$ 或 $1$，$1$ 个字节。  
将任何非零值赋给 `bool` 都会赋值为 $1$，包括负数。  
### 6.`__int128` 类型  
十分大的数据类型，不想写高精度时可以一试。  
数据范围：$2^{127}-1 \sim -2^{127}$，最大值约 $38$ 位数，$16$ 个字节(存疑)。  
过于远古，仅支持四则运算，没有适配的输入输出，可以用快读快写的板子改过来，一种实现如下：  
```cpp
#define int __int128//开始偷懒 
void write(int x){
	if(x<0){
		putchar('-');
		x*=-1;
	}
	if(x>9)write(x/10);
	putchar((x%10)^48);//这里的模运算自动转换为了int类型，因此可以使用位运算 
}
int read(){
	int x,f=1;
	char c;
	c=getchar();
	while(c>'9'||c<'0'){
		if(c=='-'){
			f=-1;
		}
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		x=(x*10)+(c^48);//这里只能使用乘号操作 
		c=getchar();
	}
	return x*f;
}
#undef int //偷懒后一定要关上！
```  
### 7.`unsigned` 关键字的作用  
`unsigned` 加于上文提到的所有数据结构的前面，将符号位让出以存储数据，因此无法表示负数，最小值是 $0$。  
数据范围：会让 $2^{原始值+1}$ ，例如 `unsigned long long` 为 $2^{64}-1 \sim 0$，`unsigned __int128` 为 $2^{128}-1 \sim 0$。  
### 8.`function` 函数  
常用于将代码模块化，在保证每一模块实现的情况下使代码更易读，更易调试。  
**注意**，非 `void` 的函数必须有返回值，否则会 RE。  
函数可以自己调用自己，形成递归。  
函数可以预先声明但不实现，而是放在程序尾部实现，如以下程序：  
```cpp
struct nodew{
    int val;
    bool israted;
};
nodew f(int i,int k);//只声明不实现
int main(){
	...
    f(1,2).val;//在主函数中仍可以调用
	...
    return 0;

}
nodew f(int i,int k){//可以放在程序尾部实现，实现必须存在
    cout<<"IAKIOI";
    return nodew{114514,true};
}
``` 
函数类型可以是 STL，可以自定义，如上方程序的结构体类型函数。

### 9.`struct` 结构体  
将一些数据组成一组，联系到一起。在赋值，交换，排序时不会乱。  
`struct` 的定义如下：  
```cpp  
struct thestruct{
	int ...;
	void f{...};
    //数据或函数

}//定义一些对象;
```   
其中 `thestruct` 是一种数据类型，可以通过 `thestruct ts` 的方式定义一个类型为 `thestruct` 的名为 `ts` 的变量（对象）。访问时用 `.` 字符访问其中成员变量或成员函数。数组同样支持定义。  
结构体在使用 `sort` 等排序函数时必须写明排序规则（即 `cmp` 函数），否则报错。

结构体中存在内存对齐现象，例如：  
```cpp
struct s{
	bool v;
	int k;
};
```  
一个 `int` 占 $4$ 字节；一个 `bool` 占 $1$ 字节，但在这里会被自动对齐 `int`，占 $4$ 字节。此时这个结构体占 $8$ 字节。因此要谨慎估计结构体占用的内存，防止 MLE。  
  
一些小操作：
- 运算符重载：  
  自定义结构体在 `priority_queue`、`map` 或 `set` 等自动排序容器里的排序规则。  
  ```cpp
  struct satt{
    	int nodeid;
    	int val;
   	bool operator<(const satt &x) const{
    		if(x.val==val){
     	   		return x.nodeid<nodeid;
     		}
      		return x.val<val;//类似于 cmp 函数
		}
	};
	int main(){
 	   priority_queue<satt> pi;//在优先队列 pi 中按 val 值从小到大排，val 相等时按 nodeid 值从小到大排  
	}
  ```  
- 构造函数  
	使用构造函数让结构体在定义时附上初值：  
	构造函数的特点：函数没有类型，函数名与结构体名相同  
	```cpp  
	struct satt{
    	int nodeid;
    	int val;
    	satt(int rootnode){//函数没有类型，函数名与结构体名相同.可不传入参数
     	   nodeid=rootnode;//初始化为传入的 rootnode 值
     	   val=0;//初始化为 0 
    	}
	};
	int main(){

    	satt a(114514);//给 rootnode 传入参数，让构造函数将 nodeid 初始化为 rootnode 的值

	}
	```
 
### 10.`union` 联合体  
~~不知道有什么用~~  
类似结构体的定义。  
其占用的内存为其中单个成员占用的最大内存。 

### 11.`class` 类  
~~不知道有什么用~~  
高级的结构体。  
似乎在某些大模拟题目有用。 

### 12.`pointer` 指针  
指针，顾名思义就是指向一个数据类型地址的箭头，非常灵活。  
定义：` T *p = &data`。其中 `p` 为指针名，`&`可以取得 `data` 的地址。  
指针可以嵌套：`T **q = &p` 获得指针 `p` 的地址并赋值给另一个指针。  
想取得指针里的值，使用 `*` 解引用符号：`T datas=*p;`。  
指针直接指向数据的地址，因此进行函数调用时，指针将无视形参和实参的概念，直接对源数据更改。 
```cpp
void add(int *a){
	(*a)++;//*a指针指向主函数中的a变量 
}
int main( ){
 	
 	int a=10;
 	int *p=&a;
 	add(p);
 	cout<<a; //输出 11 
}

```   
### 13.`lambda` 仿函数  
~~不知道有什么用~~  
可以省一个 `cmp` 函数。  
```cpp  
int a[8]={0,1,2,4,2,1,-2,3};
sort(a,a+8,[](int x,int y) -> bool { 
    return x<y;//正着排序 
} );
```
-----  
迁移自[云剪贴板](https://www.luogu.com.cn/paste/g3nswbcj)

