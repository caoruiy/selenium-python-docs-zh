.. _navigating:

打开一个页面
=============

你想做的第一件事也许是使用WebDriver打开一个链接。
常规的方法是调用 ``get`` 方法:

::

  driver.get("http://www.google.com")

WebDriver 将等待，直到页面完全加载完毕（其实是等到 ``onload`` 方法执行完毕），
然后返回继续执行你的脚本。
值得注意的是，如果你的页面使用了大量的Ajax加载，
WebDriver可能不知道什么时候页面已经完全加载。
如果你想确保也main完全加载完毕，可以使用:ref:`waits <waits>`

::


与页面交互
~~~~~~~~~~~~~~~~~~~~~~~~~

只是打开页面其实并没有什么卵用。我们真正想要的是与页面做交互。
更具体地说，对于一个页面中的HTML元素，首先我们要找到他。WebDriver
提供了大量的方法帮助你去查找元素，例如：已知一个元素定义如下::

  <input type="text" name="passwd" id="passwd-id" />

你可以通过下面的方法查找他::

  element = driver.find_element_by_id("passwd-id")
  element = driver.find_element_by_name("passwd")
  element = driver.find_element_by_xpath("//input[@id='passwd-id']")

你还可以通过链接的文本查找他，需要注意的是，这个文本必须完全匹地配。
当你使用`XPATH`时，你必须注意，如果匹配超过一个元素，只返回第一个元素。
如果上面也没找到，将会抛出 ``NoSuchElementException``异常。


.. TODO: Is this following paragraph correct ?

WebDriver有一个"基于对象"的API; 我们使用相同的接口表示所有类型的元素。
这就意味着，当你打开你的IDE的自动补全的时候，你会有很多可以调用的方法。
但是并不是所有的方法都是有意义或是有效的。不过不要担心！
当你调用一些毫无意义的方法时，WebDriver会尝试去做一些正确的事情（例如你对一个"meta"
元素调用"setSelected()"方法的时候）。

所以，当你拿到ige元素时，你能做什么呢？首先，你可能会想在文本框中输入一些内容::

  element.send_keys("some text")

你还可以通过"Keys"类来模式输入方向键::

  element.send_keys(" and some", Keys.ARROW_DOWN)

对于任何元素，他可能都叫 `send_keys` ，这就使得它可以测试键盘快捷键，
比如当你使用Gmail的时候。但是有一个副作用是当你输入一些文本时，这些
输入框中原有的文本不会被自动清除掉，相反，你的输入会继续添加到已存在文本之后。
你可以很方便的使用 `clear` 方法去清除input或者textarea元素中的内容::

  element.clear()


填写表格
~~~~~~~~~~~~~~~~

我们已经知道如何在input或textarea元素中输入内容，但是其他元素怎么办？
你可以“切换”下拉框的状态，你可以使用``setSelected``方法去做一些事情，比如
选择下拉列表，处理`SELECT`元素其实没有那么麻烦::

    element = driver.find_element_by_xpath("//select[@name='name']")
    all_options = element.find_elements_by_tag_name("option")
    for option in all_options:
        print("Value is: %s" % option.get_attribute("value"))
        option.click()

上面这段代码将会寻找页面第一个 "SELECT" 元素, 并且循环他的每一个OPTION元素，
打印从utamen的值，然后按顺序都选中一遍。

正如你说看到的那样，这不是处理 SELECT 元素最好的方法。WebDriver的支持类包括一个叫做
``Select``的类，他提供有用的方法处理这些内容::

    from selenium.webdriver.support.ui import Select
    select = Select(driver.find_element_by_name('name'))
    select.select_by_index(index)
    select.select_by_visible_text("text")
    select.select_by_value(value)

WebDriver 也提供一些有用的方法来取消选择已经选择的元素::

    select = Select(driver.find_element_by_id('id'))
    select.deselect_all()

这将取消选择所以的OPTION。

假设在一个案例中，我们需要列出所有已经选择的选项，Select类提供了方便的方法来实现这一点::

    select = Select(driver.find_element_by_xpath("xpath"))
    all_selected_options = select.all_selected_options
    
获得所以选项::

    options = select.options

一旦你填写完整个表单，你应该想去提交它，有一个方法就是去找到一个“submit”
按钮然后点击它::

  # Assume the button has the ID "submit" :)
  driver.find_element_by_id("submit").click()

或者，WebDriver对每一个元素都有一个叫做 "submit" 的方法，如果你在一个表单内的
元素上使用该方法，WebDriver会在DOM树上就近找到最近的表单，返回提交它。
如果调用的元素不再表单内，将会抛出``NoSuchElementException``异常::

  element.submit()


拖放
~~~~~~~~~~~~~

您可以使用拖放，无论是移动一个元素，或放到另一个元素内::

  element = driver.find_element_by_name("source")
  target = driver.find_element_by_name("target")

  from selenium.webdriver import ActionChains
  action_chains = ActionChains(driver)
  action_chains.drag_and_drop(element, target).perform()

在不同的窗口和框架之间移动
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

对于现在的web应用来说，没有任何frames或者只包含一个window窗口是比较罕见的。
WebDriver 支持在不同的窗口之间移动，只需要调用``switch_to_window``方法即可::

  driver.switch_to_window("windowName")

所有的 ``driver`` 将会指向当前窗口，但是你怎么知道当前窗口的名字呢，查看打开他的javascript或者连接代码::

  <a href="somewhere.html" target="windowName">Click here to open a new window</a>

或者，你可以在"switch_to_window()"中使用"窗口句柄"来打开它，
知道了这些，你就可以迭代所有已经打开的窗口了::

  for handle in driver.window_handles:
      driver.switch_to_window(handle)

你还可以在不同的frame中切换 (or into iframes)::

  driver.switch_to_frame("frameName")

通过“.”操作符你还可以获得子frame，并通过下标指定任意frame，就像这样::

  driver.switch_to_frame("frameName.0.child")

如何获取名叫“frameName”的frame中名叫 “child”的子frame呢？
**来自*top*frame的所有的frame都会被评估**
（**All frames are evaluated as if from *top*.**）

一旦我们完成了frame中的工作，我们可以这样返回父frame::

  driver.switch_to_default_content()

弹出对话框
~~~~~~~~~~~~~

Selenium WebDriver 内置了对处理弹出对话框的支持。
在你的某些动作之后可能会触发弹出对话框，你可以像下面这样访问对话框::

  alert = driver.switch_to_alert()

它将返回当前打开的对话框对象。使用此对象，您现在可以接受、排除、读取其内容，
甚至可以在prompt对话框中输入(译注：prompt()是对话框的一种，不同于alert()对话框，不同点可以自行百度)。
这个接口对alert, confirm， prompt 对话框效果相同。
参考相关的API文档获取更多信息。


访问浏览器历史记录
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在之前的文章中，我们使用``get``命令打开一个页面, (
``driver.get("http://www.example.com")``)，WebDriver有很多更小的，以任务为导向的接口，
navigation就是一个有用的任务，打开一个页面你可以使用`get`::

  driver.get("http://www.example.com")

在浏览历史中前进和后退你可以使用::

  driver.forward()
  driver.back()
请注意，这个功能完全取决于底层驱动程序。当你调用这些方法的时候，很有可能会发生意想不到的事情，
如果你习惯了浏览器的这些行为于其他的不同。（原文：It’s just possible that something unexpected may happen 
when you call these methods if you’re used to the behaviour of one browser over another.）

操作Cookies
~~~~~~~~~~~~~

在我们结束这一节之前，或许你对如何操作Cookies可能会很感兴趣。
首先，你需要打开一个页面，因为Cookie是在某个域名下才生效的::


  # 打开一个页面
  driver.get("http://www.example.com")

  # 现在设置Cookies，这个cookie在域名根目录下（"/"）生效
  cookie = {‘name’ : ‘foo’, ‘value’ : ‘bar’}
  driver.add_cookie(cookie)

  # 现在获取所有当前URL下可获得的Cookies
  driver.get_cookies()
  
