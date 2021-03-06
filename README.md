# C-Primer-Plus
---
[ToC]

### 第5章 运算符、表达式和语句

------

多重赋值（其他语言会回避）
a = b = c = 9  赋值顺序从右向左（右结合）

一元运算符  "+、-"  正负号
二元运算符  "+、-、*、/"  ，需要两个运算对象

整数除法截断小数部分，非四舍五入。

typedef机制  typedef double real → real num → num（double）
sizeof（运算符）
size_t（数据类型）：unsigned int、unsigned long

n++  先使用，再递增    ++n  先递增，在适用
ans = num/2 + 5*(1 + num++);无法保证num/2先运行，还是num++先运行，避免此类使用。

指令：由至少一条有用的语句构成。（a = b ;）、(a = b + （c = 5）;)

类型降级：char = 1107 → char = （1107%256）

Parameter形参、argument实参
形参是变量，实参是函数调用提供的值，实参被赋给相应的形参。
缺少函数原型 → 自动升级 低→高

> `#define format "%s string!\n"`
> `printf(format,format);`
> `%s string! `
> `string!`
> 字符串的原样替换

------



### 第6章 C控制语句：循环

------

scanf（）返回值  
①反映按照指定的格式符正确读入的数据的个数。（失败为0，具体因编译器而别）
②转换值之前出了问题（检测到结尾或硬件问题），返回特殊值EOF（通常为-1）

伪代码（pseudocode） 文字叙述程序结构流程
迭代（iteration）

浮点数比较时，尽量只使用"<" ">",浮点数舍入误差会导致在逻辑上应该相等的两数不相等。
3*（1/3）= ? ,浮点运算时实际成积为 .999999 

C中，表达式一定有一个值，关系表达式也不例外。
一般而言，所有的非零值都视为真，只有0被视为假。.

算数运算符 关系运算符 赋值运算符
while (status = 1) = while (status) = while（1） 
while（status == 1）
常量放左侧
技巧：如果带比较值为一个常量，可将常量置于左侧便于编译器捕获错误。（5 == canoes）

for（A,B;C;D,E）
第一个表达式执行一次，也可使用printf（）
逗号是一个序列点，保证运算顺序由左往右。
x = (y = 3,z = y + 3) → x = 6
整个逗号表达式的值是右侧项的值。

组合赋值运算符优先级 = 普通赋值运算符优先级，低于算术运算符。

Array [ element ] 数组由相邻位置的内存位置组成，只储存相同类型的数据。编号从0开始。

字符数组 + 空字符 → 字符串（字符数组）

modularity 模块化

------



### 第7章 C控制语句：分支和跳转

------

#### 语句

<u>整个if语句仍被视为一条语句（花括号内）</u>  

> `if (score > big)`
> printf("Jackpot!\n");`  //简单语句`

> `if (joe > ron) {`
> `joecash++;`
> printf("You lose, Ron.\n");`
> }`// 复合语句

#### getchar()

函数不带任何参数，从输入队列中返回下一个字符

> `ch = getchar();` 
> `scanf("%c",&ch);`

#### putchar()

函数打印它的参数

> `putchar(ch);` 
> `printf("%c",ch);`

#### getchar（）& putchar（）

不是真正的函数，它们被定义为供预处理器使用的宏。

#### #define

可视为具体的等价

> `#define SPACE ' '` //单引号+空格+单引号

#### C特有编程风格 宽松的格式要求

把两个行为合并成一个表达式，合理使用圆括号组合表达式。  

> `while((ch = getchar()) != '\n')`  

#### “ != ”优先级高于“ = ”

“ ！” 优先级仅次于圆括号 = 递增运算符；关系运算符 > 与 > 或

> `while(ch = getchar() != '\n')` 
> '!='表示关系，故(getchar() != '\n')值不是1就是0 
> while ((c = getchar()) != ' ' && c != '\n')

#### 字符实际上作为整数储存

> putchar(char→int)

#### ctype.h头文件

> 一系列专门处理字符的函数，这些函数接受一个字符作为参数，如果类型符合，就返回一个非零值（真），否则返回0（假）。
> isalpha()  //字母　　　　　　tolower()  //大写返回小写，其他不变  
> isalnum()  //字母数字　　　　toupper()  //小写返回大写，其他不变  
> isblank()  //标准空白字符　　**. . .**

#### if、else匹配规律

else与离它最近的if匹配，除非最近的if被花括号括起来。（忽略缩进）

#### if(90<=range<=100)

<=运算符求值顺序从左往右，故解释为：(90<=range)<=100

#### 序列点：&&、||

> 'while(  (c = getchar()) != ' ' && c != '\n'  )'
>
> //逻辑表达式从左往右求值，发现某元素为无效时，停止求值

#### 空白 定义

空格' '、制表符'\t'、换行符'\n'

#### 三元运算符

> `max = (a>b)?a:b;`

#### switch & if else （switch中无break跳出，将顺序执行）

1. 浮点型变量或表达式、变量在某范围内决定程序流的去向，更适用`if`，虽然使用`switch`通常代码更少、运行更快。

2. switch可以以“字符”判断，即以字符对应的整数

   > `switch(ch) {`
   >
   > ​	case 'a':  ...  ;
   >
   > ​	default： ... ；
   >
   > `}`

3. `case`决定了入口，没有`break`将会继续执行剩余`case`

#### goto 语句包括：goto + 标签名：标签语句（允许描述性单词）

> `goto part2;` + `part2:printf("Hello world!\n");`

可用于<u>跳出嵌套循环</u>（break只能跳出当前循环），过度使用会让程序错综复杂。

#### continue

提高单独分号语句可读性`continue;`，执行后，跳出本次循环剩余内容。

#### break & continue （goto特殊形式，不使用标签）

continue继续下次循环，break跳出当前（层）循环。

#### 条件的判断和赋值

> `if(flag = 10)` `if(flag == 10)`
>
> // 赋值运算符 关系运算符

#### 制表符 \t

可能是四个字符，补齐字符（如果原来就是整数倍，是不是等于没变化？）

------



### 第8章 字符输入/输出和输入验证

------

如果用一个特殊字符（如，#）来结束输入，就无法在文本中使用这个字符，是否有更好的方法结束输入？

#### 无缓存（直接）输入

回显用户输入+立即重复打印该字符，即正在等待程序可立即使用输入的字符。

#### 缓存输入（缓冲区buffer中输入可修正，块传输）

Enter键之前不会重复打印刚输入的字符，Enter键之后程序才可使用输入字符。

- ##### 完全缓冲I/O	:buffer填满后刷新（内容被发送至目的地），文件输入

  ##### 行缓冲I/O	:出现换行符时刷新，键盘输入

#### conio.h

- getche()	回显无缓冲输入
  getch()	无回显无缓冲输入
