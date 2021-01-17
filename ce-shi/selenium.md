# selenium教程

### 一、安装

#### 1.1 python安装selenium 模块

```
pip install selenium
```

#### 1.2 安装chrome driver

- 下载地址
  - http://chromedriver.storage.googleapis.com/
  - http://npm.taobao.org/mirrors/chromedriver/
- 确认chrome版本
  - 浏览器窗口 输入 `chrome://version`
- 其他驱动地址：
  - 火狐：https://github.com/mozilla/geckodriver/releases/
  - IE:http://selenium-release.storage.googleapis.com/index.html

#### 1.3 简单使用

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys


driver = webdriver.Chrome("./chromedriver.exe")
driver.get("http://www.python.org")
assert "Python" in driver.title
elem = driver.find_element_by_name("q")
elem.send_keys("pycon")
elem.send_keys(Keys.RETURN)
assert "No results found." not in driver.page_source
driver.close()
```

### 二、基本使用

#### 2.1 打开连接

| 方法名                          | 解释                     | 备注                                |
| ------------------------------- | ------------------------ | ----------------------------------- |
| webdriver.Chrome()              | 映射本地对应的chrome驱动 | 类似的还有 webdriver.Firefox() 等等 |
| webdriver.get()                 | 打开指定网站             |                                     |
| webdriver.close()               | 退出当前页面             |                                     |
| webdriver.quit()                | 关闭浏览器               |                                     |
| webdriver.forward()             | 浏览器前进               |                                     |
| webdriver.back()                | 浏览器后腿               |                                     |
| webdriver.page_source           | 返回源代码               |                                     |
| webdriver.current_url           | 返回当前url              |                                     |
| webdriver.title                 | 返回title                |                                     |
| webdriver.get_window_size()     | 获取窗口大小             |                                     |
| webdriver.set_window_size(x, y) | 设置窗口大小             |                                     |
| webdriver.maximize_window()     | 窗口最大化               |                                     |



```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

driver = webdriver.Chrome('./chromedriver.exe')
driver.get('http://www.baidu.com')
print("当前源代码：",driver.page_source)
print("当前url：",driver.current_url)
print("当前title：",driver.title)
print("窗口大小：",get_window_size())
driver.close()
driver.quit()
```

#### 2.2 设置cookie

| 方法名                         | 解释           | 备注                                                         |
| ------------------------------ | -------------- | ------------------------------------------------------------ |
| webdriver.driver.get_cookies() | 获取cookie     |                                                              |
| webdriver.driver.add_cookie()  | 添加cookie     | 其参数是一个字典，字典中必须有“name”和“value”两个key，可选的key是"path"、 "domin"、 "secure"、 "expiry"、"httponly" |
| webdriver.delete_cookie()      | 删除cookie     |                                                              |
| webdriver.delete_all_cookies() | 删除所有cookie |                                                              |



- cookie 绕过登录

```python
import time
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome('./chromedriver.exe')

driver.implicitly_wait(30)
cookies = {"id": [
    {
        'name': '_eeos_uid',
        'value': 'xxxxxx'
    },
    {
        'name': '_eeos_traffic',
        'value': 'xxxxxx'
    },
    {
        'name': '_eeos_useraccount',
        'value': 'xxxxxx'
    }
]}

driver.maximize_window()
driver.get('http://xxxx/cn/login.html')

for i in cookies["id"]:
    print(i)
    driver.add_cookie(i)

element = driver.find_element_by_xpath(
    '//*[@id="app"]/div/div[1]/div/ul/li[10]/a')
element.click()

element = driver.find_element_by_xpath(
    '/html/body/div[1]/div/div[3]/div/div[2]/div/div[3]/div/button[1]/span')
element.click()

element = driver.find_element_by_xpath(
    '/html/body/div[1]/div/div[2]/div/ul/li[3]/div[2]/ul/li[1]/span')
element.click()

driver.quit()

```



#### 2.3 历史记录和定位

### 三、元素

#### 3.1 元素定位

| 方法名                                       | 解释                 | 备注 |
| -------------------------------------------- | -------------------- | ---- |
| webdriver.find_element_by_id()               | 指定id查找元素       |      |
| webdriver.find_element_by_name()             | 指定name查找元素     |      |
| webdriver.find_element_by_xpath()            | 指定xpath查找元素    |      |
| webdriver.find_elements_by_class_name        | 指定类名查找         |      |
| webdriver.find_elements_by_link_text         | 指定文本查找         |      |
| webdriver.find_elements_by_partial_link_text | 指定文本查找（模糊） |      |
| webdriver.find_elements_by_tag_name          | 指定标签查找         |      |

- 获取xpath 方法
  - F12 开启 开发者模式
  - crlt + shift + C  ， 鼠标点击元素框( 或者鼠标点击调试框箭头)
  - 右键- >copy -> copy Xpath

**百度搜索框实例：**`<input type="text" class="s_ipt" name="wd" id="kw" maxlength="100" autocomplete="off">`

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

driver = webdriver.Chrome('./chromedriver.exe')
driver.get('http://www.baidu.com')
# print("当前源代码：",driver.page_source)
# print("当前url：",driver.current_url)

#通过id定位元素
element = driver.find_element_by_id("kw")
element.clear()
element.send_keys("python")
element.click()
time.sleep(3)

driver.get('https://www.taobao.com/')
#通过name定位元素
driver.back()
element = driver.find_element_by_name("wd")
element.send_keys("java")
element.click()
time.sleep(3)

driver.get('https://www.taobao.com/')
#通过xpath定位元素
driver.back()
element = driver.find_element_by_xpath('//*[@id="kw"]')
element.send_keys("c++")
element.click()
time.sleep(3)


driver.close()
driver.quit()
```

#### 3.2 CSS 元素定位

| 方法名                                    | 解释            | 备注 |
| ----------------------------------------- | --------------- | ---- |
| webdriver.find_element_by_css_selector()  | 选择单个css元素 |      |
| webdriver.find_elements_by_css_selector() | 选择多个css元素 |      |

- css 定位的方式：
  - id 定位：#kw
  - class定位: .s_ipt
  - 属性定位:autocomplete="off"

```python
import time

from selenium import webdriver
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome('./chromedriver.exe')
driver.get('http://www.baidu.com')

# id 定位
element = driver.find_element_by_css_selector('#kw')

#class 定位
#element = driver.find_element_by_css_selector('.s_ipt')

#属性定位
#element = driver.find_element_by_css_selector('[autocomplete="off"]')


element.clear()
element.send_keys('python')
element.click()

time.sleep(3)
driver.close()
driver.quit()
```



#### 3.3 元素操作

| 方法名                        | 解释               | 备注 |
| ----------------------------- | ------------------ | ---- |
| webdriver.element.clear()     | 清除文本框或文本域 |      |
| webdriver.element.send_keys() | 模拟键盘输入内容   |      |
| webdriver.element.click()     | 点击提交按钮       |      |

**常用方法：**

```
#获取元素文本内容
element = wd.find_element_by_id("kw")
print(element.text)


#获取元素属性
element = webdriver.find_element_by_id("kw")
print(element.get_attribute('class'))

#获取整个元素对应的html
element = webdriver.find_element_by_id("kw")
element.get_attribute('outerHTML')		#整个元素的html
element.get_attribute('innerHTML')		#元素内部包含的html

#获取输入框里面的文字
element = webdriver.find_element_by_id("kw")
print(element.get_attribute('value')) # 获取输入框中的文本
print(element.get_attribute('innerText'))
print(element.get_attribute('textContent'))

```

#### 3.4 其他方法

| 方法名               | 解释                                                         | 备注 |
| -------------------- | ------------------------------------------------------------ | ---- |
| is_displayed()       | 元素对用户是否可见                                           |      |
| is_enabled()         | 元素是否可用                                                 |      |
| is_selected()        | 元素是否被选中,可用来检测单选或者复选按钮是否被选中          |      |
| screenshot(filename) | 取当前元素的截图，有IOError会返回`False`,文件名要包含完整路径 |      |
| get_attribute(name)  | 返回元素指定的属性                                           |      |
| size                 | 元素size                                                     |      |
| tag_name             | 元素的标签名                                                 |      |
| text                 | 元素的text 内容                                              |      |



### 四、窗口切换

#### 4.1 frame切换

| 方法名                                | 解释            | 备注 |
| ------------------------------------- | --------------- | ---- |
| webdriver.switch_to.frame()           | 切换到frame页面 |      |
| webdriver.switch_to.parent_frame()    | 回到父框架      |      |
| webdriver.switch_to_default_content() | 跳到最外层页面  |      |

- 登录公司后台

```python
import time
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome('./chromedriver.exe')

driver.get('xxxx')
driver.implicitly_wait(10)
driver.maximize_window()

#输入用户名
driver.find_element_by_xpath('//*[@id="loginform"]/p[2]/input').send_keys("xxxx")

#输入密码
driver.find_element_by_xpath('//*[@id="loginform"]/p[4]/input').send_keys("xxxx")

#点击登录
driver.find_element_by_xpath('//*[@id="loginform"]/p[5]/input').click()

#选择监课管理
driver.find_element_by_xpath('//*[@id="header_133"]').click()

#选择新监课管理
driver.find_element_by_xpath('//*[@id="menu_133"]/li/a').click()


#切换至第一层iframe
element = driver.find_element_by_xpath('//*[@id="main"]')
driver.switch_to_frame(element)

#切换至第二层嵌套的iframe
element = driver.find_element_by_xpath('/html/body/iframe')
driver.switch_to_frame(element)

#输入123查找
driver.find_element_by_css_selector('[placeholder="课节名称"]').send_keys('123')

#回到最外层框架
driver.switch_to_default_content()

# #回到第二层 iframe
# driver.switch_to.parent_frame() 

# #回到父框架
# driver.switch_to.parent_frame() 

#点击主页
driver.find_element_by_xpath('//*[@id="header_home"]').click()

#点击退出
driver.find_element_by_xpath('/html/body/table/tbody/tr[1]/td/div/div[2]/p[1]/a[2]').click()


driver.quit()
```

> 注意：这里有多层iframe 嵌套，需要切换多次



#### 4.2 windows切换

| 方法名                       | 解释         | 备注 |
| ---------------------------- | ------------ | ---- |
| webdriver.switch_to.window() | 切换到新站点 |      |



- 实例： 打开百度，切换窗口，再打开腾讯

```python
import time
from selenium import webdriver
from selenium.webdriver.common.keys import Keys


driver = webdriver.Chrome('./chromedriver.exe')

driver.implicitly_wait(10)

#打开百度
driver.get('http://www.baidu.com')

#打开新的标签页
driver.execute_script('window.open()')

#切换窗口
driver.switch_to_window(driver.window_handles[1])

#打开腾讯
driver.get('http://www.qq.com')

time.sleep(2)
driver.quit()

```



#### 4.3 弹窗

| 方法名 | 解释 | 备注 |
| ------ | ---- | ---- |
|        |      |      |



#### 4.4 下拉框



#### 4.5 文件上传



#### 4.6 截图



### 五、等待事件

>为什么要wait?

```
现在很多web应用都在使用ajax 技术，浏览器加载一个页面时，页面内的元素可能是在不同的时间载入的，会导致页面未加载完，定位元素失败
```



#### 5.1 显示wait

| 方法名                            | 解释                       | 备注 |
| --------------------------------- | -------------------------- | ---- |
| webdriver.WebDriverWait().until() | 等待一个条件触发，直到超时 |      |

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

driver = webdriver.Chrome('./chromedriver.exe')
driver.get('http://www.baidu.com')
try:
    element = WebDriverWait(driver,10).until(
    	EC.presence_of_element_located((By.ID,"kw"))
    )
finally:
    driver.quit()

element.send_keys("python")
element.click()


driver.close()
driver.quit()
```



#### 5.2 隐式wait

| 方法名                   | 解释                            | 备注 |
| ------------------------ | ------------------------------- | ---- |
| driver.implicitly_wait() | 指定查找元素的轮训次数，默认是0 |      |

```python
from selenium import webdriver

driver = webdriver.Chrome("./chromedriver.exe")
driver.implicitly_wait(10)
driver.get('http://www.baidu.com')

element = driver.find_element_by_id("kw")
element.send_keys("eeo")

element.click()

driver.quit()
```



#### 5.3 人工wait

- 利用` time.sleep() ` 等待，不建议

### 六、页面对象

- 官方文档提供：面向对象的设计模型 的实现
- 好处：*可以写出能在多个测试案例里复用的代码* 减少重复代码 * 如果用户接口更改，只需要在一个地方做相应修改即可

#### 6.1 文件描述

- 页面对象类：page.py
- 页面元素：element.py
- 定位器：locators.py
- 测试用例：test.py

#### 6.2 具体实现

##### 6.2.1 页面对象类

```python

```



##### 6.2.2 页面元素

```python

```



##### 6.2.3 定位器

```python

```



##### 6.2.4 测试用例

```python

```



### 七、行为模拟

#### 7.1 鼠标事件

| 方法名                                      | 解释                                                         | 备注 |
| ------------------------------------------- | ------------------------------------------------------------ | ---- |
|                                             | 点击一个元素                                                 |      |
|                                             | 鼠标左键点击一个元素并且保持                                 |      |
|                                             | 双击一个元素                                                 |      |
|                                             | 鼠标左键点击`source`元素，然后移动到`target`元素释放鼠标按键 |      |
|                                             | 拖拽目标元素到指定的偏移点释放                               |      |
|                                             | 只按下键盘，不释放。我们应该只对那些功能键使用(Contril,Alt,Shift) |      |
|                                             | 将当前鼠标的位置进行移动                                     |      |
|                                             | 把鼠标移到一个元素的中间                                     |      |
|                                             | 鼠标移动到元素的指定位置，偏移量以元素的左上角为基准         |      |
|                                             | 执行所有存储的动作                                           |      |
|                                             | 释放一个元素上的鼠标按键                                     |      |
| send_keys(*keys_to_send)                    | 向当前的焦点元素发送键                                       |      |
| send_keys_to_element(element,*keys_to_send) | 向指定的元素发送键                                           |      |

#### 7.2 键盘事件



### 八、异常

| 异常报错信息                               | 报错场景                                                     |
| ------------------------------------------ | ------------------------------------------------------------ |
| exceptions.ElementNotSelectableException   | 当试图选中一个不能选中的元素时抛出 例如，选中一个`script`元素 |
| exceptions.ElementNotVisibleException      | 当DOM上存在元素但是不可用时，它是不可以进行交互的，最常见的场景是试图点击或者阅读一个隐藏的元素 |
| exceptions.ErrorInResponseException        | 服务端发生错误，这个异常可能会在 和 firefox扩展或者 远程驱动服务交互时产生 |
| exceptions.ImeActivationFailedException    | 激活一个 IME引擎失败                                         |
| exceptions.ImeNotAvailableException        | IME支持不可用。 如果 机器上IME支持不可用，这个异常会在所有和IME相关的方法里抛出 |
| exceptions.InvalidCookieDomainException    | 试图在一个和当前不同的域名下添加cookie                       |
| exceptions.InvalidSelectorException        | 选择器用来寻找元素，但返回的不是一个 WebElement时。 目前只会在XPath表达式选择器里产生，XPath表达式语法错误或者没有选择WebElement时 |
| exceptions.InvalidSwitchToTargetException  | 要切换的窗口或者框架不存在时                                 |
| exceptions.NoAlertPresentException         | 屏幕没有警告框时，切换到警告框                               |
| exceptions.NoSuchAttributeException        | 元素找不到这个属性，你可能会想在另外一个浏览器上检查某个属性是否存在，有些浏览器相同的属性有不同的属性名（IE8的 innerText和 Firefox的 textContent） |
| exceptions.NoSuchElementException          | 找不到元素                                                   |
| exceptions.NoSuchFrameException            | 要切换的目标框架不存在                                       |
| exceptions.NoSuchWindowException           | 要切换的目标窗口不存在，要找到当前活动窗口的句柄，你可以`print driver.window_handles`方法来获取一个句柄列表 |
| exceptions.TimeoutException                | 规定时间内一个命令没有执行完                                 |
| exceptions.UnableToSetCookieException      | 驱动设置cookie失败                                           |
| exceptions.UnexpectedAlertPresentException | 预料之外的警告框。当一个警告框阻塞了webdriver，不能执行任何命令的时候 |
| exceptions.UnexpectedTagNameException      | 当一个支持的类没有拿到预料的web元素时                        |
| exceptions.WebDriverException              | webdriver 异常                                               |

