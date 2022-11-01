# QFrame 框





# 布局

只有布局中才能添加组件。

框中可以添加布局。

## 绝对布局

直接设置几何坐标

```C++
w.setGeometry(0,0,100,100);
```

## 布局管理器

### 垂直布局

QVBoxLayout

上下填不满，要在子对象中设置sizePolicy 为Expanding

### 水平布局

QHBoxLayout

### 混合布局

QListWidget

### 表格布局

QFormLayout

### 网格布局

QGridLayout





# 样式

## Qt样式表

与CSS类似。可以运行时加载，纯文本格式。





# 透明边框、北京和移动窗口

```c++
Widget w;
w.setWindowFlag(Qt::FramelessWindowHint); // 无边框
w.setAttribute(Qt::WA_TranslucentBackground); // 透明背景

// 移动窗口
// QPoint是向量对象。
void Widget::mousePressEvent(QMouseEvent *event)
{
    QPoint mousePos = event->globalPosition().toPoint();
    QPoint topleft = this->geometry().topLeft();
    pos =topleft - mousePos;
}

void Widget::mouseReleaseEvent(QMouseEvent *event)
{

}

void Widget::mouseMoveEvent(QMouseEvent *event)
{
    QPoint mousePos = event->globalPosition().toPoint();
    QPoint move = mousePos + pos;
    this->move(move);
}
```

