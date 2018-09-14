> php垃圾回收使用来引用计数的方法,php变量由zval结构体维护,zval结构体有refcount表示有多少个变量指向这个zval容器,is_ref用于区分引用变量,当unset一个变量时,会端断开变量到内存区域的链接,同时讲该区域的refcount引用计数减1,当refcount为0时,被当做垃圾,释放内存

#### 内存溢出
> php5.3版本之前会有内存溢出,主要是环形引用的问题(环形引用举个例子:数组一个元素的值复制为该数组的引用);现在php的gc是这样的,当refcount减1时,如果不为0,则添加到回收池,当回收池满了,会遍历所有变量以及变量下面每一项,模拟recount减1,如果变量整个refcount为0,表示为垃圾,可以回收

#### unset
unset只是断开一个变量到一块内存区域的链接,同时将该区域的引用计数-1,内存是否回收主要看refcount是否为0

#### 参考
[https://segmentfault.com/a/1190000016240169](https://segmentfault.com/a/1190000016240169)