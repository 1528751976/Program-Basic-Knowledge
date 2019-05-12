python3基础以及本人常用的python库
====
## python3基础语法
### 1.数据类型
  - Number 
  - List
  - String
  - Dict
  - Tuple
  - Set
  - Bool
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
else：
	pass # 当for正常循环结束后，执行else语句

while a>b:  # while循环语句，条件不成立时，结束循环。
	pass
else：  # 当while正常结束后，执行else
	pass
```

