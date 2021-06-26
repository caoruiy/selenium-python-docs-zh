.. _waits:

等待页面加载完成(Waits)
========================

现在的大多数的Web应用程序是使用Ajax技术。当一个页面被加载到浏览器时，
该页面内的元素可以在不同的时间点被加载。这使得定位元素变得困难，
如果元素不再页面之中，会抛出 `ElementNotVisibleException` 异常。
使用 waits, 我们可以解决这个问题。waits提供了一些操作之间的时间间隔-
主要是定位元素或针对该元素的任何其他操作。

Selenium Webdriver 提供两种类型的waits - 隐式和显式。
显式等待会让WebDriver等待满足一定的条件以后再进一步的执行。
而隐式等待让Webdriver等待一定的时间后再才是查找某元素。


显式等待
~~~~~~~~~~~~~~

显式等待是你在代码中定义等待一定条件发生后再进一步执行你的代码。
最糟糕的案例是使用time.sleep()，它将条件设置为等待一个确切的时间段。
这里有一些方便的方法让你只等待需要的时间。WebDriverWait结合ExpectedCondition
是实现的一种方式。

::

  from selenium import webdriver
  from selenium.webdriver.common.by import By
  from selenium.webdriver.support.ui import WebDriverWait
  from selenium.webdriver.support import expected_conditions as EC

  driver = webdriver.Firefox()
  driver.get("http://somedomain/url_that_delays_loading")
  try:
      element = WebDriverWait(driver, 10).until(
          EC.presence_of_element_located((By.ID, "myDynamicElement"))
      )
  finally:
      driver.quit()

在抛出TimeoutException异常之前将等待10秒或者在10秒内发现了查找的元素。
WebDriverWait 默认情况下会每500毫秒调用一次ExpectedCondition直到结果成功返回。
ExpectedCondition成功的返回结果是一个布尔类型的true或是不为null的返回值。


**预期的条件**

自动化的Web浏览器中一些常用的预期条件，下面列出的是每一个实现，
Selenium Python binding都提供了一些方便的方法，这样你就不用去编写
expected_condition类或是创建至今的工具包去实现他们。

- title_is

- title_contains

- presence_of_element_located

- visibility_of_element_located
- visibility_of
- presence_of_all_elements_located
- text_to_be_present_in_element
- text_to_be_present_in_element_value
- frame_to_be_available_and_switch_to_it
- invisibility_of_element_located
- element_to_be_clickable - 显示并可用.
- staleness_of
- element_to_be_selected
- element_located_to_be_selected
- element_selection_state_to_be
- element_located_selection_state_to_be
- alert_is_present

::

  from selenium.webdriver.support import expected_conditions as EC

  wait = WebDriverWait(driver, 10)
  element = wait.until(EC.element_to_be_clickable((By.ID,'someid')))

expected_conditions 模块提供了一组预定义的条件供WebDriverWait使用。


隐式等待
~~~~~~~~~~~~~~

如果某些元素不是立即可用的，隐式等待是告诉WebDriver去等待一定的时间后去查找元素。
默认等待时间是0秒，一旦设置该值，隐式等待是设置该WebDriver的实例的生命周期。

::

  from selenium import webdriver

  driver = webdriver.Firefox()
  driver.implicitly_wait(10) # seconds
  driver.get("http://somedomain/url_that_delays_loading")
  myDynamicElement = driver.find_element_by_id("myDynamicElement")
