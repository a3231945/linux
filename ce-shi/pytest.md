

# pytest 学习

### 一、pytest安装

#### 1.1 准备pytest环境

    python3 -m venv pytest-env
    
    source pytest-env/bin/source pytest-env/bin/activate

#### 1.2 安装pytest 软件

```
vim requirements.txt
pytest
pytest-html
pytest-xdist
pytest-ordering
pytest-rerunfailures
allure-pytest

pip install -r requirements.txt
```

#### 1.3 插件介绍

| 软件名称             | 用途                         | 备注 |
| -------------------- | ---------------------------- | ---- |
| pytest               | pytest 测试框架软件包        |      |
| pytest-html          | 生成html格式的自动化测试报告 |      |
| pytest-xdist         | 测试用例分布式执行，多cpu    |      |
| pytest-ordering      | 用于改变测试用例的执行顺序   |      |
| pytest-rerunfailures | 用例失败后重跑               |      |
| allure-pytest        | 用于生成美观的测试报告       |      |

#### 1.4 pytest 基本使用规则

- 模块名必须以test_开头 或者 _test 结尾
- 测试类必须以Test 开头，并且不能有init 方法
- 测试方法必须以test 开头

#### 1.5 pytes 命令参数介绍

| 参数       | 详细解释                            | 备注                          |
| ---------- | ----------------------------------- | ----------------------------- |
| -s         | 表示输出调试信息，包括print打印信息 |                               |
| -v         | 显示更详细的信息                    | 常见 -vs 一起使用             |
| -n         | 支持多线程（并行）或分布式运行      | pytest  -n=2 .                |
| --reruns   | 错误重试运行运行                    | pytest --reruns=2 .           |
| -x         | 只要有一个运行失败，测试停止        |                               |
| --max-fail | 指定失败最大数量，出现测试停止      | pytest --max-fail=2 .         |
| -k         | 指定测试用例中 包含的字符串 运行    | pytest -k 'one' .             |
| --html     | 生成html报告                        | pytest --html ./report.html . |

#### 1.6 pytest 执行测试用例的顺序

- 默认从上到下
- 通过 `@pytest.mark.run(order=1) ` 修改执行的顺序

### 二、简单使用

#### 2.1 pytest 单实例

##### 2.1.1 编辑文件

`vim test_demo1.py`

    def add(a,b):
        return a + b
    
    def test_add():
        assert add(1 ,2) == 4

##### 2.1.2 运行测试 `pytest`

#### 2.2 pytest 多实例

##### 2.2.1 编辑文件

`vim test demo2.py`

    def mul(a,b):
        return a - b
    
    def test_mul():
        assert mul(2,1) == 1

##### **2.2.2 运行测试 `pytest`**

>注意： 这里会运行当前目录下的所有test_*.py 或 *_test.py 测试用例

### 三、pytest 运行使用

#### 3.1 pytest 类的应用

**3.1.1 编辑实例**

`vim test_class_demo1.py`

```python
import pytest

class TestClass():
    def test_one(self):
        assert 1 == 1

		@pytest.mark.smoke2
    @pytest.mark.skip(reason="不想太2")
    def test_two(self):
        assert 2 == 2

    @pytest.mark.smoke
    def test_three(self):
        assert 3 == 3
```

3.1.2 运行测试用例`pytest -q test_class_demo1.py`
         

#### 3.2 python -m pytest 使用

    python -m pytest test_class_demo1.py
    
    等同于
        pytest -q test_class_demo1.py

#### 3.3 pytest 返回码详解
- 0 ：找到所有测试用例并测试通过
- 1 ：找到测试用例，但是部分测试用例失败
- 2 ：用户终端测试
- 3 ：执行过程中发生了内部错误
- 4 ：pytest命令使用错误
- 5 ：没有找到任何测试用例

#### 3.4 pytest 命令常见使用

##### 3.4.1 重试

    pytest --maxfail=3 

##### 3.4.2 指定文件运行

    pytest test_class_demo1.py

##### 3.4.3 指定目录运行

    pytest .

##### 3.4.4 指定类运行

    pytest test_class_demo1.py::TestClass

##### 3.4.5 指定方法运行

    pytest test_class_demo1.py::TestClass::test_one

##### 3.4.6 指定标记符运行

    pytest -m smoke
    
    pytest -m "smoke or smoke2"

##### 3.4.7 指定字符串运行

```
pytest -k 'two' .
```

##### 3.4.8 python 代码中运行pytest

```python
import pytest

pytest.main(["-q","test_class_demo1.py"])
```

##### 3.4.9 跳过指定测试用例

```
指定方法或类上添加
@pytest.mark.skip(reason="不想太2")

reason  跳过的原因，默认可以不用写


@pytest.mark.skipif(条件,reason="跳过原因")
实例：
  age = 16
  @pytest.mark.skipif(age<18,reason="未成年")

```



##### 3.4.10 pytest.ini 运行

```
pytest.ini 这个文件它是pytest单元测试框架中核心配置文件
1 - 位置： 一般存放项目的根目录
2 - 编码： 必须是ANSI
3 - 作用： 修改pytest 的默认行为
4 - 运行的规则： 不管是主函数的运行模式 还是命令行的运行模式都会去读取这个配置文件

vim pytest.ini
[pytest]
;指定参数
addopts = '-vs'
;测试用例路径
testpaths = '.'
;测试用例文件命名规则
python_files = test_*.py
;类规则
python_classes = Test*
;方法规则
python_functions = test
;markers 管理
markers = 
		smoke:冒烟
		smoke2:冒烟2



```



### 四、pytest 断言



### 五、前置处理、后置处理



### 六、参数化



### 七、数据驱动



### 八、pytest + allure + jenkins 集成环境

#### 7.1 安装pytest

#### 7.2 安装allure

#### 7.3 安装Jenkins 



