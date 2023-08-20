---
categories:
  - 编程语言
  - Python
  - pyqt5
  - 1 初次使用PyQt5工具包编程
---
# 1 初次使用PyQt5工具包编程

[1一个简单的例子](1一个简单的例子.md)

[2 设置程序图标](2%20设置程序图标.md)

[3 悬停显示信息](3%20悬停显示信息.md)

[4 关闭窗口](4%20关闭窗口.md)

[5 消息窗口](5%20消息窗口.md)

[将窗口放到屏幕中心](将窗口放到屏幕中心.md)





首先引入两个库

```Python
from PyQt5 import QtWidgets
import sys
```


模板

```Python
app=QtWidgets.QApplication(sys.argv)
first_window=QtWidgets.QWidget()
...
first_window.show()
sys.exit(app.exec_())

```


常用方法

```Python
# 改变窗口大小 resize
first_window.resize(1000,750)
# 设置窗口的名字 setWindowTitle
first_window.setWindowTitle("my first project of pyqt")
# 设置窗口位置和大小 setGeometry
self.setGeometry(1700,300,1000,750)

```


图标

```Python
import sys
from PyQt5 import QtWidgets,QtGui

# 创建一个名为Icon的类，括号里的参数表示该类继承QtWidgets.QWidget类
class Icon(QtWidgets.QWidget):
    def __init__(self,parent = None):
        # 因此我们必须调用两个构造函数——Icon的构造函数和继承类QtGui.QWidget类的构造函数。
        # 在创建这个类的时候，init函数，表示，指init函数里的函数都会运行
        # 下面这是一个函数，意思是继承父类，是把父类的init函数运行一下。
        QtWidgets.QWidget.__init__(self,parent)

        self.setGeometry(1700,300,1000,750)
        self.setWindowTitle("图标")
        # setWindowIcon()方法用来设置程序图标，它需要一个QIcon类型的对象作为参数。
        # 运行self里的setWindowIcon函数，函数的参数是QtGui类的QIcon函数，QIcon的参数是图标的路径
        self.setWindowIcon(QtGui.QIcon(r'D:\flower_icon.png'))

app=QtWidgets.QApplication(sys.argv)
icon=Icon()
icon.show()
sys.exit(app.exec_())

```


所有的类在创建时都会先调用构造函数(python中就是__init__())将实例按照构造函数的操作先进行初始化，继承了其它类的类，在构造函数中，先要构造他的父类，在之前的程序中，我们都是直接调用父类的__init__()方法来完成父类的构造，

```Python
QtWidgets.QWidget.__init__(self,parent)

```


但是有一种更安全的方法就是用关键字super来完成。

```Python
super(MainWindow, self).init()
```


我们可以这样理解：super(MainWindow, self)首先找到MainWindow的父类（就是QtWidgets.QMainWindow），然后把类MainWindow的对象self转换为类QtWidgets.QMainWindow的对象，然后“被转换”的类A对象调用自己的__init__函数。

