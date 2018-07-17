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