.. _installation:

安装
=====

安装
~~~~~~~~~~~~

Selenium Python bindings 提供了一个简单的API，让你使用Selenium WebDriver来编写功能/校验测试。
通过Selenium Python的API，你可以非常直观的使用Selenium WebDriver的所有功能。

Selenium Python bindings 使用非常简洁方便的API让你去使用像Firefox, IE, Chrome, Remote等等
这样的Selenium WebDrivers（Selenium web驱动器）。当前支持的版本为 2.7, 3.2及以上。

本文的用来讲解说明Selenium 2 WebDriver的API，此文档不包含Selenium 1 / Selenium RC的文档。


下载 Python bindings for Selenium
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

可以从PyPI的官方库中下载该selenium支持库， `点此下载 <https://pypi.python.org/pypi/selenium>`_  当然，
更好的方法当然是使用 `pip <https://pip.pypa.io/en/latest/installing/>`_ 命令来安装selenium包。
Python3.5的 `标准库 <https://docs.python.org/3.5/installing/index.html>`_中包含pip命令。
使用 `pip`命令,你可以像下面这样安装 selenium::

  pip install selenium

.. Note::
  使用Python2.x版本的用户可以手动安装pip或者easy_install，下面是easy_install 的安装方法::

  easy_install selenium

你可以考虑使用`virtualenv <http://www.virtualenv.org>`_
创建独立的Python环境。Python3.5中`pyvenv
<https://docs.python.org/3.5/using/scripts.html#scripts-pyvenv>`_
可以提供几乎一样的功能。


Windows用户的详细说明
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Note::

  请在有网的情况下执行该安装命令。

1. 安装Python3.5：`官方下载页 <http://www.python.org/download>`_.

2. 从开始菜单点击运行（或者`Windows+R`）输入`cmd`,然后执行下列命令安装::
   
     C:\Python35\Scripts\pip.exe install selenium

现在你可以使用Python运行测试脚本了。
例如：如果你创建了一个selenium的基本示例并且保存在了``C:\my_selenium_script.py``，你可以如下执行::

  C:\Python35\python.exe C:\my_selenium_script.py


下载 Selenium 服务器
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Note::
  如果你想使用一个远程的WebDriver，Selenium服务是唯一的依赖，
  参见 :ref:`selenium-remote-webdriver` 获得更多细节。
  如果你只是刚刚开始学习使用Selenium，你可以忽略该章节直接开始下一节。

Selenium server是一个JAVA工程，Java Runtime Environment (JRE) 1.6或者更高的版本是推荐的运行环境。

你可以在 `该下载页 <http://seleniumhq.org/download/>`_
下载2.x的Selenium server，这个文件大概长成这个样子：``selenium-server-standalone-2.x.x.jar``，
你可以去下载最新版本的2.x server。

如果你还没有安装Java Runtime Environment (JRE)的话，
呢，`在这下载
<http://www.oracle.com/technetwork/java/javase/downloads/index.html>`_，
如果你是有的是GNU/Linux系统，并且巧了，你还有root权限，你还可以使用操作系统指令去安装JRE。

如果你把`java`命令放在了PATH(环境变量)中的话，使用下面命令安装::

  java -jar selenium-server-standalone-2.x.x.jar

当然了，把``2.x.x``换成你下载的实际版本就可以了。

如果是不是root用户你或者没有把JAVA放到PATH中，
你可以使用绝对路径或者相对路径的方式来使用命令，
这个命令大概长这样子::

  /path/to/java -jar /path/to/selenium-server-standalone-2.x.x.jar
