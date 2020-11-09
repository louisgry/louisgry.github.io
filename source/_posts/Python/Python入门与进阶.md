---
title: Python 3入门与进阶-imooc
date: 2020-04-10 16:24:04
categories: Python
tags: 
- Ebbinghaus
description: 
---

[1：Python入门导学](#1：Python入门导学)
[2：Python环境安装](#2：Python环境安装)
[3：理解写代码与Python基本类型](#3：理解写代码与Python基本类型)
[4：python中表示“组”的概念和定义](#4：python中表示“组”的概念和定义)
[5：变量与运算符](#5：变量与运算符)
[6：分支、循环、条件](#6：分支、循环、条件)
[7：包、模块、函数与变量作用域](#7：包、模块、函数与变量作用域)
[8：Python函数](#8：Python函数)
[9：高级部分-面向对象](#9：高级部分-面向对象)
[10：正则表达式与JSON](#10：正则表达式与JSON)
[11：Python的高级语法与用法](#11：Python的高级语法与用法)
[12：函数式编程](#12：函数式编程)
[13：实战-爬虫](#13：实战-爬虫)
[14：Pythonic与Python杂记](#14：Pythonic与Python杂记)


<!-- more -->
### 1：Python入门导学
效率
- 1.1：导学
    - Python诞生于上世纪90年代
    - 广泛使用公司：豆瓣、知乎
    - > 了解语法是编程的先决条件，精通语法是编程的必要条件
    - 详细讲解常见错误、Pythonic、提炼实际编程中遇到的问题
    - 最基础的Python语法编写一个爬虫（了解爬虫原理）
    - Life is short, I use Python（简洁）
    - 一生只能选一种语言，我会选Python
    - Pythonic：例如交换变量
    ```
    x, y = y, x
    ```
    - Python能做什么：AI、大数据、测试、Web
    - Python之禅：`import this`
- 1-2：Python的特点
    - 跨平台、强大的第三方库、面向对象
- 1-3：我为什么喜欢Python
    - Python既有动态脚本的特性（动态变量），又有面向对象的特性（js支持不好）
- 1-4：Python的缺点（慢）
    - 编译型语言（C、C++）
    - 解释型语言（Javascript、Python）
    - 个例，Java和C#（编译不会成机器语言而是中间语言）
    - 效率的两种理解：运行效率和开发效率
- 1-5：一个经典的误区
    - 编程=Web编程？
- 1-6：Python能做些什么
    - 自动化运维（实际工作是怎么样的？？）
    - 胶水语言（怎么把C++、Java整合到一起？？）
    - 几乎是万能的：当你遇到问题，就编写Python代码解决问题
- 1-7：课程内容与特点
    - 基础语法与Pythonic
    - Python高性能与优化
    - 数据结构
- 1-8：Python的前景
    - 排行榜第四

### 2：Python环境安装
- 2-4：Python多版本问题
    - 查看版本：`python -V`
    - linux查找目录：`which python3`
- 2-8：IDLE与第一段Python代码
    - [IDLE](https://docs.python.org/3/library/idle.html)：IDLE is Python’s Integrated Development and Learning Environment. 

### 3：理解写代码与Python基本类型
- 3-1：代码，写代码
    - 代码：现实世界事物在计算机世界中的映射
    - 写代码：将现实世界中的事物用计算机语言来描述
- 3-2：整型与浮点型
    - Number数据类型
        - 包括：整数（int），浮点数（float）
        - 注意：Python里没有单精度、双精度的区分，Python的float就是双精度的
    - 整数除整数，特例情况
    ```
    type(2/2)  # ==> <class 'float'>
    type(2//2) # ==> <class 'int'>
    1//2 # ==> 0
    ```
- 3-3：进制
    - Python2：有 long（长整型）
- 3-4：各进制的表示与转换
    - 二进制：在前面加`0b`（默认输出十进制）
    - 八进制：在前面加`0o`
    - 十六进制：在前面加`0x`
    ```
    0b10 # ==> 2
    10 # ==> 10
    0o10 # ==> 8
    0x10 # ==> 16
    ```
    - `bin()`：各进制转二进制
    - `int()`：各进制转十进制
    - `oct()`：各进制转八进制
    - `hex()`：各进制转十六进制
- 3-5：布尔类型与复数
    - Number类型：int、float、bool、complex（复数）
        - 复数：在数字后面加‘j’（36j）
    - bool：`True、False均大写`
    - False：数值为0、None和空时为False
    ```
    int(True) # ==> 1
    int(False) # ==> 0
    bool(-1.1) # ==> True
    bool(0) # ==> False
    bool(None) # ==> False
    bool('') # ==> False
    ```
- 3-6：字符串
    - str字符串：单引号和双引号是一样的
- 3-7：多行字符串
    - 三引号`'''`（包括是三个单引号、双引号）
    ```
    ''' 
    Hello world
    hello world
    '''
    ```
- 3-8：转义字符
    - 加`\`（`\n`换行`、\t`制表符、`\r`回车）
- 3-9：原始字符串
    - 输出原始字符串：加`r`
    - `print(r'c:\northwind\north')`
- 3-10~12：字符串运算
    - `+`：合并字符串、`*`：重复字符串
    - 索引和切片：截取字符串（不包括最后一个，左闭右开）
    ```
    print('helloworld'*3)` # ==> 'helloworldhelloworldhelloworld'
    "helloworld"[-1] # ==> 'd'
    "helloworld"[0:5] # ==> 'hello'
    ```
    - 从头或从尾截取（:所在位置）
    ```
    "python c c++ c# java javascript python"[:6] # ==> 'python'
    "python c c++ c# java javascript python"[-6:] # ==> 'python'
    ```

### 4：Python中表示“组”的概念和定义
- 4-1：列表（list）的定义
    - list：列表内部元素可以为任意类型，也可以为列表（列表里嵌套列表，二维数组）
    ```
    ["hello", "world", 1, 9, True, False]
    [[1,2,3], [4,5,6]]
    ```
- 4-2：列表的基本操作
    - 加法（没有减法）：合并list
    - 乘法（不是乘列表）：重复元素
    - 索引和切片
    ```
    ['hello'] + ['world'] # ==> ['hello', 'world']
    [1] + [2] # ==> [1,2]
    [1,2,3]*3 # ==> [1,2,3,1,2,3,1,2,3]
    ['hello', 'world'][0] # ==> 'hello'
    ```
- 4-3：元组（tuple）
    - 元组内部也可以是多类型，与列表的区别：不可更改
    ```
    (1,2,True,'a')
    ```
    - 元组的操作：加法、乘法、索引和切片
    - 单个元组表示：要用`(1,)`，`()`表示括号运算(1)
    ```
    type((1)) # ==>  <class 'int'>
    type(('1')) # ==>  <class 'str'>
    type((1,)) # ==>  <class 'tuple'>
    type([1,]) # ==> <class 'list'>
    ```
- 4-4：序列总结
    - 序列：str，list，tuple（序列里面的元素都有序号）
    - 序列共有操作：加法、乘法、索引、切片
    - 判断元素是否在序列中
    ```
    3 in [1,2,3,4,5] # ==> True
    3 not in [1,2,3,4,5] # ==> False
    ```
    - 长度、最值
    ```
    len('hello world') # ==> 11
    max([1,2,3,4,5]) # ==> 5
    min('hello world') # ==> 输出空格
    max('hello world') # ==> w
    ```
    - 获取ascii码（ord）
    ```
    ord(w) # ==> 119
    ```
- 4-5：set集合
    - set最大的特点：无序
    - 第二个特点：不重复
    ```
    type({1,2,3}) # 定义集合 { }
    {1,1,2,2,3,3} # ==> {1,2,3}
    ```
    - set获取不了元素、无法进行切片，无加法乘法
    - 其他操作：长度len，最值，是否在集合中in
    - 特殊操作：-（求差集），&（求交集），|（求并集）
    ```
    {1,2,3,4,5} - {4,5} # ==> {1,2,3}
    {1,2,3,4,5} & {4,5,6,7} # ==> {4,5}
    {1,2,3,4,5} | {3,4,5,6} # ==> {1,2,3,4,5,6}
    ```
    - 空集合
    ```
    type({}) # ==> class 'dict'
    type(set()) # ==> class 'set'
    ```
- 4-5：dict字典
    - 字典：key（不重复），value（可重复）
    ```
    {'Q':1, 'W': 2}
    {'Q':1, 'W': 2}['Q']
    ```
    - key不重复（不可变）：
    ```
    {1: 'hello', '1':'world'} # ==> 1、'1' key不同
    ```
- 4-6：总结基本数据类型
    - 数字（Number）
        - 整型 int
        - 浮点型 float
        - 布尔型 bool
        - 复数 complex
    - 组
        - 序列：字符串 str，列表 list，元组 tuple
        - 集合 set
        - 字典 dict

### 5：变量与运算符
- 5-1：变量
    - 动态变量
- 5-2：变量命名规范
    - 模块变量函数名：首字母不能是数字、小写、单词间用_分割
    - 类名：首字母大写
    - 私有变量：双下划线开头
- 5-3：值类型与引用类型
    ```
    type = 1
    type(1) # ==> 'int' object is not callable （不可调用的）
    del type # 释放变量内存
    ```
    - 值类型（不可改变int，str，tuple）
    ```
    a = 1
    b = a
    a = 3
    print(b)` # ==> 1
    ```
    -  引用类型（可变list，set，dict）
    ```
    a = [1,2,3]
    b = a
    a[0] = '1'
    print(b)` # ==> ['1',2,3]
    ```
    - 思考解释：str是不可变，那字符串相加不是改变了？
    ```
    b = 'hello'
    id(b)
    b = b + 'python'
    id(b) # ==> id改变了，说明引入了新的字符串来相加
    ```
- 5-4：列表的可变与元组的不可变
    - 可改变的元组
    ```
    a = (1,2,[3,4])
    a[2]=5 # ==> TypeError
    a[2][0] = 5 # ==> (1,2,[5,4])
    ```
- 5-6：赋值运算符
    - Python没有`++`, `--`操作
- 5-8：不只是数字才能做比较运算
    - 思考题
    ```
    b=1
    b+=b>=1 # ==>2 (1+True=2)
    ```
- 5-9：逻辑运算符
    - 当作bool的非bool类型运算（考虑短路原则）
    ```
    'a' and 'b';  'a' or 'b' # ==> 'b', 'a'
    [] and [1];  [1] and [] # ==> [1],[1]
    0 or 1;  1 or 0; 1 or 2 # ==> 1,1,1
    ```
- 5-10：成员运算符
    - in和not in
    - 字典dict：只针对key
    ```
    1 in {'a':1} # ==> False, 
    'a' in {'a':1} # ==> True
    ```
- 5-11：身份运算符
    - is
    ```
    a=1, b=2, a is b # ==> False
    ```
    - is与==的区别：is判断id
    ```
    a=1
    b=1.0
    a==b # ==> True
    a is b # ==> False( id 不相等）
    ```
    - is not
- 5-12：如何判断变量的值、身份与类型
    - 对象的三大特征：
    - 值 ==，身份 is，类型 type（isinstance更好：可以判断变量子类的类型）
    ```
    isinstance('hello', str) # ==> True
    isinstance('hello', (int,float,str)) # ==> 是否是int，float，str中的一种类型
    ```
- 5-13：位运算符（把数字当作二进制数进行运算）
    - `&`：按位与
    - `|`：按位或
    - `^`：按位异或（相异为真）
    - `~`：按位取反
    - `<<`：左移动
    - `>>`：右移动

### 6：分支、循环、条件
- 6-1：什么是表达式
    - 表达式（Expression）是运算符（operator）和操作数（operand）所构成的序列
- 6-2~3：表达式优先级
    - not 先于 and 先于 or
- 6-4~5：VSCode开发环境与Python插件安装
    - [应用商店](https://marketplace.visualstudio.com/vscode)
    - 右键文件“在命令行打开”定位到文件目录 
    - 自动保存：文件 - 自动保存
    - 命令行：`ctrl+~`
    - 查找文件：`ctrl+p`
    - 快捷键修改：`shift + enter`
- 6-6~7： 流程控制语句之条件控制
    - pymysql 规范代码`__init__.py`
    - 两个特点：
        - 末尾不需要加；
        - 不需要加{}，以缩进表示代码块
    - 多行注释`'''`
    - 条件判断：
    ```
    user_password = input()
    if user_password == '123456':
        print('Login successfully!')
    ```
- 6-8：常量与Pylint规范
    - Python常量：`全大写`
    - Python变量：在函数、类中
    - `:` 前面不要有空格
    - `程序末尾需换行`
- 6-9：snippet、嵌套
    - snippet：一般不用，手写if else的过程中思考
    - `pass`：空语句占位，搭程序框架、API接口交互
    - python没有`goto`关键字
    - 思想：不要嵌套多层，使用`函数`封装
- 6-10：elif的优点
    - python没有`switch`，用`elif`替代
    - python官网[Design and History](https://docs.python.org/3/faq/design.html#why-isn-t-there-a-switch-or-case-statement-in-python)
- 6-11：改变定势思维
    - a和b为数字，但不同时为False
    ```
    a or b
    ```

### 7：包、模块、函数与变量作用域
- 7-1：while循环及使用场景
    - while...else
    ```
    while count<10:
        print(count)
    else:
        print('EOF')
    ```
- 7-2：for与for-else循环
    - for循环：
    ```
    for x in x_list:
        print(x)
    ```
    - for-else：
    ```
    for x in range(5):
        print(x)
    else:
        print('for-else')
    ```
- 7-3：for与range
    - range：
    ```
    for x in range(0,10,2):
        print(x)  # ==> 0,2,4,6,8
    ```
    - 逆序
    ```
    for x in range(10,0,-2): 
        print(x)  # ==> 10,8,6,4,2
    ```
- 7-4：新篇章
    - for循环
    ```
    for i in range(0,len(li),2):
        print(li[i])
    ```
    - 用切片代替循环
    ```
    print(li[0: len(li): 2])
    ```
- 7-5~6：Python工程的组织结构：包、模块、类
    - 组织结构：包（文件夹）、模块（文件）、类
    - 命名空间
    - 包与普通文件夹的区别：是否含有`__init__.py`
    - `__init__.py`的模块名就是他的包名，不是`seven.__init__`
- 7-7：import导入模块
    - 导入“包.模块”名
    ```
    import t.c1
    import t.c7 as c
    ```
    - `__pycache__`里的`.pyc`文件是Python的字节码文件
- 7-8：from import导入变量
    - from import
    ```
    from t.c7 import a
    ```
    - 直接导入模块
    ```
    from t import c7
    print(c7.a)
    ```
    - 不推荐：`from t import *`
    - 如果要`import *`，可以在类中可定义‘*’的变量
    ```
    __all__ = ['a','c']  # b不让导入
    a = 1
    b = 2
    c = 3
    ```
- 7-9：`__init__.py` 的用法
    - vscode设置隐藏pycache文件夹：
        - 用户设置 - ‘files.exclude' - '__pycache__'
    - 换行的两种方法：
        - `\`（不推荐）
        - `()`（推荐）
    - 导入多个模块
    ```
    from c9 import a, b, c
    ```
    - `__init__.py` 导入时会自动运行
        - 在c11.py中`import t`，`print('this is __init__.py'` 会被自动执行
        - 限制导入哪个包：在`__init__.py`中限制`__all__ = ['c7']`
    - 批量导入（多个模块需要）：在`__init__.py`中：
    ```
    import numpy
    import pandas 
    ```
    - vscode清屏命令：`cls`
- 7-10：包与模块的几个常见错误
    - 包和模块不会被重复导入
    - 不能循环导入：`p1 import p2, p2 import p1`
- 7-11：模块内置变量
    - 在模块中，使用dir()函数
    ```
    print(dir())  # 返回当前模块的所有变量
    ```
    - `__name__`：t.c9
    - `__package__`：t
    - `__doc__`：docstring（'''）
    - `__file__`：路径
- 7-12：入口文件和普通模块内置变量的区别
    - Error（must be str, not NoneType）修改：
    ```
    print(__doc__ or 'none doc string')
    ```
    - 入口文件的差异：
        - `print(__name__)`是`__main__`
    - `__package__`没有值：因为入口文件的包是顶级
- 7-13：`__name__`的经典应用
    - dir查看某个模块
    ```
    import sys
    dir(sys)
    ```
    - 既可作模块也可作可执行文件：当import时，不会执行if里的代码
    ```
    if __name__ == '__main__': ... # make a script both importable and executable
    ```
    - 可执行文件以模块方式运行
    ```
    python -m seven.c13  # 直接使用c13会报错
    ```
- 7-14：相对导入和绝对导入（一）
    - 关于顶级包的理解（不是绝对的，取决于入口文件的位置）
        - 目录结构为：demo下有package1、package2、main.py，则`import package1.m`
        - 目录结构为：demo下有package1、package2、main.py在package2中，则`import demo.package1.m`
    - 入口文件一般为 `main.py`（入口文件不能用相对导入）
    - 绝对导入：从顶级包开始导入
    - 相对导入：`.`表示同级，`..`表示上一级
- 7-15：相对导入和绝对导入（二）
    - 更改顶级包位置：将可执行文件以模块方式运行
    ```
    cd ..
    python -m demo.main
    ```

### 8：Python函数
- 8-1：认识函数
    - round函数
    ```
    round(a, 2) # 四舍五入保留两位小数
    round(4.5)  # ==> 4，五保留
    ```
    - vscode新建另一个命令行：旁边的加号进入python命令行
    - `help(round)` # 查看函数信息，Enter翻页
    - 特点：功能性，隐藏细节（使用时候简单），避免编写重复的代码
- 8-2：函数的定义及运行特点
    - 递归错误RecursionError: maximum recursion depth exceeded
    - `import sys ... sys.setrecursionlimit(100000)`
    - 函数调用顺序及return None
    - `a = add(1,2)` `b = print_code('python')` `print(a,b)` # ==> python ...3 None
- 8-3：如何让函数返回多个结果
    - return之后的代码不会执行
    - 返回多值：`return a, b`
    - 用索引查找返回结果是很不好的，建议使用序列解包：
    - `ans1, ans2 = fun(a, b)`
- 8-4：序列解包与链式赋值
    - 序列解包：
    - `d = 1, 2, 3` # class 'tuple'  `a, b, c = d`
    - 链式赋值：
    - `a=b=c=1`
- 8-5：必须参数与关键字参数
    - 形参、实参
    - 关键字参数（代码的可读性）：
    - `ans = add(b=1, a=2)` # 不需要按顺序赋值
- 8-6：默认参数
    - 坑：不能将必须参数放在默认参数之后
    - Error: def add(a, b=1, c)
- 8-7：可变参数
    - 可变参数（加*）
    - `def demo(*param): ... print(param) ... print(type(param)) ... demo(1,2,3,4)` # (1,2,3,4) class 'tuple'
    - 特性：实参加*，不会叠加成二维tuple，如下
    - `a = (1,2,3,4) ... demo(*a)` `demo(*[1,2,3])`
- 8-8：关键字可变参数
    -  for 遍历可变参数
    -  `for i in param: ... sum += i`
    - 关键字可变参数（支持任意个数关键字）
    - `def tem(**param): ... print(param)`
    - `tem(bj=32, xm=30,sh=31)`
    - 打印key和value
    - ` for key, value in param.items(): ... print(key, ':', value)`
    - 特性：`a={'xm': 30, 'sh': 31}` `tem(**a)`
- 8-9：变量作用域
    - 函数里面调用函数
    - `def fun1(): ... c = 2 ... def fun2(): c=3 print(c) ... fun2()` `fun1()` # 3
- 8-10：作用域链
    - 上面那个代码：作用域的链式特性
    - 纠正：for里面不算作用域！ 
- 8-11：global关键字
    - 内部变量变成全局变量：`global a`
- 8-12：划算还是不划算
    - Python最适合用来解决问题（不要局限于web编程）
    - 练习：游戏的经济系统（五行石合成问题）
        - 探究自己合成划算，还是从别人那购买划算
        - 合成过程消耗金、钻石、体力（为了得到6级五行石）
        - 1级：金+钻石；3级：+1级；4级：+一定概率；6级：4级
### 9：高级部分-面向对象
- 9-1：类的定义
    - 类 != 就是面向对象
    - 面向对象核心：类（数据成员&方法）、对象
    - 类（class，**首字母大写**，不要用_连接，基本作用：封装）
    - `class Student(): ... name = '' ... def print_file(self): ...print('name:' + self.name)` # 必须加**self**
    - 实例化：`student = Student()` # 不需要new关键字
    - `student.print_file()`  # 调用类中的方法（不是函数，有self关键字，调用在类外）
- 9-3：类与对象
    - 类：是现实世界或思维世界中的实体在计算机中的反映
    - 它将数据以及这些数据的操作封装在一起
    - 设计：特征 + 行为 
- 9-4：构造函数
    - `__init__()` # 构造函数，自动调用
    - `type(student.__init__())` # NoneType，显式调用，**不能return值**
    - 初始化对象属性：
    - `def __init__(self, name,  age): ... self.name = name ... self.age = age`
- 9-6：类变量与实例变量
    - 类变量（类）：`name='' ... age=0` # 使用self保存类变量
    - 实例变量（对象）：`student = Student('louis', 23)`
    - 赋值：`self.name = name`
    - self不是关键字，可以改其他名称如this
    - 实例变量`print(student.name)` 类变量`print(Student.name)`
- 9-7：类与对象的变量查找顺序
    - 学习方法：实例讲解
    - 为什么`name=name ... age=age`会为初始值
    - `print(student.__dict__)` # __dict__保存变量，此时为空
    - 寻找机制：先查找实例变量，再查找类变量，最后查找父类中的变量
- 9-8：self与实例方法
    - 实例方法：第一个参数是self
    - 调用不需要self，在__init__中调用
    - `print(name)` #  打印出实例变量
    - self：当前调用某一个方法的对象
- 9-9：在实例方法中访问实例变量与类变量
    - 类：
        - 变量（类变量、实例变量）
        - 方法（实例方法、类方法、静态方法）
        - 构造方法
    - 实例方法中访问类变量：self.变量
    - `print(Student.sum)` `print(self.__class__.sum)`
    - 实例方法中：`print(self.name) ... print(name)` # 值一样，但是二者不相等，第二个是形参
- 9-10：类方法
    - 类方法：专门操作类变量，不操作实例变量时（访问不了实例变量）
    - 定义：`@classmethod` # 类方法标记，装饰器
    - `def plus_sum(cls):` # 首字母cls
    - `cls.sum += 1 ... print(cls.sum)`
     - `Student.plus_sum()`
    - 建议不要：用对象调用`student.plus_sum()`
- 9-11：静态方法
    - 定义：`@staticmethod` # 装饰器标记
    - `def add(x,y):  .... print('static method')`
    - 可以同时被类和对象调用
    - 与面向对象关联很弱，不建议使用
    - **Python的类方法与C#的静态方法类似**
- 9-12：成员可见性
    - 私有：变量函数前加__
    - 构造函数（特有函数，前后都有__ 就不是私有了）
    - 但是私有变量还是能访问：`student.__score = -1` #  但不是类变量
- 9-13：没有什么是不能访问
    - `student.__score = -1` `print(Student.__dict__)` #_Student__score, __score
    - 间接读取私有变量：`student._Student__score` # 设置私有即更改类名
- 9-14：继承
    - `class People(): ... def __init__(self, name, age): ... self.name=name ... self.age=age ... def get_name(self): ... print(self.name)`
    - `from c6 import People ... class Student(People): ...pass `
    - `student = Student('louis', 23) ...print(student.name)`
    - 构造函数的继承
    - `Student(People): ... def __init__(self, school, name, age): ... self.school=school ... People.__init__(self, name,  age)` # 子类显式调用父类构造函数（**要传入self**）
- 9-15：子类方法调用父类方法
    - People.__init__(self, name, age) 类调用实例方法（普通方法调用）
    - 知识点：Student.do_homework() # Error：需要传入self = student，'louis'
    - 开闭原则：不能因为父类变了更改代码
    - 使用`super`关键字调用（super代表父类）
    - `super(Student, self).__init(name, age)` 替换上面的People.__init__
    - 子类方法与父类方法同名，使用子类方法
    - 调用父类方法：`super(Student, self).do_homework()`

### 10：正则表达式与JSON
- 10-1：初始正则表达式
    - 正则表达式：是一个特殊的字符序列，一个字符串是否与我们所设定的这样的字符序列相匹配
    - 用于快速检索文本、实现一些替换文本操作
    - 检测字符串`a = 'C C++ Java Python Javascript'`是否包含‘Python’
    - `'Python' in a `或`a.index('Python') > -1`
    - 使用正则表达式：
    - `import re ... r=re.findall('Python', a)`  # ==> ['Python']
    - 正则表达式的灵魂：规则
- 10-2：元字符与普通字符
    - 提取字符串中所有的数字：
    - `r = re.findall('\d', a)`
    - 提取字符串中的非数字：
    - `r = re.findall('\D', a)`
    - 元字符：\d，普通字符：Python
- 10-3：字符集
    -  字符集：`s = 'abc, acc, adc, aec, afc'`
    - 查找字符串中间一个是c或f的字符：
    - `r = re.findall('a[cf]c', s)` # acc, afc
    - `r = re.findall('[cf]', s)` # c,c,c,c,c
    - `[]`：中括号表示'或'关系
    - 匹配不是c和f的字符：`a[^cf]c`
    - 匹配c到f的字符：`a[c-f]c`
- 10-4：概括字符集
    - 找数字的另一种方法：`[0-9]`，非数字：`[^0-9]`
    - 匹配数字和字母：`\w` = `[A-Za-z0-9_]`
    - 非数字和字母：`\W` # 包括空格和回车
    - 匹配空白字符：`\s` # 回车，制表符，空格
    - 匹配换行符\n以外的字符：`.`
- 10-5：数量词
    -  `a = 'python 111 java php'`
    - 匹配字母字符串：`[a-z]{3,6}` # 区间3至6：python java php【逗号后无空格】
    - 思考：为什么匹配到3个的时候不会直接返回
- 10-6：贪婪与非贪婪
    - 贪婪：尽可能匹配更多
    - Python倾向于贪婪匹配
    - 非贪婪模式：`[a-z]{3,6}?` # 加`?`问号：pyt，hon，jav，php
- 10-7：匹配0,1,无限多次
    - `*`：匹配0次或无限多次
    - `a = 'python0python1pythonn2'`
    - `r = re.findall('python*', a)` # 星号，['python', 'python', 'pythonn']
    - `+`：匹配1次或无限多次
    - `r = re.findall('python+', a)` # 加号，['python', 'python', 'pythonn']
    - `?`：匹配0次或1次（去重）
    - `r = re.findall('python?', a)` # 问号，['python', 'python', 'python']
    - `python{1,2}`贪婪模式pythonn，非贪婪模式python
- 10-8：边界匹配符
    - 前`^`后`$`：边界匹配符
    - QQ号长度需满足4到8位：
    - 大于8位仍满足：r = re.findall('\d{4,8}', qq)
    - 正确：`^\d{4,8}$` # 边界匹配
- 10-9：组
    - `{}`：组（表示且关系，[]表示或）
    - `a = 'PythonPythonPythonPython'`
    -  判断字符串是否包含100个Python单词：
    - `r = re.findall('(Python){100}', a)`
- 10-10：匹配模式参数
    - `re.I`：忽略大小写
    - `r = re.findall('c', a, re.I)`
    - `re.S`：使得`.`能够匹配换行符\n
    - 同时支持大小写和.号：`re.findall('c#.{1}', a, re.I | re.S)`
- 10-11：re.sub正则替换
    - `re.sub`：查找后的替换
    -  `r = re.sub('C#', 'go', a)` # 将C#替换成go
    - 无限制替换，第四个参数赋值为0：`re.sub('C#', 'go', a, 0)`
    - python内置：`a = a.replace('C#', 'go')`
    - 强大的功能：第二个参数可是函数
    - `def convert(value): ... print(value) ` # 返回match对象，span(6,8)：C#前面有6个，其占7,8位
    - `r = re.sub('C#', convert, a, 1)`
- 10-12：把函数作为参数传递
    - 大于6变9，小于6变0：
    - `def convert(value): ... matched = value.group() ... if int(matched) >= 6: ... return '9' ... else: ... return '0'`
    - `r = re.sub('\d', convert, a)`
    - 软件设计：convert交付给其他工程师编写
- 10-13：search与match函数
    - `re.match('\d',a)` # ==> None
    - match从首字母开始
    - `re.search('\d',a)` # ==> span(1,2)
    - search搜索直到满足，然后返回结果
- 10-14：group分组
    - `s = 'life is short, I use python'`
    - 提取life和python之间的字符串：
    - `r = re.search('life.*python', s)` `print(r.group())`# 返回包含首尾
    - 爬虫获取标签中间的内容：group
    - `r = re.search('life(.*)python', s)` `print(group(1))` # ==> is short, I use
    - 使用findall：`r = re.findall('life(.*)python', s)` `print(r)`
    - 多个分组：` s = 'life is short, I use python, I love python'`
    - `r = re.search('life(.*)python(.*)python', s)` `print(r.group(0,1,2))` `print(r.groups())`
- 10-16：理解JSON
    - JSON（Javascript Object Notation）：是一种轻量级的数据交换格式（对比XML）
    - 强调：JSON是**数据格式**，字符串是JSON的表现形式
    - JSON字符串：符合JSON格式的字符串
    - JSON优势：易于阅读、易于解析、网络传输效率高、跨语言交换数据
- 10-17：反序列化
    - JSON字符串（双引号）：`json_str = '{"name":"louis", "age":18}'`
    - JSON数组（中括号）：`json_arr = '[{"name":"louis", "age":18},{"name":"louis", "age":18}]'`
    - 反序列化：字符串到某一数据类型
    - `student = json.loads(json_str)`
- 10-18：序列化
    - `students = [ {'name':'louis', 'age':23 }, { }]`
    - `json_str = json.dumps(students)`
    - NoSQL数据库适合存储对象
- 10-19：JSON、JSON对象、JSON字符串
    - 跳出语言限制（JSON不只是Javascript方面的，可以看成平级）
    - JSON对象：python中没有，Javascript中有
    - 理解：JSON中间数据类型，有自己的数据类型

### 11：Python的高级语法与用法
- 11-1：枚举其实是一个类
    - `from enum import Enum`
    - `class VIP(Enum): ... YELLOW= 1 ... GREEN= 2` # 枚举：全大写
    - `print(VIP.YELLOW)` # ==> VIP.YELLOW
- 11-2：枚举和普通类的区别
    - 字典和普通类：可变、没有防止相同值的功能
- 11-3：枚举类型、枚举名称、枚举值
    - 获取枚举值：`VIP.GREEN.value`
    - 枚举名称：`VIP.GREEN.name` 
    - 枚举类型：`VIP.GREEN` `VIP['GREEN']`
    - `for v in VIP: ... print(v) ` # VIP.YELLOW VIP.GREEN
- 11-4：枚举的比较运算
    - 可做等值比较，不能做大小比较
    - 身份比较 is
- 11-5：枚举注意事项
    - 枚举的值一般不相等
    - 遍历出别名：`for v in VIP.__members__: ... print(v)`
- 11-6：枚举转换
    - 数据库中0,1,2,3的值转枚举类型
    - `VIP(0)` `VIP(1)` `VIP(2)` 
- 11-7：枚举小结
    - `from enum import IntEnum`
    - `class VIP(IntEnum)` # 限制枚举为整型
    - `from enum import unique`
    - `@unique` # 限制值必须不同
    - 枚举类型不能实例化（23种设计模式：单例
- 11-8：进阶内容
    - 基础：业务逻辑开发者，不考虑太多封装性
    - 进阶：包、类库开发者
- 11-9：一切皆对象
    - Python中函数也是对象    
- 11-10：什么是闭包
    - 不能在外部调用curve()函数
    - `f = curve_pre()`
    - `a = 10` `f(2)` # ==> 100 不受外部变量影响
    - 闭包 =  函数 + 环境变量（保存在f.__closure__）
    - 取出环境变量：`f.__closure__[0].cell_contents`
- 11-12：闭包的经典误区
    - `def f1(): ... a=10 ... def f2(): ...... a = 20 ... print(a) ... print(a) # 与f2同级 ... f2() ... print(a) ... f1()` # ==> 10 20 10
    - 判断是不是闭包：`print(f.__closure__)`
    - 不是闭包： 
    
     - 是闭包：
    
 
- 11-13：出个题，用闭包解决
    - 题目：旅行者； x走一步加1；返回总共走了多少步
    - 关键：每次走需要保存上一步的值
- 11-14：先用非闭包解决
    - error：出现在等号左边时就是局部变量了
     
    - 更改：增加`global`
    
 - 11-15：再用闭包解决
    - `nonlocal` 非本地局部变量
    - 非闭包问题：全局变量容易被修改
    - factory：工厂语言
    - 
 - 11-16：小谈函数式编程
    - 保存变量，除了闭包，可以使用OOP中的类变量
### 12：函数式编程
- 12-1：lambda表达式
    - 匿名函数（表达式）：`f = lambda x,y: x+y`
    - `f(1,2)` # ==> 3
- 12-2：三元表达式
    - 取x，y较大值：
    - `r = x if x > y else y`
- 12-3：map
    - `def fun(x): ... return x*x`
    - `r = map(fun, li)` # 第一个参数为函数
- 12-4：map与lambda
    - 上面代码修改：
    - `r = map(lambda x: x*x, li)`
    - 多个参数：（map提示：`*iter `**可变参数**）
    - `r = map(lambda x,y: x*x+y, li_x, li_y)`
- 12-5：reduce
    - `from functools import reduce`
    - `r = reduce(lambda x,y: x + y, li_x)` # 没有传入y
    - 与map区别：reduce连续计算，连续调用lambda（即一次传入两个计算，前两个计算结果作为x继续传入）
    - 问题：旅行者修改，经纬度(x, y)
    - 可以使用reduce计算（连续计算）
    - 直观观察：`li = ['1',...'5']``r = reduce(lambda x,y: x + y, li_x, 'aaa')`
    - google大数据计算框架map/reduce借鉴函数式编程思想
- 12-6：filter
    - 剔除list中的0值：
    - `r = filter(lambda x: True if x==1 else False, li)` `print(list(r))`
    - 特点：返回值需要表示真/假，0/1也行
    - 等同于`r = filter(lambda x: x, li)`
    - 过滤小写保留大写：
    - `r = filter()`
- 12-7：命令式编程vs函数式编程
    - 习惯于命令式编程，结合函数式编程（lisp语言）
    - python本质上并不是函数式编程
- 12-8：装饰器（一）
    - 地位：装饰器框架中用到最多
    - 类似：C#的特性，Java的注解
    - 举例：打印时间的函数
    - `import time`
    - `def f1(): ... print(time.time())` # unix 时间戳
    - 问题：如果有100个函数需要打印时间
    - 开闭原则：对修改是封闭的，对扩展是开放的
    - 修改1（缺点：定义的函数没有与原函数想关联）
    
- 12-9： 装饰器（二）
    - 使用装饰器修改：
    
- 12-10： 装饰器（三）
    - `@`：语法糖
    - 作用：没有改变原来调用方法（定义复杂，调用不能复杂）
    
     - 装饰器体现`AOP编程思想`
- 12-11：装饰器（四）
    - 通用方法，加入可变参数
    
 - 12-12：装饰器（五）
    - 如果调用的函数有关键字可变参数`**kw**`
    - 小技巧：支持各种参数的列表`(*args, **kw)`
    - 修改：
    
- 12-13：装饰器（六）
    - flask的装饰器：
    - `@api.route('/get', methods=['GET'])`
    - `@api.route('/psw', methods=['GET'])` 

### 13：实战-爬虫
- 13-1：分析抓取目的确定抓取页面
    - 目的：爬取熊猫TV主播人气排行
    - 数据：主播 + 观看人数
- 13-2：整理爬虫常规思路
    - 查看html：所需要的标签中的数据
    - 爬虫：搜索引擎、今日头条
    - 获取数据：正则表达式处理html标签
    - 代码注释：
    - 前期准备：
    - 1、明确目的
    - 2、找到数据对应的网页
    - 3、分析网页的结构找到数据所在的标签位置
    - 爬取数据：
    - 4、模拟HTTP请求，向服务器发送该请求，获取到服务器返回的html
    - 5、使用正则表达式提取所需数据（主播名称 + 观看人数）
- 13-3：VSCode中调试代码    
    - `from urllib import request` 
    - `class Spider()` `spider = Spider()` `spider.go()` 
        
    - 不推荐使用print查看变量
    - 推荐：断点调试
    - 思想：你可以什么都不会，但你必须得会断点调试（70%的问题都可以通过断点调试解决）
    - VSCode断点调试：
    - 1、加断点（小红点）
    - 2、`F5`键：调试运行
    - 3、`F10`键：单步运行
    - 4、`F5`键：继续，即跳到下一个断点
    - 5、`F11`键：进入函数
- 13-4：HTML结构分析基本原则
    - `htmls = str()` # 解决获取到的html是数字的问题
    - `def __analysis()`
    
    - 寻找标识符（标签）
    - 两个原则：
    - 1、尽量选取具有唯一标识的标签
    - 2、尽量提取接近所需数据的标签
- 13-5：数据提取层级及原则三
    - 第三个原则：
    - 3、尽量选取可以闭合的标签
    - 选择`<div class='video-info'></div>`标签：包含video-nickname和video-number
    
 
- 13-6：正则分析HTML
    - `root_pattern = '<div class="video-info">([\s\S])*?</div>'`
    - `import re`
    - `root_html = re.findall()`
    
    
    - 正则表达式：非贪婪匹配
    - error： htmls = None（原因，`__fetch_content 需要 return htmls`）
    - error：`\s`反斜杆不是/s！！！
- 13-7：正则分析获取名字和人数
    - 主播姓名：`name_pattern = '</i>([\s\S]*?)</span>'`
    - 观看人数：`number_pattern = '<span class="video-number">([\s\S]*?)</span>'`
    
     
- 13-8：数据精炼refine
    - `define __refine():`
     
    - error：NoneType in map(l,anchors) （`__analysis 需要 return anchors`）
- 13-9：sorted排序
    - `define __sort():`
    - `define __show():`
    - `define __sort_seed():`
    
     
     
    
    - **正则表达式**处理观看人数排序（9人，8000人，1.2万人）
- 13-10：案例总结
    - VSCode快速查找：`ctrl + shift + o` # @...
    - 建议数据库存储方法
    - 代码规范：主方法（`go()`函数）里面的函数是平级的，思路清晰
    
    - 注释规范：注释写在里面
    - `'''`：模块，类，方法
    - `#`：写在代码上面，上一行留空行
    - 善用空格增加代码块的概念
    - 小技巧：更改熊猫url最后的一个参数
    - 代码问题：代码复用性，业务更改时（如，不是熊猫TV）这个代码就用不了了
    - 爬虫框架：BeautifulSoup，Scrapy（框架能不用就不用）
    - 爬虫难点：爬虫 + 反爬虫 + 反反爬虫
    - 遇到问题：IP被封，使用代理IP解决

### 14：Pythonic与Python杂记
- 14-2：用字典映射代替switch case语句

- 14-4：字典的列表推导式
    - 列表存储：`b = [key for key,value in students.items()]`
    - 字典存储：`dic = {value:key for key,value in students.items()}`

- 14-5：iterator与generator
    - 元组的列表推导式即生成器：
    `t = (i for i in range(100))`













