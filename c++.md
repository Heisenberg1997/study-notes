### c++智能指针
***
- scoped_ptr 最经常使用的智能指针,在new后不用自己来delete一般在离开作用后会自行释放.
属于boost库里面的东西
#include<boost/smart_ptr.hpp>
如:
```
int* p = new int;

   scoped_ptr<int> scoped_int_ptr(p);
```
但是不允许进行赋值操作,就是没有
```
 scoped_ptr<int> scoped_int_ptr2 =  scoped_int_ptr;
```
这种操作!!

- auto_ptr
    和scoped的用法基本类似,但是多了指针传递和构造等操作!!
- shared_ptr




***
explicit 关键字 可以阻止不经过允许的隐式转换的发生.
***
编译过程的几个步骤:
    配置:放在一个配置文件里面,一般用来放一些文件库的位置等信息
    查找标准库的位置:
    确定依赖关系:
    头文件的预编译:
    预处理:
    编译:生成机器码,但是仍无法识别 
    连接:将外部函数的添加到可执行文件中 静态连接和动态拦截两种
    连接在内存中进行,就是在内存中生成了可执行文件 然后将可执行文件存放到指定的位置.