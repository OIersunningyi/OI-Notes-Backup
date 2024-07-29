## 前言
计算机中通常使用后缀表达式进行数值计算，因为后缀表达式计算简单，**无需括号**。且中 $\to$ 后缀表达式的操作是将中缀表达式从左往右处理，相比于前缀的从右往左更符合人类逻辑。  
**三种表达式的互相转换通常使用 栈 数据结构**。   
### 1. 中缀表达式转后，前缀  
#### 1.1 中缀转后缀  
规则如下：  
1. 从前往后遍历原中缀表达式。  
2. 遇到数字，直接进入后缀表达式。
3. 遇到字符，如果**严格大于**栈顶字符的优先级，则直接压入栈。  
4. 遇到字符，如果**小于或等于**栈顶字符的优先级，则弹出栈顶字符并直接进入后缀表达式，**直至此元素严格大于栈顶元素优先级或栈空**，此时将此元素压入栈。  
5. 如遇 `(` ，直接压入队列。如遇 `)`，弹出栈顶字符并直接进入后缀表达式，直至遇到第一个 `(` 。
6. 遍历完后，将栈中所有元素弹出并直接进入后缀表达式，直到栈空。 
7. 特殊的，对于`^`符号，如果匹配方式为 `2^3^3` $\to$ `2^(3^3)`，则在插入时需特判：如果栈顶和待插入元素均为`^`，则直接插入。如果匹配方式为 `2^3^3` $\to$ `(2^3)^3` ，则无需特判。  

板子，虽然很长，但是~~不难写~~：  
```cpp
#include<bits/stdc++.h>
using namespace std;
int greaters(char c){// 使用switch判断优先级 
	switch(c){
		case '+':return 1;
		case '-':return 1;
		case '*':return 2;
		case '/':return 2;
		case '^':return 3;
		case '(':return -1;
		default:return -0xffffff;
	}
}
string s,bformat=""; //"bformat" 变量为后缀表达式所存放的字符串，可在放入时中间加一个空格，方便以后
int getnumber(int i){//取数,快读模板
	int x=0,f=1;
	if(s[i]=='-'){f=-1;i++;}
	while(s[i]>='0' && s[i]<='9'){
		x=(x<<3)+(x<<1)+(s[i]^48);
		i++;
	}
	return x*f;
}
int main(){
	cin>>s;
	stack<char> st;
	for(int i=0 ; i<s.size() ; i++){//规则#1:从前往后遍历 
	
		//规则#2:遇到数直接插入,对于负号情况,特殊判断
		if((s[i]>='0' && s[i]<='9') //是一个非负数，直接读入
        || (s[i]=='-' //对是负数还是减号的情况特判
        && (i==0 //如果第一个字符为 - ，则一定是负数
        ||(s[i+1]>='0' && s[i+1]<='9'&& !(s[i-1]>='0' && s[i-1]<='9') ) ) ) ){//否则看看后面和前面的字符是不是数字来判断
			bformat = bformat + ' ' + to_string(getnumber(i));
			if(s[i]=='-')i++;
			while(s[i]>='0' && s[i]<='9'){
				i++;
			}
			i--;
		}//当然如果输入时每个运算符之间加上空格就不用这么麻烦了~
		
		else{
			if(!st.empty() && s[i]=='^' && st.top()=='^')st.push('^');
			//对于规则#7的特判
			else if(s[i]=='(')st.push('(');
			// 规则#5
			else if(s[i]==')'){// 规则#5 
				while(!st.empty() && st.top()!='('){//不要忘了判空，不然会RE！下同 
					bformat = bformat + ' ' + st.top();
					st.pop();
				}
				st.pop();//记得把那一个 ( 也弹出
			}
			else if(!st.empty() && greaters(s[i]) > greaters(st.top()) )st.push(s[i]);
			// 规则#3
			else if(!st.empty() && greaters(s[i]) <= greaters(st.top()) ){// 规则#4
				while(!st.empty() && greaters(s[i]) <= greaters(st.top())){ 
					bformat = bformat + ' ' + st.top();
					st.pop();
				}
				st.push(s[i]);
			}
			else if(st.empty())st.push(s[i]);//没有东西直接压入即可
		}
	}
	while(!st.empty()){// 规则#6
		bformat = bformat + ' ' + st.top();
		st.pop();
	}
	bformat.erase(0,1);
	cout<<bformat;
	return 0;
}
```  
#### 小技巧  
人类可没有计算机的运算速度和准确度，那作为一个人该如何快速进行表达式的转换呢？  
举个例子：  
$$1 \times 2 + 3 \div 6 \bmod (1\times 114)$$  
我们首先根据**四则运算的法则**给这个式子添上括号，像这样：  
$$((1 \times 2) + ((3 \div 6) \bmod (1\times 114)))$$  
之后我们将每一个运算符移动到它所代表的运算顺序的 `)`这个**右括号**的**右边**。  

没听懂？再举个例子：  
对于 `mod` 运算，它所代表的运算顺序的 `(` `)` 两个左右括号是下面标红的两个括号：  
$$((1 \times 2) + {\color{Red}(} (3 \div 6) \bmod (1\times 114){\color{Red})}$$  
这时我们需要将它移动到它所代表的运算顺序的 `)` 这个右括号的右边，像这样：  
$$((1 \times 2) + {\color{Red}(} (3 \div 6) (1\times 114){\color{Red})} \bmod$$  
对所有运算符都这么干，于是原式变成了：  
$$((1 \ 2) \times ( (3 \ 6) \div (1 \ 114) \times ) \bmod +$$  
去掉括号:  
$$1 \ \ 2 \ \times \ 3 \ \ 6 \ \div \ 1 \ \ 114 \ \times \ \bmod \ +$$
转换完成啦！  
熟练了以后还是很快的。  
#### 1.2 中缀转前缀  
规则如下：  
1. **从后往前**遍历原中缀表达式。  
2. 遇到数字，直接进入前缀表达式。
3. 遇到字符，如果**严格小于**栈顶字符的优先级，则直接压入栈。  
4. 遇到字符，如果**大于或等于**栈顶字符的优先级，则弹出栈顶字符并直接进入前缀表达式，**直至此元素严格小于栈顶元素优先级或栈空**，此时将此元素压入栈。  
5. 如遇 `)` ，直接压入队列。如遇 `(`，弹出栈顶字符并直接进入前缀表达式，直至遇到第一个 `)` 。
6. 遍历完后，将栈中所有元素弹出并直接进入前缀表达式，直到栈空。 
7. 对于 `^` 符号的特判。  
8. **将生成好的前缀表达式倒序输出，即为正确的前缀表达式。**   
 
板子，默认为每个操作数分开读入，免去一些特判：  
```cpp  
#include<bits/stdc++.h>
using namespace std;
string fformat[10000];int ft=1;
stack<char> oper;
int greaters(char s){//判断优先级 
	switch(s){
		case '+': { return 1; }
		case '-': { return 1; }
		case '*': { return 2; }
		case '/': { return 2; }
		case '^': { return 3; }
		case ')': { return 0; }
	}
}
int main(){
	int n;cin>>n;
	string s[10000];
	for(int i=1;i<=n;i++){cin>>s[i];}
	for(int i=n;i>=1;i--){//一定是倒着处理! 
		if(s[i].size()==1 && ( s[i][0]<'0' || s[i][0]>'9')){//是操作字符 
			if(oper.empty())oper.push(s[i][0]);
			//空了直接插入，写在头上还可免去下面 if 里的特判
			else if(s[i][0]== ')')oper.push(')');
			//右括号，直接插入
			else if(greaters(s[i][0]) >= greaters(oper.top()))oper.push(s[i][0]);
			//优先级大于或等于，直接插入
			else if(s[i][0]== '(' ){//是左括号，弹出栈顶至第一个右括号 
				while(!oper.empty() && oper.top() != ')'){//一定要判栈空，不然会 RE 
					string b(1,oper.top());//使用 string 的构造函数来转换字符到字符串 
					fformat[ft++]=b;
					oper.pop();
				}
				oper.pop();
			}
			else if(greaters(s[i][0]) < greaters(oper.top())){//严格小于，弹出栈顶直到大于等于 
				while(!oper.empty() && greaters(s[i][0]) < greaters(oper.top())){//一定要判栈空，不然会 RE
					string b(1,oper.top());
					fformat[ft++]=b;
					oper.pop();
				}
				oper.push(s[i][0]);
			}
		}
		else fformat[ft++]=s[i];//数字直接插入
	}
	while( !oper.empty() ){//剩下的全都弹出 
		string b(1,oper.top());
		fformat[ft++]=b;
		oper.pop();
	}
	for(int i=ft;i>=1;i--)cout<<fformat[i]<<' ';
	//一定不要忘了倒序输出!!!!!
	return 0;
}
```    
快速转换前缀表达式的技巧与前文后缀表达式介绍的方法基本相同，只不过要将运算符**往前移**而非往后。
### 2. 后，前缀表达式的计算  
#### 2.1 后缀表达式的计算  
规则如下：  
1. 从左往右遍历整个后缀表达式。  
2. 遇到数字时，直接压入一个栈。  
3. 遇到运算字符，取出栈顶两个数，经过运算变成一个数后再次压入栈。注意减法、除法和乘方等的运算顺序。  
4. 遍历完成后，输出栈顶数字即可。  
  
板子，[洛谷P1449](https://www.luogu.com.cn/problem/P1449)。这份代码中并没有实现乘方运算。  
```cpp  
#include<bits/stdc++.h>
using namespace std;
stack<int> ss;
int main(){
	char c;
	int opern=0; 
	while(cin>>c){
		if(c=='@')break;	
		else if(c>='0'&&c<='9'){opern=(opern<<3)+(opern<<1)+(c^48);}//读入操作数，快读的模板 
		else if(c=='.'){ss.push(opern);opern=0;}//读完后直接压入栈 
		
		switch(c){//分类每种字符 
			case '+':{
				int op1=ss.top();ss.pop();int op2=ss.top();ss.pop();//提取操作数 
				ss.push(op1+op2);//操作完成后压回栈，下同 
				break;
			}
			case '-':{
				int op1=ss.top();ss.pop();int op2=ss.top();ss.pop();
				ss.push(op2-op1);//注意顺序 
				break;
			}
			case '*':{
				int op1=ss.top();ss.pop();int op2=ss.top();ss.pop();
				ss.push(op1*op2);
				break;
			}
			case '/':{
				int op1=ss.top();ss.pop();int op2=ss.top();ss.pop();
				ss.push(op2/op1);//注意顺序
				break;
			}
			default:break;
		}
	}
	cout<<ss.top();//直接输出栈顶即可 
	
	return 0;
} 
```  
#### 2.2 前缀表达式的计算   
规则如下：  
1. **从右往左**遍历整个后缀表达式。  
2. 遇到数字时，直接压入一个栈。  
3. 遇到运算字符，取出栈顶两个数，经过运算变成一个数后再次压入栈。注意减法、除法和乘方等的运算顺序。  
4. 遍历完成后，输出栈顶数字即可。 

除遍历顺序外与后缀表达式的计算并无不同，这里不出示代码。  
### 3. 后，前缀表达式转中缀表达式  
#### 3.1 后缀表达式转中缀表达式  
思路很简单，我们在计算中只合并操作数和字符而不计算，合并时加上括号即可。  
板子，从[洛谷P1449](https://www.luogu.com.cn/problem/P1449)稍作更改而来。  
```cpp  
#include<bits/stdc++.h>
using namespace std;
stack<string> ss;
int main(){
	char c;
	int opern=0; 
	while(cin>>c){
		if(c=='@')break;	
		else if(c>='0'&&c<='9'){opern=(opern<<3)+(opern<<1)+(c^48);}//读入操作数，快读的模板 
		else if(c=='.'){ss.push(to_string(opern));opern=0;}//读完后直接以字符串压入栈 
		
		switch(c){//分类每种字符 
			case '+':{
				string op1=ss.top();ss.pop();string op2=ss.top();ss.pop();//提取操作的数或整式 
				string ctc="( "+op2+" "+'+'+" "+op1+" )";//拼成一个新的整式，压回栈中，下同 
				ss.push(ctc);
				break;
			}
			case '-':{
				string op1=ss.top();ss.pop();string op2=ss.top();ss.pop();
				string ctc="( "+op2+" "+'-'+" "+op1+" )";//注意顺序 
				ss.push(ctc);
				break;
			}
			case '*':{
				string op1=ss.top();ss.pop();string op2=ss.top();ss.pop();
				string ctc="( "+op2+" "+'*'+" "+op1+" )"; 
				ss.push(ctc);
				break;
			}
			case '/':{
				string op1=ss.top();ss.pop();string op2=ss.top();ss.pop();
				string ctc="( "+op2+" "+'/'+" "+op1+" )";//注意顺序
				ss.push(ctc);
				break;
			}
			default:break;
		}
	}
	cout<<ss.top();//直接输出栈顶即可 
	
	return 0;
} 
```  
#### 3.2 前缀表达式转中缀表达式   
除遍历顺序外与后缀表达式的转换并无不同，这里不出示代码。

----  
迁移自[云剪贴板](https://www.luogu.com.cn/paste/jqk2vtmq)

