.. _faq:

附录：常见问题
==============

Another FAQ: https://github.com/SeleniumHQ/selenium/wiki/Frequently-Asked-Questions

如何使用 ChromeDriver ?
~~~~~~~~~~~~~~~~~~~~~~~~~

下载最新版本的 `chromedriver
<https://sites.google.com/a/chromium.org/chromedriver/downloads>`_.  解压缩这个文件::

  unzip chromedriver_linux32_x.x.x.x.zip

你应该会看到一个 ``chromedriver`` 的可执行文件.  现在你可以像这样创建一个
Chrome WebDriver 实例::

  driver = webdriver.Chrome(executable_path="/path/to/chromedriver")

这个示例的其余部分应该在其他的文档中给出。

Selenium 2是否支持XPath 2.0版本？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

参考：http://seleniumhq.org/docs/03_webdriver.html#how-xpath-works-in-webdriver

Selenium代表的XPath查询基于浏览器自身的XPath引擎，所以Selenium支持任何
支持XPath的浏览器。在不具备原生的XPath引擎（IE6,7,8）的浏览器，Selenium只支持XPath 1.0。


如何向下滚动到页面的底部？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

参考: http://blog.varunin.com/2011/08/scrolling-on-pages-using-selenium.html

你可以在加载完成的页面上使用 `execute_script` 方法执行js。所以，
你调用javascript API滚动到底部或页面的任何位置。

这里是一个滚动到页面底部的例子::

  driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

该 `window <http://www.w3schools.com/jsref/obj_window.asp>`_ 对象在DOM有一个
`scrollTo <http://www.w3schools.com/jsref/met_win_scrollto.asp>`_ 滚动到打开窗口
的任意位置的方法。 该 `scrollHeight <http://www.w3schools.com/jsref/dom_obj_all.asp>`_
是所有元素的共同属性。 该 `document.body.scrollHeight` 将给出整个页面体的高度。

如何使用自定义的Firefox 配置文件保存文件？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

参考: http://stackoverflow.com/questions/1176348/access-to-file-download-dialog-in-firefox

参考: http://blog.codecentric.de/en/2010/07/file-downloads-with-selenium-mission-impossible/

第一步是要确认自动保存文件的类型。

要确定你想要自动下载的内容类型，你可使用 `curl <http://curl.haxx.se/>`_::

  curl -I URL | grep "Content-Type"

找到内容类型的另一种方法是使用 `requests <http://python-requests.org>`模块，
你可以像这样使用::
  import requests
  content_type = requests.head('http://www.python.org').headers['content-type']
  print(content_type)
  
一旦内容类型被确认，你可以用它来设置firefox配置文件的偏好: ``browser.helperApps.neverAsk.saveToDisk``
下面是一个例子::

  import os

  from selenium import webdriver

  fp = webdriver.FirefoxProfile()

  fp.set_preference("browser.download.folderList",2)
  fp.set_preference("browser.download.manager.showWhenStarting",False)
  fp.set_preference("browser.download.dir", os.getcwd())
  fp.set_preference("browser.helperApps.neverAsk.saveToDisk", "application/octet-stream")

  browser = webdriver.Firefox(firefox_profile=fp)
  browser.get("http://pypi.python.org/pypi/selenium")
  browser.find_element_by_partial_link_text("selenium-2").click()

在上面的例子中，``application/octet-stream`` 被当作内容类型。

该 ``browser.download.dir`` 选项指定了你要下载文件的目录。

如果上传文件到文件上传控件？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

选择 ``<input type="file">`` 元素并且调用 ``send_keys()`` 方法传入要上传文件的路径，可以
是对于测试脚本的相对路径，也可以是绝对路径。
请牢记在Windows和Unix系统之间的路径名的区别。

如果在Firefox中使用firebug工具？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

首先下载Firebug插件的XPI文件， 然后调用对于firefox 的配置提供的 ``add_extension`` 方法

  from selenium import webdriver

  fp = webdriver.FirefoxProfile()

  fp.add_extension(extension='firebug-1.8.4.xpi')
  fp.set_preference("extensions.firebug.currentVersion", "1.8.4") #Avoid startup screen
  browser = webdriver.Firefox(firefox_profile=fp)

如果获取当前窗口的截图？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

使用webdriver提供的 `save_screenshot` 方法::

  from selenium import webdriver

  driver = webdriver.Firefox()
  driver.get('http://www.python.org/')
  driver.save_screenshot('screenshot.png')
  driver.quit()

