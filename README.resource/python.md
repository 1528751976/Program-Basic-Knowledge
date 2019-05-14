python3基础以及本人常用的python库
====
## python3基础语法
### 1.数据类型
  - Number 
   <br>如整型、浮点型</br>
  - List
   <br>[1,"a",2,"b"]列表里可以是任意类型</br>
  - String
  <br>"这是字符串"</br>
  - Dict
  <br>{1:"a",2:"b",3:"c"} 字典型为键值对</br>
  - Tuple
  <br>(1,2,3) 元组</br>
  - Set
  <br>set([1,2,3]) 集合</br>
  - Bool
  <br>True和False</br>
python中，list是可变的，而tuple和set是不可变的。
### 2.语法详解
#### 输入/输出
 ```python
	A = input（“xxx”） # python2/3输入语句，py2返回数字类型，py3返回字符串类型
	B = raw_input(“xxx”) # python2输入语句，返回字符串类型。
	print(“这是一个字符串”) # pytho3输出语句
	print “这也是一个字符串”# python2输出语句
```

#### 条件语句
```python
if a>b:   # if空格后直接加判断语句加冒号
	pass  # pass语句，表示跳过，不做任何动作
elif c>d:  # 相当于else if
	pass
else:
	pass
```

#### 循环语句
```python
for i in range(a[[,b],c]): 
# for循环语句，和range()配合使用，range()表示一个范围，左闭右开区间,当只有一个参数a时，范围为0-(a-1);当有两个参数时，范围为a-(b-1); 当有三个参数时，范围为a-(b-1)，步长为c;
	pass
	break # 结束整个循环
else：
	pass # 当for正常循环结束后，执行else语句

while a>b:  # while循环语句，条件不成立时，结束循环。
	pass
	continue # 结束当前次的循环，进入下次循环。
else：  # 当while正常结束后，执行else
	pass
```
#### 逻辑运算符
	
运算符 | 逻辑表达式 | 描述
---- | ---- | ----
and | a and b | “与”，如果a和b中至少有一个为False，则为False；a和b全为True，则为True。
or | a or b |“或”，如果a和b中有一个为True，则为True；a和b全为False，则为False。
not | not a | “非”，如果a为True，则False；如果a为False，则False。

#### 函数的定义
```python
def 函数名(参数列表):  # def 定义一个函数，传入n个参数（n>=0）
	pass
	return x
	
def fun():
	return 1+2
fun() # 调用fun()函数
```
 #### 列表的方法
方法 | 描述
---- | ---- 
list.append(x)	|把一个元素添加到列表的结尾。 
list.extend(L)	|通过添加指定列表的所有元素来扩充列表。
list.insert(i, x)|在指定的位置插入一个元素。
list.remove(x)	|删除列表中值为 x 的第一个元素。如果没有这样的元素，就会返回一个错误。
list.pop([i])	|从列表的指定位置移除元素，并将其返回。如果没有指定索引，a.pop()返回最后一个元素。元素随即从列表中被移除。
list.clear()	| 清除列表中的所有项。
list.index(x)	| 返回列表中第一个值为 x 的元素的索引。如果没有匹配的元素就会返回一个错误。
list.count(x)	| 返回 x 在列表中出现的次数。
list.sort()	 | 对列表中的元素进行排序。
list.reverse() |倒排列表中的元素。
list.copy()	| 返回列表的浅复制。



## python库selenium基础应用
### 初始化
-安装selenium<br>
首先，下载selenium库<br>
```pip install selenium```<br>

之后导入selenium库，检查是否下载成功。
```python
import selenium
```

- 安装浏览器驱动<br>
    Selenium调用浏览器必须有一个webdriver驱动文件。
    1. Chrome驱动文件下载：[chromedrive](https://chromedriver.storage.googleapis.com/index.html?path=2.35/)
    2. Firefox驱动文件下载：[geckodriver](https://github.com/mozilla/geckodriver/releases)
    
   Chrome版本与chromedriver版本对照及下载：
   

chromedirve版本|chrome版本
----|----
ChromeDriver v2.46 (2019-02-01) | Chrome v71-73
ChromeDriver v2.45 (2018-12-10) | Chrome v70-72
ChromeDriver v2.44 (2018-11-19) | Chrome v69-71
ChromeDriver v2.43 (2018-10-16) | Chrome v69-71
ChromeDriver v2.42 (2018-09-13) | Chrome v68-70
ChromeDriver v2.41 (2018-07-27) | Chrome v67-69
ChromeDriver v2.40 (2018-06-07) | Chrome v66-68
ChromeDriver v2.39 (2018-05-30) | Chrome v66-68
ChromeDriver v2.38 (2018-04-17) | Chrome v65-67
ChromeDriver v2.37 (2018-03-16) | Chrome v64-66
ChromeDriver v2.36 (2018-03-02) | Chrome v63-65
ChromeDriver v2.35 (2018-01-10) | Chrome v62-64

 > 下载的浏览器驱动请放在python安装目录下。如果放在其他目录下，请自行配环境。

### selenium的定位方式

- selenium提供了8钟定位方式
    - id
    - name
    - class name
    - tag name
    - link text
    - partial link text
    - xpath
    - css selector
- 8种定位的python方法

    定位一个元素 | 定位多个元素 | 含义
    ---- | ---- | ----
    find_element_by_id() | find_elements_by_id() | 使用元素id定位
    find_element_by_name() | find_elements_by_name() | 使用元素名字定位
    find_element_by_class_name() | find_elements_by_class_name() | 使用元素类名定位
    find_element_by_tag_name() | find_elements_by_tag_name() | 使用标签定位
    find_element_by_link_text() | find_elements_by_link_text() | 使用完整链接定位
    find_element_by_partial_link_text() | find_elements_by_partial_link_text() | 使用部分链接定位
    find_element_by_xpath() | find_elements_by_xpath() | 使用元素xpath路径定位
    find_element_by_css_selector() | find_elements_by_css_selector() | 使用css选择器定位
    
- 举个栗子

    pass
    
    
### 控制浏览器的一些方法

 方法 | 含义
 ---- |---
 set_window_size() | 设置浏览器大小
 back() | 浏览器后退
 forward() | 浏览器前进
 refresh() | 刷新当前页
 clear() | 清除文本
 send_keys(value) | 模拟按键输入
 click() | 单击元素
 submit() | 用于提交表单
 get_attribute(name) | 获取元素属性
 is_displayed() | 设置该元素是否用户可见
 size | 返回元素的尺寸
 text | 获取元素的文本

### 鼠标事件
方法 | 说明 
----| ----
ActionChains(driver) | 构造ActionChains对象
click() | 单击
double_click() | 双击
context_click() | 右击
move_to_element(above) | 执行鼠标悬停操作
drag_and_drop(source，target) | 拖动
drag_and_drop_by_offset(source，x,y) | 拖动到某个坐标后松开
perform() | 执行所有 ActionChains 中存储的行为，可以理解成是对整个操作的提交动作
    
    
