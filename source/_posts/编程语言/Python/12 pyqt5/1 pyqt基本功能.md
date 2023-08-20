---
categories:
  - 编程语言
  - Python
  - 12 pyqt5
---
# 2.1 一个简单的例子
```python
# -*- coding: utf-8 -*-  
"""第一个程序"""  
from PyQt5 import QtWidgets  
import sys  
app=QtWidgets.QApplication(sys.argv)  
# 每一个PyQt5程序都需要有一个application对象，application类包含在QtGui模块中。
# sys.argv参数是一个命令行参数列表。Python脚本可以从shell中执行，参数可以让我们选择启动脚本的方式。  
first_window=QtWidgets.QWidget()  
first_window.resize(400,300)  
first_window.setWindowTitle("my first project of pyqt")  
first_window.show()  
sys.exit(app.exec_())  
# 最后我们进入该程序的主循环。事件处理从本行语句开始。主循环接受事件消息并将其分发给程序的各个部件。如果调用exit()或主部件被销毁，主循环就会结束。使用sys.exit()方法退出可以确保程序可以完整的结束，这种情况下系统的环境变量会记录程序是如何退出的。  
# 也许你会疑惑，为什么exec_()方法会有一个下划线。这是因为exec是Python的关键字，为避免冲突，PyQt使用exec_()替代。
```
