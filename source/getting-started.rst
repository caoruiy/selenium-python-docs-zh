.. _getting-started:

快速入门
========

简单用例
~~~~~~~~~~~~

如果你已经安装好了selenium，你可以把下面的python代码拷贝到你的编辑器中

::

  from selenium import webdriver
  from selenium.webdriver.common.keys import Keys

  driver = webdriver.Firefox()
  driver.get("http://www.python.org")
  assert "Python" in driver.title
  elem = driver.find_element_by_name("q")
  elem.clear()
  elem.send_keys("pycon")
  elem.send_keys(Keys.RETURN)
  assert "No results found." not in driver.page_source
  driver.close()

上面的脚本可以保存到一个文件（如：-
`python_org_search.py`），那么可以这样使用

::

  python python_org_search.py

你运行的 `python` 环境中应该已经安装了 `selenium` 模块。

示例详解
~~~~~~~~~~~~~~~~~

`selenium.webdriver` 模块提供了所有WebDriver的实现，
当前支持的WebDriver有： Firefox, Chrome, IE and Remote。
`Keys`类提供键盘按键的支持，比如：RETURN, F1, ALT等

::

  from selenium import webdriver
  from selenium.webdriver.common.keys import Keys

接下来，创建一个Firefox WebDriver的实例

::

  driver = webdriver.Firefox()

`driver.get` 方法将打开URL中填写的地址，WebDriver 将等待，
直到页面完全加载完毕（其实是等到"onload" 方法执行完毕），然后返回继续执行你的脚本。
值得注意的是，如果你的页面使用了大量的Ajax加载，
WebDriver可能不知道什么时候页面已经完全加载::

  driver.get("http://www.python.org")

下一行是用assert的方式确认标题是否包含“Python”一词。
(译注：assert 语句将会在之后的语句返回false后抛出异常，详细内容可以自行百度)

::

  assert "Python" in driver.title

WebDriver 提供了大量的方法让你去查询页面中的元素，这些方法形如： `find_element_by_*`。 
例如：包含 `name` 属性的input输入框可以通过 `find_element_by_name` 方法查找到，
详细的查找方法可以在第四节元素查找中查看::

  elem = driver.find_element_by_name("q")

接下来，我们发送了一个关键字，这个方法的作用类似于你用键盘输入关键字。
特殊的按键可以使用Keys类来输入，该类继承自 `selenium.webdriver.common.keys`，
为了安全起见，我们先清除input输入框中的任何预填充的文本（例如："Search"）,从而避免我们的搜索结果受影响：

::

  elem.clear()
  elem.send_keys("pycon")
  elem.send_keys(Keys.RETURN)

提交页面后,你会得到所有的结果。为了确保某些特定的结果被找到，使用`assert`如下::

  assert "No results found." not in driver.page_source

最后，关闭浏览器窗口，你还可以使用quit方法代替close方法，
quit将关闭整个浏览器，而_close——只会关闭一个标签页，
如果你只打开了一个标签页，大多数浏览器的默认行为是关闭浏览器::

  driver.close()


用Selenium写测试用例
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Selenium 通常被用来写一些测试用例.  `selenium`
包本身不提供测试工具或者框架.  你可以使用Python自带的模块unittest写测试用例。
The other options for
a tool/framework are py.test and nose.

在本章中，我们使用 `unittest` 来编写测试代码，下面是一个已经写好的用例。
这是一个在 `python.org` 站点上搜索的案例::


  import unittest
  from selenium import webdriver
  from selenium.webdriver.common.keys import Keys

  class PythonOrgSearch(unittest.TestCase):

      def setUp(self):
          self.driver = webdriver.Firefox()

      def test_search_in_python_org(self):
          driver = self.driver
          driver.get("http://www.python.org")
          self.assertIn("Python", driver.title)
          elem = driver.find_element_by_name("q")
          elem.send_keys("pycon")
          elem.send_keys(Keys.RETURN)
          assert "No results found." not in driver.page_source
          

      def tearDown(self):
          self.driver.close()

  if __name__ == "__main__":
      unittest.main()


你可以在shell中运行下列代码::

  python test_python_org_search.py
  .
  ----------------------------------------------------------------------
  Ran 1 test in 15.566s

  OK

结果表明这个测试用例已经成功运行。


逐步解释测试代码
~~~~~~~~~~~~~~~~~~~~~~~~~~~

一开始，我们引入了需要的模块， `unittest
<http://docs.python.org/library/unittest.html>`_  模块是基于JAVA JUnit的Python内置的模块。
该模块提供了一个框架去组织测试用例。 `selenium.webdriver` 模块提供了所有WebDriver的实现。
现在支持的WebDriver有：Firefox, Chrome, IE and Remote. `Keys` 类提供所有的键盘按键操作，比如像这样的：
 RETURN, F1, ALT等。

::

  import unittest
  from selenium import webdriver
  from selenium.webdriver.common.keys import Keys

该测试类继承自 `unittest.TestCase`.
继承 `TestCase` 类是告诉 `unittest` 模块该类是一个测试用例::

  class PythonOrgSearch(unittest.TestCase):


`setUp` 方法是初始化的一部分, 该方法会在该测试类中的每一个测试方法被执行前都执行一遍。
下面创建了一个Firefox WebDriver的一个实例。

::

      def setUp(self):
          self.driver = webdriver.Firefox()

这是一个测试用例实际的测试方法. 测试方法始终以 `test`开头。  
在该方法中的第一行创建了一个在 `setUp` 方法中创建的驱动程序对象的本地引用。

::

      def test_search_in_python_org(self):
          driver = self.driver

`driver.get` 方法将会根据方法中给出的URL地址打开该网站。
WebDriver 会等待整个页面加载完成（其实是等待"onload"事件执行完毕）之后把控制权交给测试程序。
如果你的页面使用大量的AJAX技术来加载页面，WebDriver可能不知道什么时候页面已经加载完成::

          driver.get("http://www.python.org")

下面一行使用assert断言的方法判断在页面标题中是否包含 "Python" ::

          self.assertIn("Python", driver.title)


WebDriver 提供很多方法去查找页面值的元素，这些方法都以
`find_element_by_*` 开头。  例如：包含 `name` 属性的input元素可以使用
 `find_element_by_name`方法查找到。详细的细节可以参照 :ref:`locating-elements` 章节::

          elem = driver.find_element_by_name("q")

接下来我们发送keys，这个和使用键盘输入keys类似。
特殊的按键可以通过引入`selenium.webdriver.common.keys`的 `Keys` 类来输入
::

          elem.send_keys("pycon")
          elem.send_keys(Keys.RETURN)

提交页面之后，无论如何你都会得到搜索结果，为了确保某些结果类检索到，可以使用下列断言
After submission of the page, you should get result as per search if
::

  assert "No results found." not in driver.page_source

`tearDown` 方法会在每一个测试方法执行之后被执行。
该方法可以用来做一些清扫工作，比如关闭浏览器。
当然你也可以调用 `quit` 方法代替`close`方法，
 `quit` 将关闭整个浏览器，而`close`只会关闭一个标签页，
 如果你只打开了一个标签页，大多数浏览器的默认行为是关闭浏览器。

::

      def tearDown(self):
          self.driver.close()

下面是入口函数::

  if __name__ == "__main__":
      unittest.main()

.. _selenium-remote-webdriver:

使用远程 Selenium WebDriver
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

为了使用远程 WebDriver, 你应该拥有一个正在运行的 Selenium 服务器。
通过下列命令运行服务器::

  java -jar selenium-server-standalone-2.x.x.jar

Selenium 服务运行后, 你会看到这样的提示信息::

  15:43:07.541 INFO - RemoteWebDriver instances should connect to: http://127.0.0.1:4444/wd/hub

上面一行告诉你，你可以通过这个URL连接到远程WebDriver，
下面是一些例子::

  from selenium import webdriver
  from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

  driver = webdriver.Remote(
     command_executor='http://127.0.0.1:4444/wd/hub',
     desired_capabilities=DesiredCapabilities.CHROME)

  driver = webdriver.Remote(
     command_executor='http://127.0.0.1:4444/wd/hub',
     desired_capabilities=DesiredCapabilities.OPERA)

  driver = webdriver.Remote(
     command_executor='http://127.0.0.1:4444/wd/hub',
     desired_capabilities=DesiredCapabilities.HTMLUNITWITHJS)

`desired_capabilities`是一个字典，如果你不想使用默认的字典，你可以明确指定的值
::

  driver = webdriver.Remote(
     command_executor='http://127.0.0.1:4444/wd/hub',
     desired_capabilities={'browserName': 'htmlunit',
                           'version': '2',
                          'javascriptEnabled': True})

