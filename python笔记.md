# python

## 变量和简单数据类型

- 无定义

## 列表简介

列表   list = []

元组   tuple = ()

## 列表操作

- 增删改查

  - 增加

    alist.append()末尾增加

    alist.insert(index,value)在index处增加

  - 删除

    del alist[i] 删除i位置元素

    val = alist.pop() 删除尾元素并赋值

    remove(val) 删除指定值的元素

  - 改

    alist[i] = val;

  - 查

    a in alist

- 排序

  alist.sort()

  alist.reverse()

- 遍历 

  ```python
  for  a in alist :	#注意冒号
  	....
  ```

- 复制列表用切片 b = a[:]
- 元组a()类似列表a[]，但元素不可变

## 条件句

-  

  ```python
  if ...  : 	#注意冒号
  ```

- and   or 

- 是否在列表中

  ```python
  if a in alist  /  if  a not in alist
  ```

- 列表为空

  ```
  if alist:
  	....	#非空
  else:
  	....	#空
  ```

## 字典

dic = {'color':'grean', 'points:5'} 键与值可以是任何值

- 增删改

  - 增

    dic[新键] = 新值

  - 删

    del dic[键]

  - 改

    dic[键] = 新值

- 遍历

  ```python
  for key,value in dic.items():	#调用内置功能化为列表
  
  	...
  
  for key in dic.key():
  
  	...
  
  for value in dic.values():
  
  	...
  ```

- 嵌套

  字典内套列表，列表内套字典

## 用户输入

- s = input(str)

  先打印str作为提示信息，再将输入内容赋值给s，类型为字符串

- 强制转化

  int(str)，将字符串类型的str转为int，'3'转为3

- while

  ```python
  while 条件:
  	...
  ```

## 函数

```python
def func(a,b):		##  a,b形参，是传入的实参给它赋值得到的

	...

	return c
```

- 默认值

  通常放无默认值的形参后面

- 传入列表

  若直接传入def func(alist):，则函数可直接修改列表

  若传入def func(alist[:])，则传入列表副本，不可直接修改

- 传入任意数量实参

  ```python
  def func(*a):
      for b in a:
          ...
  	...
  ```

  传入所有参数存入a的元组中，注意该元组内元素任意，可以为不同类型元素

  - 位置实参+任意数量实参

    ```python
    def func(a,*b):
    ```

    使得可以先接受a再接受任意数量实参

  - 任意数量关键字实参

    ```python
    def func(**a):
        for key, value in a:
            ...
    func('q' = 1, 'w' = 2)
    ```

    a为参数字典

- 将函数存储在模块中

  - 导入整个模块

    `import module_name`导入该模块中所有函数`module_name.func_name()`调用该函数

    或

    `import module_name as m`导入模块函数并重命名

    `m.func_name()`调用该函数

  - 导入模块中某函数

    `from module_name import func_name`导入模块中某函数

    `func_name()`使用该函数

    或

    `from module_name import func_name as f`导入模块中某函数并重命名

    `f()`使用该函数

    或

    `from module_name import *`导入该模块所有函数

    `f()`使用函数

## 类

```python
class Dog():
    
    def __init(self, name, age):
        self.name = name
        self.age = age
        
    def fun1(self):
        ...
    def fun2(self, ...):
        ...
        
my_dog = Dog("tt", 3)
```

- self

  self是指向实例的引用，必不可少

- 类的属性

  定义：即在`__init__`中定义的self.name、self.age

  访问：my_dog.name即可访问与修改，或者在类的操作中访问修改

- 继承

  ```python
  class Dog():
  	...
  class BigDog(Dog):
      def __init__(self, name, age):
          super().__init__(name, age)
          ... #可定义子类属性，如 self.height = 
          
  ```

  - 重写

    重新覆盖def fun1()即可

- 导入类

  - 导入某类

    `from car import Car` 从car文件中导入Car类

    `my_car = Car(...)`使用Car类

  - 导入整个模块

    `import car`

    `my_car = car.Car(...)`使用Car类

## 文件和异常

- 读文件

  ```python
  with open(filename) as file_object:
  	contends = file_object.read()
  	'''
  	逐行读取
  	for line in file_object:
  		...
  	或
  	lines = file_object.readlines()
  	'''
  ```

  with：读完文件后自动关闭

  filename：文件路径

  .read()：整个文件的字符串

  .readline()：文件一行一行的列表

  - open函数

    `file object = open(file_name [, access_mode][, buffering])`

    - access_mode

      w：打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。**如果该文件不存在，创建新文件。**

- 写文件

  `open(filename,'w')`覆盖写

  `open(filename,'a')`附加写

  `file_object.write("......")`:写文件

- 异常处理

  ```python
  try:
      ...
  except ...Error:
      ...
      #也可用pass表示什么都不做
  else:
      ...
  ```


## enumerate()

 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。

## 命令行处理

argparse.ArgumentParser()

- 创建解析器

  ```python
  parser = argparse.ArgumentParser(description='Process some integers.')
  ```

- 添加参数

  ```python
  parser.add_argument('integers', metavar='N', type=int, nargs='+', help='an integer for the accumulator')
  ```

- 解析参数

  ```python
  >>> parser.parse_args(['--sum', '7', '-1', '42'])
  Namespace(accumulate=<built-in function sum>, integers=[7, -1, 42])
  ```