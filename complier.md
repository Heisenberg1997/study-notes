### 编译原理

***

五个阶段：

- 词法分析
- 语法分析
- 语义分析和中间代码生成
- 优化
- 目标代码生成

**词法分析**

输入源程序，识别出一个个单词，描述语法规则的有限工具是正规式和有限自动机



**语法分析**：

在词的基础上进行进行层次上的分析







所有的数据一般都以表格的形式进行存储



前三部分称为编译前端，后面两个称为编译后端



.so是Linux里面的一个动态链接库,相当于windows下的dll文件

.a文件在编译的时候进行将函数连接成可执行文件后存在的形式,就是静态库.静态库是程序在运行时所需要的所有的库都已经或得到了,而在程序运行的时候仍然对库进行访问的为动态库,动态库在访问的时候首先会让操作系统查看内存中是不是存在着正在运行的库 如果有直接拷贝,能够极大的减少内存空间的使用.

头文件一般只包括这个函数的名字,一般没有函数的具体实现,而具体的实现包含在库中,而库一般是二进制存在的文件,一般不会对外进行开放,而开源的文件则可以看到.

***

一般在执行了catkinmake之后会子啊程序里面发生什么????

首先会找到住cmakelist 找到文件的位置 之后调用catkin workspace函数进行软件包的定位和翻译 软件包的确定只有package来确定,之后使用其内部的cmakelist进行软件包的便宜

cmakelist内容:

版本 项目名字 依赖

依赖一般使用findpackage 使用了外部库 而且不知道外部库的位置就是没有定义一个path

如果找到了 则可以添加include_directories(<NAME>__INCLUDE_DIRS) 来包含库的头文件，添加target_link_libraries(源文件   <NAME>_LIBRARIES)命令将源文件与库文件链接起来。

也就是添加好可执行文件 之后使用findpkg找到库 之后如果找到了 使用include lib 和targrtlinklib

参数(文件名 是否必须 需要的附件)

编译出来的内容:

`bin`内是被编译的可执行文件。`lib`是`pkg.pc`文件和python的库文件。`pkg.pc`是`pkg-config`的配置文件。 `include`用来放置头文件。 `share`是放置生成的`pkgConfig.cmake`文件的，在cmake文件中`find_package`就会用到这些文件。下面是其中的一个例子。



整个编译过程:

1. 执行`catkin_make`

2. 执行`catkin_workspace`。解析`catkin_make`的参数同时遍历整个工作空间把所有的有`package.xml`的文件夹添加进软件包列表里面。对每个软件包执行`add_subdirectory`

3. 执行每个软件包内部的`CMakeList.txt`文件。

4. 执行 `catkin_package`。解析`package.xml`文件，载入对应的参数。根据依赖参数，载入对应的软件包参数。根据载入参数生成当前软件包的配置文件。\

   cmake

   ***

#### cmake:

     程序在cmake编译是这样的流程, cmake指令依据你的CMakeLists.txt 文件,生成makefiles文件,make再依据此makefiles文件编译链接生成可执行文件. 



- **find_package()**  	查找外部库的位置 一般不知道外部库(头文件库和链接库)的位置 查找模块和包的路径.之后包含头文件

- **INCLUDE_DIRECTORIES** 添加头文件目录 **使用这个将找到的包内的头文件目录包含进去,然后在添加lib的时候可以不用直接制定绝对目录而使用名字 就直接在目录里面搜索了.**

- **LINK_LIBRARIES** 添加库 这个是在目录添加完之后进行库的添加,可以**指定路径或者使用包名字** 包含进去之后在生成目标文件的时候不一定需要使用

- **ADD_LIBRARY** 和上面那个的区别 上面那个只需要指定lib 一般就是一个lib变量,而自动会去目录里面去寻找那些lib 而这个需要**指定文件在哪里** 然后添加成lib

- **TARGET_LINK_LIBRARIES**  这个是将之前的东西全部包含进去 就是那些库文件.

- **set** 对一些环境变量进行复制 一般用来指定需要的内容和路径什么的

- **file** 有许多的参数进行选择,常用的如GLOB进行宏定义 相当于将后面的添加的文件形成一个list组合到给出的名字变量里面 如果为一个表达式指定了**RELATIVE**标志，返回的结果将会是相对于给定路径的相对路径。

  ​	<参考: https://www.cnblogs.com/dverdon/p/4574221.htmlcmake命令>

  

  **add execute和target link libraries的区别 库一般是自己编写的内容中所涉及的函数之类的东西,而可执行文件就是自己编写的内容 将库中的函数或者变量组装的东西.!!!**

  所以add execute 一般后面的参数就是自己写的cpp或者其他的路径 而target的一般就是库的路径(或者宏)

  

  ***

core 文件可以使用gdb进行调试 core文件是程序非法执行的时候产生的

当程序遇到设置的断点时会运行结束,等待输入 就是输入gdb的交互命令

gdb交互命令:

***

运行

- run (r)  continue(c) next(n) step(s) quit(q) 

设置断点

- bread n 第n行 
- break func 函数入口
- delete n 删除第n个断点
- delete breakpoints 删除所有断点



当足够熟练的时候可以使用 **cgdb**

***

ldd 命令 用来将程序的依赖列出来