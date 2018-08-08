写进程的时候 不要使用fork 而是用Process进行创建进程

p = Process(需要执行的函数,参数)

p.start() 用来将进程启动

p.join()可以等待子进程结束后在继续往下运行 



Process常用的几个方法:

- **is_alive()** 判断是不是还在运行
- **join{[timeout]}** 是不是等待进行实例结束执行 或者等待多少秒
- **run()** 当没有给定一个target的参数的时候 对这个对象调用start的时候就会执行对象中的run方法

- **terminate()**立即终止

当创造一个新的类的时候 这个类实际上以一个对象保存,就是这个对象有创建对象的能力





```python
def choose_class(name):

...:    if name == 'foo':

...:        class Foo(object):

...:            pass

...:        return Foo #返回的是类,不是类的实例

...:    else:

...:        class Bar(object):

...:            pass

...:        return Bar

...:

MyClass=choose_class('foo')

print(MyClass)#函数返回的是类，不是类的实例

<class '__main__.choose_class.<locals>.Foo'>

print(MyClass())#你可以通过这个类创建类实例，也就是对象

<main.choose_class.<locals>.Foo object at 0x000001C54F7007B8>

```

type查看类 查看数据的类名 如果查看一个类的对象 那就是类名 如果直接查看一个类 那就返回type

还可以用来更加动态的创建对象 test2= type("test2",(),{})

```python
Foo = type('Foo',(),{'bar':True})
```

相当于:

class Foo(object):

​	bar = True

可以这么想  远类就是用来构造类的一个类

所以type 可以用来去构造一个元类