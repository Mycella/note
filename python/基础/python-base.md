### 一.注释

```python
#单行注释
print('单行注释')
'''
第一行
第二行
'''
print('多行注释')
```

平台注释，针对linux

```python
#!usr/bin/python3
```

### 二.变量和数据类型

1. 数字类型

   int float long complex bool

2. 字符串

3. 字典

4. 元组

5. 列表

> type()函数可以查看变量的数据类型

### 三.算数运算符

> /是保留小数位的
>
> //是取整除法

### 四.逻辑运算符

> and
>
> or
>
> not

**优先级**

not->and->or

### 五.赋值运算

不支持++和--

### 六.输入和输出

#### 1.格式化输出

```python
#占位符% 类型s
name = 'gaojian'
age = '18'
print('i am %s, i am %s years old'%(name,age))

#format格式化输出  {}占位，不需要指定类型
print('我的名字:{}'.format(name))
print('我的年龄:{}'.format(age))
```

#### 2.输入

```python
addr = input("请输入地址:")
phone = input("请输入电话:")
print('我的名字:{}'.format(name))
print('我的年龄:{}'.format(age))
print('我的地址:{}'.format(addr))
print('我的电话:{}'.format(phone))
```

**format不需要类型转换，%d需要类型转换**

```python
int(input("请输入地址:"))
```

### 七.流程控制

#### 1.分支

```python
if 条件:
    执行语句
elif:
    执行语句
else:
    执行语句
```

#### 2.循环

```
while 条件:
	执行语句

for ... in 可迭代对象:
	执行语句
```

