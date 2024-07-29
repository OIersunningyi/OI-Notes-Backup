### 前言  
STL（Standard Template Library）为我们封装了许多实用的数据结构、算法和操作函数。学会它们可以为我们敲代码的过程提供许多便利，但在使用之前也要先记住它们各自的用法。  

string 类型是 C++ 区别于 C 单独封装的字符串类型。相对与 C 的字符数组，C++的字符串类型提供了多种方便的操作函数，这些函数都在头文件 ```
<string>``` 中。

### 1.1 定义和基本操作  
string 类型与普通的变量类型相似，可直接用```cin```进行输入，但使用```scanf``` 时需进行预分配空间（一般不推荐用```scanf```）输入。  
```cpp   
#include<iostream>  
#include<string>  

string s;
cin>>s;  

/*另一种方法*/    
s.resize(100);
scanf("%s",&s);//此时输入长度不能超过100个字符，有很大局限性

```  
```string```同样支持多维数组的定义，一般情况下，```string```的类型本身即为一个一维数组，通过```[]```可直接访问这个字符串的每一个字符，下标从0开始。  
```cpp
string s[100]//支持数组定义，此时相当于一个二维字符型数组  
string test;
cin>>s[1]>>test;//输入方法相同  

for(int i=0;i<=5;i++)cout<<s[1][i]<<' ';//支持下标访问每一个字符
for(int i=0;i<=5;i++)cout<<test[i]<<' ';

```  
### 1.2 操作函数  
#### 1.2.1 查找函数 ```.find()```  
不同于 STL 中自带的 ```find()``` 函数，这个函数是 string 自己的查找函数。STL 自带的 ```find``` 仅能查找字符，但 string 的可以查找字符串。  
- **参数设置**  
```.find()``` 中可以传入字符或字符串。  
```.find()``` 返回从左往右第一个匹配成功的字串的第一个字符的下标，如果未能匹配成功则返回一个 ```string::npos``` ，在 ```if``` 语句中体现为 -1，可用作判断。
  
```cpp
string s="qwertytkkksc03chen_zhe";
cout<<s.find('t')<<'\n';//传入字符，此时输出第一个't'的下标，为4
cout<<s.find("kkksc03")<<'\n';//传入字符串，此时输出原串中第一个字串的第一个字符'k'在原串中的下标，为7。
```
```cpp
//判断未找到的两种方法，均可使用
string s="qwertytkkksc03chen_zhe";  
if(s.find("xht")==string::npos) cout<<"Not Found!";
if(s.find("xht")==-1)			cout<<"Not Found!";
```  

-----  
迁移自[云剪贴板](https://www.luogu.com.cn/paste/x1e4mw5z)
