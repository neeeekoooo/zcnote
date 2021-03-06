## 安装
- 下载jdk
- 设置环境变量`JAVA_HOME`为jdk安装目录地址
- 将`JAVA_HOME`的bin目录添加到环境变量，这样可以在任何目录执行java命令

## 运行java文件
- `javac`编译文件，后缀为`.class`
- `java`启动JVM执行编译后的文件
- `jar`将一组`.class`文件打包为一个`.jar`文件

`String`的默认类型为`null`

对象和数组是引用类型

通常用`final`修饰常量，例如`public final dobule PI = 3.14159`

前缀`0`表示 8 进制，而前缀`0x`代表 16 进制

- 类变量，也成为静态变量，被`static`修饰，通过`类.类变量`访问
- 实例变量
- 局部变量，局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。

## 修饰符
- `static`关键字用来声明独立于对象的静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝。 静态变量也被称为类变量。局部变量不能被声明为 static 变量
- `final`修饰符，用来修饰类、方法和变量，final 修饰的类不能够被继承，修饰的方法不能被继承类重新定义，修饰的变量为常量，是不可修改的
- `synchronized`，声明的方法同一时间只能被一个线程访问
- `transient`，序列化的对象包含被 transient 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量。
- `volatile`，修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值

`switch`语句表现和`php`一致

- `ceil` 向大取整
- `floor` 向小取整
- `round` 四舍五入，3.5是3，不是3.6

`finalize`类似于PHP的析构函数`____destruct（析构函数会在对象的所有引用被删除了时执行）`

## 声明自定义异常
- 如果希望写一个检查性异常类，则需要继承 Exception 类。
- 如果你想写一个运行时异常类，那么需要继承 RuntimeException 类。


