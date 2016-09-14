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

Selenium delegates XPath queries down to the browser's own XPath
engine, so Selenium support XPath supports whatever the browser
supports.  In browsers which don't have native XPath engines (IE
6,7,8), Selenium supports XPath 1.0 only.



如何向下滚动到页面的底部？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

参考: http://blog.varunin.com/2011/08/scrolling-on-pages-using-selenium.html

你可以在加载完成的页面上使用 `execute_script` 方法执行js。所以，
你调用javascript API滚动到底部或页面的任何位置。

这里是一个滚动到页面底部的例子::

  driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

The `window <http://www.w3schools.com/jsref/obj_window.asp>`_ object
in DOM has a `scrollTo
<http://www.w3schools.com/jsref/met_win_scrollto.asp>`_ method to
scroll to any position of an opened window.  The `scrollHeight
<http://www.w3schools.com/jsref/dom_obj_all.asp>`_ is a common
property for all elements.  The `document.body.scrollHeight` will give
the height of the entire body of the page.

How to auto save files using custom Firefox profile ?
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

The ``browser.download.dir`` option specify the directory where you
want to download the files.

How to upload files into file inputs ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Select the ``<input type="file">`` element and call the ``send_keys()`` method passing 
the file path, either the path relative to the test script, or an absolute path.
Keep in mind the differences in path names between Windows and Unix systems.

How to use firebug with Firefox ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First download the Firebug XPI file, later you call the
``add_extension`` method available for the firefox profile::

  from selenium import webdriver

  fp = webdriver.FirefoxProfile()

  fp.add_extension(extension='firebug-1.8.4.xpi')
  fp.set_preference("extensions.firebug.currentVersion", "1.8.4") #Avoid startup screen
  browser = webdriver.Firefox(firefox_profile=fp)

How to take screenshot of the current window ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the `save_screenshot` method provided by the webdriver::

  from selenium import webdriver

  driver = webdriver.Firefox()
  driver.get('http://www.python.org/')
  driver.save_screenshot('screenshot.png')
  driver.quit()

