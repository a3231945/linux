# pywinauto教程

### 一、安装

#### 1.1 程序安装要求

- 安装python3`略`
- 安装pywinauto 模块

```
pip install pywinauto
```

#### 1.2 确认引用程序的可访问技术

支持控件的访问技术：pywinauto的后端

- Win32 API(backend = "win32") - 默认的backend
  - MFC,VB6,VCL,简单的WinForms控件和大多数旧的应用程序
- MS UI Automation API( backend=“uia”)
  - WinForms,WPF,Store apps,Qt5,浏览器

#### 1.3 确认程序的类型

- 单进程（pywinauto.Application）
- 多进程（pywinauto.Desktop）

#### 1.4 类似GUI自动化工具

- Pyautogui
- Lackey
- AXUI
- winGUIauto

### 二、辅助程序检查工具的使用

#### 2.1 inspect.exe

windows 自带工具，win8 以上系统自带，工具存放在`c:\Program Files(x86)\Windows Kits\10\bin\x64`

#### 2.2 spy++.exe

使用Win32 API ，如果Spy++ 能够显示程序的所有控件，那么该程序适合使用win32的backend

#### 2.3 ViewWizard

ViewWizard(窗口信息查看精灵)，使用起来非常简洁可查看窗口和控件句柄、类名、标题、风格等信息



> 使用原则：
>
> 1、那个识别的控件足够多，建议使用谁
>
> 2、优先使用pywinauto 识别出来的控件名

### 三、窗口管理

#### 3.1 打开程序

| 方法                    | 解释               | 备注                           |
| ----------------------- | ------------------ | ------------------------------ |
| Application().start()   | 打开指定的程序     | 传参：后端控件类型，程序的路径 |
| Application().connect() | 连接已经打开的程序 | 可以基于 pid 、文件句柄        |



##### 3.1.1 打开windows 记事本

```python
from pywinauto.application import Application

app = Application(backend="uia").start("notepad.exe")
```

##### 3.1.2 打开其他程序

```python
from pywinauto.application import Application


#打开classin
app = Application(backend="uia").start(r'"D:\Program Files (x86)\ClassIn\ClassIn.exe"')

```

##### 3.1.3 打开已经打开的应用程序

```python
from pywinauto.application import Application


#通过pid 连接已经打开的程序
app = Application("uia").connect(process=29584)
print(app)


#通过窗口句柄连接
app = Application('uia').connect(handle=23682260)
print(app)
```



#### 3.2 打开窗口

| 方法                            | 解释                 | 备注 |
| ------------------------------- | -------------------- | ---- |
| dlg.print_control_identifiers() | 打印当前窗口所有控件 |      |



```python
from pywinauto.application import Application

app = Application("uia").start("notepad.exe")


# 方式一: app[类名/标题]
#使用类名来选择窗口
# dlg = app["Dialog"]

#使用窗口的标题来选择
# dlg  = app["无标题 - 记事本"]

#方式二:
dlg = app.Dialog

#打印窗口所有控件
dlg.print_control_identifiers()

```

> 注意：推荐使用方式一，当窗口标题包含 中文，或者是 空格时，使用方式二无法正常处理

#### 3.3 窗口的操作

| 方法                 | 解释             | 备注                   |
| -------------------- | ---------------- | ---------------------- |
| dlg.maximinze()      | 窗口最大化       |                        |
| dlg.minimize()       | 窗口最小化       | 和其他窗口混用会有问题 |
| dlg.restore()        | 还原窗口正常大小 |                        |
| dlg.get_show_state() | 获取窗口显示状态 |                        |
| dlg.close()          | 关闭窗口         |                        |
| dlg.rectangle()      | 获取窗口坐标     |                        |
| dlg.draw_outline()   | 给窗口画边线     |                        |



```python
from pywinauto.application import Application

app = Application("uia").start("notepad.exe")

# 方式一: app[类名/标题]
#使用类名来选择窗口
dlg = app["Dialog"]

#窗口最大化
dlg.maximize()

#窗口最小化
# dlg.minimize()

#窗口恢复正常大小
dlg.restore()

#查看窗口当前状态 最大化：1 正常大小：0
print(dlg.get_show_state())

#查看窗口坐标
print(dlg.rectangle())

#关闭窗口
dlg.close()
```



> 注意:
>
>   1. 最小化 后， 无法调用 dlg.retore() 恢复正常大小
>
>   2. 窗口的坐标是以左上角为顶点 x + y 坐标，定位。
>
>      (L204, T129, R1596, B909) 表示，
>
>      ​		左边线离 左上角 204 像素
>
>      ​		右边线离 左上角 1596 像素
>
>      ​        上边线离 左上角 129  像素
>
>      ​        下边线离 左上角 909  像素

### 四、控件管理

| 控件名      | 含义           |      |
| ----------- | -------------- | ---- |
| StatusBar   | 状态栏         |      |
| Button      | 按钮           |      |
| RadioButton | 单选框         |      |
| ComboBox    | 组合框         |      |
| Edit        | 编辑栏         |      |
| ListBox     | 列表框         |      |
| PopupMenu   | 弹出菜单       |      |
| Toolbar     | 工具栏         |      |
| Tree View   | 树状视图       |      |
| MeunItem    | 菜单项         |      |
| Static      | 静态内容       |      |
| CheckBox    | 复选框         |      |
| GroupBox    | 组框           |      |
| Dialog      | 对话框（窗口） |      |
| Header      | 头部内容       |      |
| ListView    | 列表显示控件   |      |
| TabControl  | 选项卡空间     |      |
| ToolTips    | 工具提示       |      |
| Menu        | 菜单           |      |
| Pane        | 窗格           |      |



#### 4.1 选择控件

```python
#与打开窗口方式相同

```



#### 4.1 控件属性获取



#### 4.2  窗口控件

#### 4.3 菜单控件

#### 4.4 编辑类型控件

#### 4.5 控件及窗口的截图

### 五、等待机制

### 六、键盘操作

### 七、鼠标操作



