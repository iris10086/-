# Meta Object System

## QT为C++的Object做了扩展

C++类的缺陷。

> 不能向java一样获取对象具有的方法。

QT对其进行了扩展。

大多数Q开头的对象是基于QObject类(QT定义的基类)

## 添加了信号和槽

可以动态添加属性

## 属性系统

需要继承QObject类，使用Q_PROPERTY()宏。

通过查询QObject，QMetaObject，QMetaProperties，可以在<i>运行时</i>查询



> 通过QObject的setProperty函数可以通过字符串访问成员。
>
> 同时，要是setPeoperty访问的变量没有。就会新增一个变量。



## 动态类型转换



## 内存管理

QObject可以指定一个父对象。

父对象被回收时，子节点都会被回收。





# MOC 元对象编译器

在预编译之后调用MOC预处理器。

对每个添加了Q_Object宏的类进行处理。

自定义QObject类要添加宏才能使用QT框架的便利。







# 事件与信号

重写QMainWindow继承链中有关事件的方法，可以自定义事件逻辑。

继承链中的信号也可以自定义关联槽。



# 基本数据结构

## 容器

QList

QMap

QSet

和标准库差不多。

## 字符串

QString

标准库的输入输出流不支持QString的操作

所以用QTextStream

QString也支持类插槽。

使用%1，%2等占位符在输出时，使用arg()方法绑定数据。



## 日期和时间

QDate

构造方法 QDate(int year,int mouth,int day)

初始化方法 boolean setDate(int year,int mouth,int day) 返回日期是否合法

QTime 

构造方法 QTime(int h,int m,int s,int ms=0)

初始化方法 boolean QTime(int h,int m,int s,int ms=0)

toString可以指定格式化类型 

toString(hh:mm:ss)

QDateTime Date 和 Time 的结合。

有操作符重载实现date的大小比较。判断闰年等功能。

## 文件 QFile

与java File类似？

```c++
if(argc != 2){
    qWarning("报错了哦");
    return 1;
}

QString filename = argv[1]; // 通过命令行参数获取文件名。
QFile file(filename);
if(!file.exists()){
    qInfo()<<"文件不存在";
}
qInfo()<<"filename:"<<file.fileName();
```

一般配合QTextStream使用。

```C++
qInfo() << " ==读== ";
if(!file.open(QIODevice::ReadOnly)){
    qWarning("只读打开失败");
}
QTextStream f(&file);

while(!f.atEnd()){
    QString line = f.readLine();
    qInfo() << line;
}
qInfo()<< "==写==";
QFile f2("test.txt");
if(!f2.open(QIODevice::WriteOnly)){
    qWarning("只写打开失败");
    return 1;
}
QTextStream in(&f2);
in << "eihei" << Qt::endl;
in << "this is a test text.";
in << "new line ?";
```

