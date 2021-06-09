
目    录
一．需求说明	3
1．文法说明	3
2．目标代码说明	9
二．详细设计	9
1．程序结构	9
2．类/方法/函数功能	9
3．调用依赖关系	10
4．符号表管理方案	11
5．存储分配方案	11
6. PCODE设计	12
7. 解释执行程序	13
8. 出错处理	17
三．操作说明	18
1．运行环境	18
2．操作步骤	18
四．测试报告	19
1．测试程序及测试结果	19
2．测试结果分析	20
五．总结感想	20

 


一．需求说明
1．文法说明
1、＜加法运算符＞ ::= +｜-
作用：用于加法运算
限定条件：
例句：c＝a+b c＝a-b

2、＜乘法运算符＞  ::= *｜/
作用：用于乘法运算
限定条件：
例句：c＝a*b  c＝a/b

3、＜关系运算符＞  ::=  <｜<=｜>｜>=｜!=｜==
作用：用于关系比较运算 
限定条件：
例句：if（a<b）

4、＜字母＞   ::= ＿｜a｜．．．｜z｜A｜．．．｜Z
作用：英文字母 
限定条件：区分大小写
例句：int a；

5、＜数字＞   ::= ０｜＜非零数字＞
作用：数字字符 
限定条件：
例句：a＝0； b＝1；

6、＜非零数字＞  ::= １｜．．．｜９
作用：非零数字字符 
限定条件：
例句：a＝2；

7、＜字符＞    ::=  '＜加法运算符＞'｜'＜乘法运算符＞'｜'＜字母＞'｜'＜数字＞'
作用：单个字符 
限定条件：
例句：int a;

8、＜字符串＞   ::=  "｛十进定编码为32,33,35-126的ASCII字符｝"
作用：字符串构成 
限定条件：
例句：“c language”



9、＜程序＞::= ［＜常量说明＞］［＜变量说明＞］{＜有返回值函数定义＞|＜无返回值函数定义＞}＜主函数＞
作用：程序的定义 
限定条件：见注释
例句：const int a=1;//常量声明，要么没有，要么出现一次
int b;//变量声明，要么没有，要么出现一次
int add(int a,int b){
return a+b;
	}//有返回值函数定义，可以没有
	void print(int n){
		printf(“%d”,n);
	}//无返回值函数定义，可以没有
	void main(){
		b = 0;
		b = add(a,b);
		print(b);
	}//主函数

10、＜常量说明＞ ::=  const＜常量定义＞;{ const＜常量定义＞;}
作用：常量说明
限定条件：可连续说明多个常量
例句：const int a；

11、＜常量定义＞   ::=   int＜标识符＞＝＜整数＞{,＜标识符＞＝＜整数＞}
                                    | float＜标识符＞＝＜实数＞{,＜标识符＞＝＜实数＞}
                                    | char＜标识符＞＝＜字符＞{,＜标识符＞＝＜字符＞}
作用：常量定义 
限定条件：
例句：int a ＝ 9； float m ＝ 0.1；

12、＜整数＞        ::= ［＋｜－］＜无符号整数＞｜０
作用：整数定义 
限定条件：正号可省略，负号不能省略
例句：1236540  -3214564879413

13、＜小数部分＞    ::= ＜数字＞｛＜数字＞｝｜＜空＞
作用：实数的小数部分 
限定条件：
例句：013156461681

14、＜实数＞        ::= ［＋｜－］＜整数＞[.＜小数部分＞]
作用：实数 
限定条件：可以没有小数部分，没有小数部分的不能有小数点
例句：123465486516   13241564.1321503

15、＜标识符＞    ::=  ＜字母＞｛＜字母＞｜＜数字＞｝
作用：标识符定义 
限定条件：必须以字母开头，后面可以接任意数量的字母或数字
例句：char printf
16、＜声明头部＞   ::=  int＜标识符＞ |float ＜标识符＞|char＜标识符＞
作用：声明的头部 
限定条件：
例句：int abs  float fl

17、＜变量说明＞  ::= ＜变量定义＞;{＜变量定义＞;}
作用：变量声明格式 
限定条件：可连续声明多个变量
例句：int i；

18、＜变量定义＞  ::= ＜类型标识符＞(＜标识符＞|＜标识符＞‘[’＜无符号整数＞‘]’){,(＜标识符＞|＜标识符＞‘[’＜无符号整数＞‘]’ )}
作用：变量定义格式 
限定条件：定义单个变量，或数组变量。可连续定义多个变量。
例句：int a，b[2]；

19、＜可枚举常量＞   ::=  ＜整数＞|＜字符＞
作用：可用于枚举的常量类型 
限定条件：只能是单个字符或数字
例句：a 0

20、＜类型标识符＞      ::=  int | float | char
作用：文法允许的变量类型 
限定条件：只支持int float char三种类型
例句：int a; char ch;

21、＜有返回值函数定义＞  ::=  ＜声明头部＞‘(’＜参数＞‘)’ ‘{’＜复合语句＞’}’
作用：有返回值的函数定义格式 
限定条件：
例句：int add（int a，int b）｛
		return a+b；
	｝

22、＜无返回值函数定义＞  ::= void＜标识符＞’(’＜参数＞‘)’‘{’＜复合语句＞‘}’
作用：无返回值的函数定义格式 
限定条件：
例句：void print（int a）｛
		printf（“%d”，a）；
	｝

23、＜复合语句＞   ::=  ［＜常量说明＞］［＜变量说明＞］＜语句列＞
作用：用于函数体的复合语句 
限定条件：常量和变量说明都可选。
例句：const int a;
	int b,c;
	c = a+b;

24、＜参数＞    ::= ＜参数表＞
25、＜参数表＞    ::=  ＜类型标识符＞＜标识符＞{,＜类型标识符＞＜标识符＞}| ＜空＞
作用：参数格式 
限定条件：参数可以有0个，1个或多个
例句：int a，char b;

26、＜主函数＞    ::= void main’(’‘)’‘{’＜复合语句＞‘}’
作用：主函数定义格式 
限定条件：类似于其他函数，只是函数名限定为main，且返回值必须为void
例句：void main（）｛
		int a；
	｝

27、＜表达式＞    ::= ［＋｜－］＜项＞{＜加法运算符＞＜项＞}
作用：表达式定义格式 
限定条件：至少有一项
例句：3 + 5 + （a+b）* add（a，b）；

28、＜项＞     ::= ＜因子＞{＜乘法运算符＞＜因子＞}
作用：项定义格式 
限定条件：至少有一个因子
例句：（a+b）*add（a，b）

29、＜因子＞    ::= ＜标识符＞｜＜标识符＞‘[’＜表达式＞‘]’|‘(’＜表达式＞‘)’｜＜整数＞|＜实数＞|＜字符＞｜＜有返回值函数调用语句＞ 
作用：因子定义格式 
限定条件：因子可以是变量，数组，表达式，整数，实数，字符，有返回值函数调用等。
例句：（a+b） add（a，b）        

30、＜语句＞    ::= ＜条件语句＞｜＜循环语句＞| ‘{’＜语句列＞‘}’｜＜有返回值函数调用语句＞;
 |＜无返回值函数调用语句＞;｜＜赋值语句＞;｜＜读语句＞;｜＜写语句＞;｜＜空＞;|＜情况语句＞｜＜返回语句＞;
作用：语句定义格式 
限定条件：一般情况下语句后面必须有；
例句：if（m!=n)  while（t＝＝0）；

31、＜赋值语句＞   ::=  ＜标识符＞＝＜表达式＞|＜标识符＞’[’＜表达式＞‘]’=＜表达式＞
作用：赋值语句 
限定条件：被赋值可以是变量，表达式，或者数组中某一值
例句：a＝9； s＝a[0]；

32、＜条件语句＞  ::=  if ‘(’＜条件＞’)’＜语句＞
作用：条件语句定义格式 
限定条件：条件为真才执行语句，且语句不能为空。
例句：if（a＝＝b）

33、＜条件＞    ::=  ＜表达式＞＜关系运算符＞＜表达式＞｜＜表达式＞ //表达式为0条件为假，否则为真
作用：用于条件语句和循环语句的条件 
限定条件：值为0条件为假，值为1条件为真
例句：a＝＝b  a>b

34、＜循环语句＞   ::=  while ‘(’＜条件＞’)’＜语句＞
作用：循环语句定义格式 
限定条件：仅支持while循环，条件为真则循环执行语句
例句：while（a＝＝b） count＝ count+1；

35、＜情况语句＞  ::=  switch ‘(’＜表达式＞‘)’ ‘{’＜情况表＞＜缺省＞ ‘}’
36、＜情况表＞   ::=  ＜情况子语句＞{＜情况子语句＞}
37、＜情况子语句＞  ::=  case＜可枚举常量＞：＜语句＞
38、＜缺省＞   ::=  default : ＜语句＞|＜空＞
作用：情况选择语句 
限定条件：至少有一个case语句，可以没有缺省语句。只能匹配可枚举常量
例句：switch（a）｛
		case ‘a’：b＝0；
		case ‘b’：b＝1；
		default： b＝2；
	｝

39、＜有返回值函数调用语句＞ ::= ＜标识符＞‘(’＜值参数表＞‘)’
40、＜无返回值函数调用语句＞ ::= ＜标识符＞’(’＜值参数表＞‘)’
作用：函数调用语句 
限定条件：两种函数的调用语句一样，但有返回值的函数的调用语句可作为表达式使用
例句：a ＝ add（b，c）；

41、＜值参数表＞   ::= ＜表达式＞{,＜表达式＞}｜＜空＞
作用：函数所需参数列表 
限定条件：可以没有参数
例句：add（b，c）；

42、＜语句列＞   ::= ｛＜语句＞｝//这一个没什么好说的

43、＜读语句＞    ::=  scanf ‘(’＜标识符＞{,＜标识符＞}’)’
作用：读入语句格式 
限定条件：直接读入，不用格式化。
例句：scanf（a，b）；

44、＜写语句＞    ::= printf ‘(’ ＜字符串＞,＜表达式＞ ‘)’| printf ‘(’＜字符串＞ ‘)’| printf ‘(’＜表达式＞’)’
作用：写语句格式 
限定条件：可输出字符串或表达式
例句：printf（“adfasdf”，a+b）；

45、＜返回语句＞   ::=  return[‘(’＜表达式＞’)’]
作用：返回语句格式 
限定条件：可选择地返回一个表达式
例句：return； return a+b；

46、＜无符号整数＞  ::= ＜非零数字＞｛＜数字＞｝
作用：无符号整数格式
限定条件：非零数字后跟任意数量的数字
例句：12901491489

附加说明：
（1）char类型的表达式，用字符的ASCII码对应的整数参加运算，在写语句中输出字符
（2）标识符区分大小写字母
（3）写语句中的字符串原样输出
（4）情况语句中，switch后面的表达式和case后面的常量只允许出现int和char类型；每个情况子语句执行完毕后，不继续执行后面的情况子语句
（5）数组的下标从0开始
（6）三种基本类型int char float可以相互转化，char类型以ASCII码进行运算
（7）在表达式中，多个加法符号按语义处理，即奇数个负号为负，偶数个负数为正，不当作错误处理
2．目标代码说明
生成PCODE虚拟机的代码，并在解释执行程序中解释执行。
二．详细设计
1．程序结构
 
2．类/方法/函数功能
1）函数功能说明
void base();解释执行程序
void readch();读一个字符
void getsym();词法分析程序
int turn_to_int(string s);将单词转换为整形值
float turn_to_float(string s);将单词转换为浮点数值
void _getsym();返回一个单词，加入了缓冲区
void getsym();词法分析程序
void enter(_type t);符号表登陆
void gen(__pcode p, float a, int l);目标代码生成
void constdeclaration();常量声明
void constdefinition();常量定义
void vardeclaration();变量声明
void vardefinition();变量定义
void program();程序
void mainfunc(int p);main函数
void paradeclaration();参数声明
void funcdeclaration(int p);函数声明
void parameter();参数传递
void func();函数调用
void combinestatement(int p);复合语句
int expression();表达式
int poly();项
int factor();因子
void statement();语句
void ifstatement();if语句
void condition();条件
void whilestatement();while语句
void casestatement();case语句
void defaultstatement();default语句
void switchstatement();switch语句
void statements();语句列
void scanfstatement();scanf语句
void printfstatement();printf语句
void returnstatement();return语句
void assignstatement();赋值语句
int number();常数处理
int checktable();查符号表
void block();语法分析程序
void err(int err_type);错误处理
2）关键算法
对每一个非终结符编写分析程序，采用递归子程序法自顶向下分析。同时采用一遍式，在语法分析的过程中调用此法分析取词。
3．调用依赖关系
 
说明：由于大部分子程序都调用了gen和err，所以在调用关系图中省略了。菱形的图是因为调用关系太多连不下延伸出来的。图需要放大之后才能看清。
4．符号表管理方案
struct sign{
    string name;
    _type type;
    int intNum;
    float floatNum;
    char ch;
    int level;
    int top;
    int button;
    int offset;
    int entrence;
    int length;
    int paraNum;
};符号表是这个结构体的数组。

说明：
name：符号名
_type：符号类型，包括（consti,constf,constc,vari,varf,varc,funci,funcf,funcc,funcv,arrayi,arrayf,arrayc,parai,paraf,parac）九种
intNum：整形常量值
floatNum：浮点常量值
ch：字符常量值
level：层数
top：数组长度
offset：偏移
entrence：函数入口
letch：函数长度
pataNum：函数参数个数

管理算法：
a 登陆符号表前先检查是否有重名，判断重名条件：符号名相同且层数一样或者与函数重名
b 每一个子函数编译完成之后，将符号表中子函数的常量，变量，参数等项目删除
c 查询符号表时，若当前层有该符号则返回当前层符号索引，若没有则检查上一层是否有。若两层都没有则查询符号表失败，符号未定义。




5．存储分配方案
a 运行栈是float的数组。
b 程序开始为全局常量和变量分配空间
c 每个子过程调用时多分配三个空间，分别用于存储返回值、返回地址、上层基地址。


6. PCODE设计
PCODE 总体格式：指令名  a  l
详细说明：
LIT：将一个常量取到栈顶
a：常量值
l：常量类型（int。float，char）

LOD：将栈内某个变量取到栈顶
a：变量偏移
l：变量层数

LOA：将数组中某个值取到栈顶
a：数组首地址偏移
l：数组层数

STO：将栈顶值存入栈内某一变量
a：变量偏移
l：变量层数

STA：将栈顶值存入数组
a：数组首地址偏移
l：数组层数

CAL：函数调用
a：函数位置，即函数第一条代码在整个代码中的位置

RET：函数返回

INIT：在栈顶开辟空间
a：开辟空间的个数

JMP：无条件跳转
a：跳转地址

JPC：条件跳转
a：跳转地址

OPR：算数运算和逻辑运算
a：运算类型

REA：从控制台输入，读到栈顶
a：读入类型

WRI：向控制台输出栈顶内容
a：输出类型
7. 解释执行程序
	所用辅助变量：
	i：程序计数器
point：栈顶指针
en_0：全局基址
en_1：当前层基址
tmpen：临时基址
_stack[2000]：运行栈

届时执行过程：
LIT：将一个常量取到栈顶
a：常量值
l：常量类型（int。float，char）
_stack[point++] = a;

LOD：将栈内某个变量取到栈顶
a：变量偏移
l：变量层数
if(l==0) _stack[point++] = _stack[en_0+a];
else if(l==1) _stack[point++] = _stack[en_1+a];
else _stack[point++] = _stack[tmpen+a];
根据l的值，用不同的基址。当l==2时，说明是参数处理过程中，由于之前已经分配了函数的空间，基址已经变化，所以这里用临时基址。

LOA：将数组中某个值取到栈顶
a：数组首地址偏移
l：数组层数
if(_stack[point-2]>=_stack[point-1]){
    err(OUT_OF_BOUND);//err
    stack[point-2] = 0;
    point--;
}
else{
    if(l==0) _stack[point-2] = _stack[en_0+a+(int)_stack[point-2]];
    else if(l==1) _stack[point-2] = _stack[en_1+a+(int)_stack[point-2]];
    else _stack[point-2] = _stack[tmpen+a+(int)_stack[point-2]];
    point--;
} 
在生成这条指令前，先计算了需要读取的index，然后将数组长度放入运行栈中。在读取前先判断了是否越界。
根据l的值，用不同的基址。当l==2时，说明是参数处理过程中，由于之前已经分配了函数的空间，基址已经变化，所以这里用临时基址。

STO：将栈顶值存入栈内某一变量
a：变量偏移
l：变量层数
if(l==0) _stack[en_0+a] = _stack[point-1];
else _stack[en_1+a] = _stack[point-1];
point--;
根据l的值，用不同的基址。

STA：将栈顶值存入数组
a：数组首地址偏移
l：数组层数
if(_stack[point-3]>=_stack[point-1]){
     err(OUT_OF_BOUND);//err
     _stack[point-2] = 0;
     point--;
}
else{
     if(l==0) _stack[en_0+a+(int)_stack[point-3]] = _stack[point-2];
     else _stack[en_1+a+(int)_stack[point-3]] = _stack[point-2];
     point -= 3;
}
在生成这条指令前，先计算了需要读取的index，然后将数组长度放入运行栈中。在读取前先判断了是否越界。
根据l的值，用不同的基址。当l==2时，说明是参数处理过程中，由于之前已经分配了函数的空间，基址已经变化，所以这里用临时基址。

CAL：函数调用
a：函数位置，即函数第一条代码在整个代码中的位置
_stack[en_1+2] = tmpen;
_stack[en_1+1] = i+1;
i = a-1;
将上层基址和返回地址存入运行栈，将程序计数器变为函数入口。这里赋值为a-1的原因是在外层循环中i会自动加1。

RET：函数返回
i = _stack[en_1+1]-1;
point = en_1 + 1;
en_1 = _stack[en_1+2];
将程序计数器、栈顶和当前基址恢复。

INIT：在栈顶开辟空间
a：开辟空间的个数
tmpen = en_1;
en_1 = point ;
point += a;
将当前基址存入临时基址，当前基址变为栈顶，栈顶向上移动。

JMP：无条件跳转
a：跳转地址
i = a-1;
将程序计数器变为跳转地址。这里赋值为a-1是因为外层循环中i会自动加1。

JPC：条件跳转
a：跳转地址
if(_stack[point-1]==0) i=a-1;
point--;
若栈顶值为假，则将程序计数器变为跳转地址。这里赋值为a-1是因为外层循环中i会自动加1。

OPR：算数运算和逻辑运算
a：运算类型
switch(a){
case _add:
_stack[point-2] += _stack[point-1];
point--;
break;
case _sub:
_stack[point-2] -= _stack[point-1];
point--;
break;
case _mult:
_stack[point-2] *= _stack[point-1];
point--;
break;
case _div:
_stack[point-2] /= _stack[point-1];
point--;
break;
case _gre:
_stack[point-2] = (_stack[point-2]>_stack[point-1])?1:0;
point--;
break;
case _geq:
_stack[point-2] = (_stack[point-2]>=_stack[point-1])?1:0;
point--;
break;
case _lss:
_stack[point-2] = (_stack[point-2]<_stack[point-1])?1:0;
point--;
break;
case _leq:
_stack[point-2] = (_stack[point-2]<=_stack[point-1])?1:0;
point--;
break;
case _beq:
_stack[point-2] = (_stack[point-2]==_stack[point-1])?1:0;
point--;
break;
case _neq:
_stack[point-2] = (_stack[point-2]!=_stack[point-1])?1:0;
point--;
break;
以上各种算数运算和逻辑运算都是取栈顶和次栈顶进行运算，结果保存在次栈顶。
case _back:
    point -= l;
    break;
}
栈顶回滚l个空间。

REA：从控制台输入，读到栈顶
a：读入类型
switch(a){
case 0:
cout<<"please enter a char"<<endl;
cin>>tmpc;
_stack[point] = (int)tmpc;
break;
case 1:
cout<<"please enter an int"<<endl;
cin>>tmpi;
_stack[point] = tmpi;
break;
case 2:
cout<<"please enter a float"<<endl;
cin>>_stack[point];
break;
}
point++;
根据要求的类型输出提示信息，等待用户从控制台输入数据。

WRI：向控制台输出栈顶内容或者字符常量
a：输出类型
l：0为栈顶1为字符常量
switch(a){
case 0:
if(l==0) tmpc = (int)_stack[point-1];
else{
    tmpc = l;
    point++;
}
cout<<tmpc;
break;
case 1:
tmpi = (int)_stack[point-1];
cout<<tmpi;
break;
case 2:
printf("%.6f",_stack[point-1]);
break;
}
point--;
根据要求类型向控制台输出。
8. 出错处理
词法分析中的错误：
#define FRONT_ZERO 0：数字有前零
#define MULTIPLE_PERIOD 1：多个小数点
#define NO_NUM_AFTER_FERIOD 2：小数点后没有数字
#define NOT_CHAR_IN_QMARK 3：单引号内没有字符
#define MULTIPLE_CHAR_IN_QMARK 4：单引号内多个字符
#define MISS_QMARK 5：缺少单引号
#define MISS_DQMARK 6：缺少双引号
#define MISS_ASSIGN 7：缺少等号

语法分析中的错误
#define REDEFINITION 8：重复定义
#define MISS_SEMICN 9：缺少分号
#define EXPECT_IDEN_IN_CDEFINE 10：常量定义缺少标识符，跳过到分号或者逗号
#define EXPECT_ASSIGN_IN_CDEFINE 11：常量定义缺少等号，跳过到分号或者逗号
#define EXPECT_INTCON_IN_CDEFINE 12：常量定义缺少常数，将值赋为0
#define EXPECT_FLOATCON_IN_CDEFINE 13：常量定义缺少常数，将值赋为0
#define EXPECT_CHARCON_IN_CDEFINE 14：常量定义缺少常数，将值赋为0
#define SHOULD_BE_TYPE_IN_CDEFINE 15：常量定义缺少类型名，跳过到分号
#define EXPECT_INTCON_IN_ARRAY 16：数组定义缺少常数，数组长度设置为0
#define EXPECT_RBRACK 17：数组定义缺少右中括号，退回一个单词
#define EXPECT_LPARENT 18：缺少左括号，退回一个单词
#define MISS_TYPE_IDEN 19：缺少标识符，退回一个单词
#define EXPECT_RPARENT 20：缺少右括号，退回一个单词
#define EXPECT_LBRACE 21：缺少左大括号，退回一个单词
#define EXPECT_RBRACE 22：缺少右大括号，退回一个单词
#define NOT_DEFINED 23：变量未定义，生成（lit，0，0）取一个0到栈顶
#define EXPECT_LBRACK 24：缺少左中括号，退回一个单词
#define IDEN_NOT_DEFINED 25：标识符为定义，跳过到分号
#define MULTIPLE_EXPRESSION 26：条件中表达式多余，跳过到右括号
#define EXPECT_COLON 27：缺少冒号，退回一个单词
#define SHOULD_BE_ENUMABLE_TYPE 28：case了非可枚举常量，生成（lit，0，0）
#define EXPECT_IDEN_IN_SCANF 29：scanf中不是标识符
#define SHOULD_BE_VAR_IN_SCANF 30：scanf中不是变量
#define SHOULD_BE_COMMA_OR_RPARENT 31：printf中出现非法符号
#define EXPECT_ASSIGN_IN_ASSIGN 32：赋值语句缺少等号，跳过到分号
#define TOO_MANY_PARA 33：参数过多，跳过到右括号
#define CONST_AFTER_VAR 34：常量定义位于变量定义之后
#define CANT_DEFINE_THERE 35：非法位置定义常量或变量
#define CANT_PREASSIGN 36：变量定义赋初始值
#define TOO_LESS_PARA 37：参数过少
#define OUT_OF_BOUND 38：数组越界
#define MULTILE_MUTLTOP 39：因子之间过多乘法运算符
#define CANT_ASSIGN_TO_NONVAR 40：向非变量赋值
#define CANT_RETURN 41：无返回值函数return一个值
#define SHOULD_RETURN 42：有返回值函数无返回值
#define MISS_FUNCNAME 43：缺少函数名
#define MISS_MAIN 44：缺少main函数
#define SHOULD_BE_VOID 45：main函数不为void




三．操作说明
1．运行环境
程序用C++语言所写，编程环境为codeblocks13.12。
2．操作步骤
程序编译之后可直接运行，输入完整路径便可执行。
若源程序有错则不会执行解释执行。
四．测试报告
1．测试程序及测试结果
正确的程序：
test1.txt：
输入：i2(int)，f2(float)，c2(char)
输出：i1 = 3,i2 = i2,f1 = 3.333000,f2 = f2,c1 = b,c2 = c2

test2.txt：
输入：十个整数ai
输出：十组数，每组为ai，ai+1

test3.txt：
输入：一个int，然后n个int，一个char，然后n个int
输出：
max of 5 1 2is:5
Input the choice: 1 2 3 , 0 for return
这里输入1会输出good
	输入2会输出better
	输入3会输出best
	输入0会结束循环
	输入其他会输出error
the ascii of the char is:97
Input the choice: 1 2 3 , 0 for return 
这里输入1会输出good
	输入2会输出better
	输入3会输出best
	输入0会结束循环
	输入其他会输出error

test4.txt：
输入：不大于12的整数n
输出：n！

test5.txt：
输入：不大于45的整数n
输出：n个斐波那契数

错误的程序：
	test1-e.txt： 
	13行缺少左花括号
	25行函数缺少参数
	test2-e.txt：
	5行缺分号
	14，20，23，30行向非变量赋值

	test3-e.txt：
	44行重复定义标识符
 
	test4-e.txt：
	5行return值没用括号括起来
	 
	test5-e.txt：
	源程序为空或不完整

2．测试结果分析
test1.txt：测试运算和输入输出
test2.txt：测试数组
test3.txt：测试while，if，switch以及char的ASCII运算
test4.txt：测试递归
test5.txt：测试递归和数组
	每一个例子都测试了函数调用。

五．总结感想
首先，我要先说一下这次课程设计最大的遗憾，就是没有选择高级。
一开始对自己没有信心，抱着求过的心态选择了中级，但在完成第一版程序并且初步调试通过之后就意识到自己其实有能力完成高级的课程设计。
在这次课程设计的过程中，我有了很多体会和收获。
第一步是写词法分析程序，由于本学期选修了高级语言程序设计2，再加上大二阶段面向对象建模这门课的锻炼，对字符串处理掌握的比较熟练，所以词法分析程序很快就完成了。但在写错误处理的过程中遇到了一个难点，就是字符串常量和字符常量缺少一个引号的情况。在尝试了很多次之后我加入了缓冲区，这样就可以处理这个情况了。虽然错误处理在程序测试中占的比例并不大，但我还是希望自己的程序能够做到尽量完善。
之后便要完成整个编译系统的编码，但在一开始，理论课掌握的还不是很牢固，对递归子程序法理解并不深刻，所以不知道从何下手。在不知道如何进展下去的时候，我请教了一位已经开始编码的同学，大致了解了递归子程序法，开始尝试着编码。很快就得心应手。
在写语法分析程序的过程中，我采用了一个方法：先写出语法分析框架，再逐步加入代码生成和错误处理。完成语法框架之后，我采用了一个巧妙的调试方法，printf调试法——这是上学期操作系统试验助教交给我们的方法。大致做法就是保留词法分析的输出，并在每一个子程序入口和出口向该文件输出两句话，以显示子程序调用关系。这样一来，此法分析和子程序的输出配合起来，就可以清晰地看到子程序调用关系以及进入子程序时读到的单词和退出子程序时读到的单词，如此便可以很容易地看到程序有没有按正确的语法分析框架走。
语法分析框架调试完成后，我加入了代码生成。这里调试比较困难一些，我依旧采用的是printf调试法——将符号表和生成的PCODE分别输出到两个文件，再对比源程序，看代码生成是否正确。确保代码生成正确后，我完成了解释执行程序。至此，第一个版本的编译器已经完成，测试了几个正确的程序都能通过。
然后就是加入错误处理的部分，在语法分析框架编写的时候，我已经在需要错误处理的地方做了标记，所以添加起来也很方便。由于只要源程序有错就不需要进入解释执行，所以我对大部分错误的处理方式是跳词，跳过部分单词直至可以继续编译的地方，同时输出错误信息。
后期的调试大部分都在完善错误处理程序。但在几次期中测试中也发现了一些bug。比如符号表管理中，子函数编译完成之后应该删除其参数、常量和变量的符号表，但我在编码过程中，有一种情况没考虑到，所以没有删除，导致整个程序错误。就在这样不断的测试中，我的编译器在一步步完善。
在临近最后提交的时期，我又进行了一次全面的测试。在测试最复杂的递归程序的时候，发现了错误。是解释执行程序中函数空间分配和基地址变换出了错，在简单的调试后递归程序测试通过。
至此，一个比较完善的编译器已经完成。
总的来说，编译技术课程设计是一门非常好的课。不但巩固了理论课的学习，还锻炼了我们程序设计能力，培养了缜密分析思考问题的能力。
最后，再次抱憾没有选择高级难度。
编译技术课程设计是一次对自己的挑战，更重要的是给了我更大的信心。
