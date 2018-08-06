写进程的时候 不要使用fork 而是用Process进行创建进程

p = Process(需要执行的函数,参数)

p.start() 用来将进程启动

p.join()可以等待子进程结束后在继续往下运行 



Process常用的几个方法:

- **is_alive()** 判断是不是还在运行
- **join{[timeout]}** 是不是等待进行实例结束执行 或者等待多少秒
- **run()** 当没有给定一个target的参数的时候 对这个对象调用start的时候就会执行对象中的run方法

- **terminate()**立即终止