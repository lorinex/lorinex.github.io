# **软件工具** **LEX**

### 一、LEX使用流程

![image-20230926150023284|300](file://C:/Users/xxx/Desktop/%E8%AF%8D%E6%B3%95%E5%88%86%E6%9E%90%E7%A8%8B%E5%BA%8F.assets/image-20230926150023284.png?lastModify=1695712117)

### 二、LEX源程序结构

```
说明部分  
%%  
翻译规则  
%%  
辅助过程
```

#### 1.说明部分

- 包括：变量的声明、符号常量的声明、正规定义 
- 正规定义中的名字可在翻译规则中用作正规表达式的成分 
- C语言的说明必须用分界符“ %{ ”和“%} ”括起来。 
	- 出现在括号中的任何东西都直接抄写到词法分析器Lex.yy.c中，不作为正规定义和翻译规则的一部分。在第三节中的辅助过程也按同样方式处理  
```LEX
%{ 
#include <stdio.h>
#include <stdlib.h> 
#include <string.h> 
#include <ctype.h> 
#include ″y.tab.h″ 
typedef char * YYSTYPE; 
char * yylval; 
%}
```

#### 2.翻译规则

形式： 
```LEX
P1 { 动作1 } 
P2 { 动作2 } 
… 
Pn { 动作n }
```

Pi 是一个正规表达式，描述一种记号的模式。

动作i是C语言的程序语句，表示当一个串匹配模式Pi时，词法分析器应执行的动作。

**$P_i$书写中可能用到的规则：**
(1) 转义字符：`" \ [ ] ^ - ? . * + | ( ) $ / { } % < >`
具有特殊含义，不能用来匹配自身。如果需要匹配的话，可以通过引号`"`或者转义符号`\`来指示。比如：`C"++"`和`C\+\+`都可以匹配C++。 
(2) 通配符：`.`可以匹配任何一个字符。 
如：`a.c`匹配任何以a开头、以c结尾的长度为3的字符串。 
(3) 字符集：用方括号`[`和`]`指定的字符构成一个字符集。
如，`[abc]`表示一个字符集，可以匹配a、b或c中的任意一个字符。 
使用`–`可以指定范围。比如：`[A-Za-z]`。 
(4) 重复：
`*`表示任意次重复（可以是零次），
`+`表示至少一次的重复，
`?`表示零次或者一次。 
如：`a+`相当于`aa*`，`a*`相当于`a+|ε`，`a?`相当于`a|ε`。
(5) 选择和分组：`|`表示二者则一；括号`(`和`)`表示分组，括号内的组合被看作是一个原子。 
如：`x(ab|cd)y` 匹配`xaby`或者`xcdy`。

#### 3.辅助过程

- 对翻译规则的补充
- 翻译规则部分中某些动作需要调用的过程，如果不是C语言的库函数，则要在此给出具体的定义。
- 这些过程也可以存入另外的程序文件中，单独编译，然后和词法分析器连接装配在一起。

#### LEX源程序举例

传递单词的属性，是把属性值赋给全程变量yylval
正规定义式： 
```
if -> if 
then -> then 
else -> else 
relop -> < | <= | = | <> | > | >= 
id -> letter(letter|digit)* 
num -> digit+(.digit+)?(E(+|-)?digit+)?
```

相应的 LEX 源程序 框架:
```c
/* 声明部分 */ 
%{ 
#include <stdio.h> 
┇ /* C语言描述的符号常量的定义，如LT、LE、EQ、NE、GT、 GE、IF、THEN、ELSE、ID、NUMBER、RELOP */ 
extern yylval, yytext, yyleng; 
%} 

/* 正规定义式 */ 
delim [ \t\n] 
ws {delim}+ 
letter [A-Z a-z] 
digit [0-9] 
id {letter}({letter}|{digit})_ 
num {digit}+(.{digit}+)?(E[+-]?{digit}+)? 
%%
 
/* 规则部分 */ 
 {ws}  {/* 没有动作，也不返回 */} 
 if    { return(IF); } 
 then  { return(THEN); } 
 else  { return(ELSE); } 
 {id}  { yylval=install_id(); return(ID); } 
 {num} { yylval= num_val(); return(NUMBER); } 
 "<"   { yylval=LT; return(RELOP); } 
 "<="  { yylval=LE; return(RELOP); } 
 "="   { yylval=EQ; return(RELOP); } 
 "<>"  { yylval=NE; return(RELOP); } 
 ">"   { yylval=GT; return(RELOP); } 
 ">="  { yylval=GE; return(RELOP); } 
%%
 
/* 辅助过程 */ 
int install_id() { 
┇ /* 把单词插入符号表并返回该单词在符号表中的位置 
	yytext指向该单词的第一个字符 
	yyleng给出它的长度 */ 
} 
int num_val() { 
┇ /* 将识别出的无符号数字符串转换成数值型返回。*/ 
}
```

LEX解决冲突的方式:
1. 根据规则定义的先后次序 解决了例子中关键字和标识符的冲突
2. 最长匹配原则 解决了例子中诸如 “<” 和 “<=” 的冲突

### 三、LEX的工作原理

#### 1.工作过程

扫描每一条翻译规则Pi，为之构造一个非确定的有限自动机NFA Mi 将各条翻译规则对应的NFA Mi合并为一个新的NFA M

![image-20230926150712087](file://C:/Users/xxx/Desktop/%E8%AF%8D%E6%B3%95%E5%88%86%E6%9E%90%E7%A8%8B%E5%BA%8F.assets/image-20230926150712087.png?lastModify=1695712117)

将NFA M确定化为DFA D，并生成该DFA D的状态转换矩阵和控制执行程序。

#### 2.识别单词时的二义性处理

最长匹配原则 在识别单词符号过程中，当有几个规则看来都适用时，则实施最长匹配的那个规则 优先匹配原则 如有几条规则可以同时匹配一字符串，并且匹配的长度相同，则实施最上面的规则。

如果有一些内容不匹配任何规则，则LEX将会把它拷贝到标准输出

#### 3.工作过程举例

```
%%
a           {  }
abb         {  }
{a}*b{b}*   {  }
%%
```

![截图_20230926153011.png|400](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20230926153011.png)
![截图_20230926204247.png|400](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20230926204247.png)
![截图_20230926204400.png|400](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20230926204400.png)
![截图_20230926204429.png|400](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20230926204429.png)
![截图_20230926204456.png|400](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/%E6%88%AA%E5%9B%BE_20230926204456.png)
