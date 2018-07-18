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

.a文件实在编译的时候进行将函数连接成可执行文件后存在的形式,就是静态库.静态库是程序在运行时所需要的所有的库都已经或得到了,而在程序运行的时候仍然对库进行访问的为动态库,动态库在访问的时候首先会让操作系统查看内存中是不是存在着正在运行的库 如果有直接拷贝,能够极大的减少内存空间的使用.

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