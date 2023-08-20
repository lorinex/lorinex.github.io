---
categories:
  - 编程语言
  - Python
  - pyqt5
  - 2 pyqt5中的菜单和工具栏
---
# 2 pyqt5中的菜单和工具栏

#### 主窗口

**QMainWindow 类**用来创建应用程序的主窗口。通过该类，我们可以创建一个包含状态栏、工具栏和菜单栏的经典应用程序框架。

#### 状态栏

```Python
import sys
from PyQt5 import QtWidgets

class MainWindow(QtWidgets.QMainWindow):
    def __init__(self):
        super(MainWindow,self).__init__()

        self.resize(1000,750)
        self.setWindowTitle("状态栏显示程序")
        self.statusBar().showMessage("就绪")

app=QtWidgets.QApplication(sys.argv)
main_window=MainWindow()
main_window.show()
sys.exit(app.exec_())
```


所有的类在创建时都会先调用构造函数(python中就是__init__())将实例按照构造函数的操作先进行初始化，继承了其它类的类，在构造函数中，先要构造他的父类，在之前的程序中，我们都是直接调用父类的__init__()方法来完成父类的构造，但是有一种更安全的方法就是用关键字super来完成。super(MainWindow, self).**init**()我们可以这样理解：super(MainWindow, self)首先找到MainWindow的父类（就是QtWidgets.QMainWindow），然后把类MainWindow的对象self转换为类QtWidgets.QMainWindow的对象，然后“被转换”的类A对象调用自己的__init__函数。考虑到super中只有指明子类的机制，因此，在多继承的类定义中，通常我们保留使用之前代码中的方法。对于之前用的方法为什么不安全，请参考网页http://www.cnblogs.com/lovemo1314/archive/2011/05/03/2035005.html

#### 菜单栏

```Python
# -*- coding: utf-8 -*-
"""菜单栏"""
import sys
from PyQt5 import QtWidgets, QtGui


class MainWindow(QtWidgets.QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        self.resize(1000, 750)
        self.setWindowTitle("菜单栏示例")

        exit_menu = QtWidgets.QAction(QtGui.QIcon(r"1.ico"), "退出", self)
        exit_menu.setShortcut("Ctrl+Q")
        exit_menu.setStatusTip("退出程序")
        exit_menu.triggered.connect(QtWidgets.qApp.quit)

        self.statusBar()

        menubar = self.menuBar()
        file = menubar.addMenu("文件")
        file.addAction(exit_menu)

app = QtWidgets.QApplication(sys.argv)
mainwindow = MainWindow()
mainwindow.show()
sys.exit(app.exec_())

```


首先我们使用 QMainWindow 类的 menuBar()方法创建一个菜单栏。然后使用 addMenu()方法添加一个菜单。最后我们把动作对象（这里是exit_menu）添加到 file 菜单中。

#### 工具栏

```Python
# -*- coding: utf-8 -*-
"""工具栏"""
import sys
from PyQt5 import QtWidgets, QtGui


class MainWindow(QtWidgets.QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        self.resize(1000, 750)
        self.setWindowTitle("工具栏示例")

        self.exit_menu = QtWidgets.QAction(QtGui.QIcon(r"D:\flower_icon.png"), "退出", self)
        self.exit_menu.setStatusTip("退出程序")
        self.exit_menu.setShortcut("Ctrl+Q")
        self.exit_menu.triggered.connect(QtWidgets.qApp.quit)

        self.toolbar = self.addToolBar("退出")
        self.toolbar.addAction(self.exit_menu)

app = QtWidgets.QApplication(sys.argv)
main_window = MainWindow()
main_window.show()
sys.exit(app.exec_())

```




#### 一个综合的例子

