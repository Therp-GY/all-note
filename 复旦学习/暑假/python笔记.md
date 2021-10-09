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
    
    - 获取某元素索引
    
      a.index(target)

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

- self,cls

  self是指向实例的引用，必不可少

  cls代表的是类本身

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
  
- 注意

  类的定义上的括号用于完成继承指定；

  类的创建调用内部`__init__()`，是个函数因此需要加括号：`class_a  =  someClass()`；

  类的初始化在`__init__()`中初始化；

  类的属性使用`self.attribute`定义；

//TODO

- 类的静态方法和类方法

  ```python
  class A(object):
      a = 'a'
      @staticmethod
      def foo1(name):
          print 'hello', name
          
      def foo2(self, name):
          print 'hello', name
          
      @classmethod
      def foo3(cls, name):
          print 'hello', name
  def __main__():
      a = A()
      a.foo1('mamq')  # correct
      A.foo1('mamq')  # correct
      a.foo2('mamq')  # only can use this
      a.foo3('mamq')  # correct
      A.foo3('mamq')  # correct
  ```

  同时@classmethod中还可依靠cls调用内部属性和函数

  ```python
  class A(object):
      a = 'a'
  	....
      @classmethod
      def foo3(cls, name):
          print A.a
          print cls().foo2(name)
  ```

- `__new__`和`__init__`

  ```python
  class Person(object):
  
      def __new__(cls, name, age):
          print '__new__ called.'
          return super(Person, cls).__new__(cls, name, age)
  
      def __init__(self, name, age):
          print '__init__ called.'
          self.name = name
          self.age = age
  ```

  new调用在init前，用于创建一个实例，而init用于初始化这个创建的实例，self就是这个实例

  

## 接口

需要import abs来定义接口

```python
'''
	define an interface
'''

class fatherInterface(metaclass = abc.ABCMeta):
    @abc.abstractmethod
    def pullshit(self, method):
        pass
   
class son(fatherInterface):
    def __init__(self):
        self.a = "big shit"

    def pullshit(self, method):
        print(method + "shit!!!!")
        return self.a
class shitFactory():

    def createShit(self, who):
        if who == 'brother':
            return brother()
        if who == 'sister':
            return sister()

factory = shitFactory()
someBody = factory.createShit('brother')
someBody.pullshit()

```

- 以fatherInterface作为接口
- 以`class son(fatherInterface)`继承写法作为实现接口
- @abs.abstractmethod作为接口函数，实现接口的类必须重写该函数

如此以来，我只需要将someBody



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
    
  - os.walk(dir_path)函数：

    返回三个值

    1. dirpath(目录路径，它是一个string类型)
    2. dirname(子目录名，它是一个列表，因为在一个目录路径下会有很多很多子目录)
    3. filename(文件名，它也是一个列表，因为同一目录下一班有多个文件

  - shutil.copy(src, dst)

    shutil.copyfile(src, dst)：复制文件内容（不包含元数据）从src到dst。

     DST必须是完整的目标文件名;

    shutil.move剪切

  - os.path.join()函数

    连接两个或更多的路径名组件

    1. 如果各组件名首字母不包含'/'，则函数会自动加上
    2. 如果有一个组件是一个绝对路径，则在它之前的所有组件均会被舍弃

  - os.listdir(path)函数

    用于返回指定的文件夹包含的文件或文件夹的名字的列表

- 写文件

  `open(filename,'w')`覆盖写

  `open(filename,'a')`附加写

  `file_object.write("......")`:写文件

  `os.rename(src,dst)`：修改文件名

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

## numpy

- 数组重新排列

  reshape()函数

## PIL

### 图像存储

- 存储方式

  图片是通过像素拼接而成的，我们常说的分辨率指的就是[图像像素](https://www.baidu.com/s?wd=图像像素&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)的数量，比如分辨率为[1024*768](https://www.baidu.com/s?wd=1024*768&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)的一张图片就是在长度上的像素点数为1024个，高度768个。

   每个像素点在计算机中存储的信息是他的RGB数值,将这些RGB（红、绿、蓝，组成图像的三原色，范围是从0~225）以二进制方式存在硬盘中。

  灰度图像只有1个通道，读取为数组的shape为[m,n]，坐标[m,n]就是图像中m行n列位置的像素。

  RGB图像有3个通道，读取为数组的shape为[x,y,3]

- 几种存储模式

  1. 位图模式

     位图模式是1位深度的图像。它只是黑和白两种颜色。它可以由扫描或置入黑色的矢量线条图像生成，也能由灰度模式或双色调模式转换而成。其他图像模式不能直接转换为位图模式。

  2. 灰度模式

     灰度模式是8位深度的图像模式。也就是28，28=256，在全黑和全白之间插有254个灰度等级的颜色来描绘灰度模式的图像。

     所有模式的图像都能换成灰度模式，甚至位图也可转换为灰度模式。Photoshop几乎所有的功能都支持灰度模式。

  3. RGB模式

     RGB模式是数码图像中最重要的一个模式，Photoshop的全部功能都支持它，因为Photoshop就是以它为基础来开发的。显示屏上显示的颜色是RGB模式，电视屏幕也是RGB模式，所不同的它不是用数码而是用电平来描述的。扫描仪和数码相机都是捕捉RGB图像信息的。

     RGB模式是相加的模式，当R、G、B的值都达到最大值时，三色合成便成白色。

     RGB模式是24位颜色深度。它共有三个通道，每个通道都有8位深度。三个通道合成一起可生成1677万种颜色，我们也称之谓“真彩色”。

  4. RGBA

     RGBA是代表Red（红色） Green（绿色） Blue（蓝色）和 Alpha的色彩空间。虽然它有的时候被描述为一个颜色空间，但是它其实仅仅是RGB模型的附加了额外的信息。

     alpha通道一般用作不透明度参数。

  5. CMYK模式


### .Image

- Image.open()

  对于彩色图像，不管其图像格式是PNG，还是BMP，或者JPG，在PIL中，使用Image模块的open()函数打开后，返回的图像对象的模式都是“RGB”。而对于灰度图像，不管其图像格式是PNG，还是BMP，或者JPG，打开后，其模式为“L”。

  返回的就是image类
  
- 与数组互转

  数组转Image：`image = Image.fromarray(array)`

  Image转array：`imgL_array = np.array(image, dtype='float64')`

  注意dtype可以为float、uint、int转换时数值会变化

### .ImageChops()

- ImageChops.difference(image_one, image_two)


## CV2

- 读取

  ```python
  img = cv2.imdecode(np.fromfile(img_path, dtype=np.uint8), cv2.IMREAD_COLOR)
  ```

  读取中文路径图像

  img为**numpy数组**，

- 图片属性

  img.shape：例(616,894,3)表示图像高616，宽894，通道为3，图像为一个616*894\*3的numpy数组

  注：数组转图像也可以直接用一个numpy数组转换

- 显示图像

  ```python
  cv2.namedWindow("image", 0)	#设置为可变大小
  cv2.resizeWindow("image", 400, 225) # 设置初始窗口大小
  cv2.imshow('image',img)
  cv2.waitKey(0)
  cv2.destroyAllWindows()#dv2.destroyWindow(wname)
  ```

- 保存图像

  ```python
  cv2.imwrite(file，img，num)
  # num为 1到9 对应不同图像格式
  ```

  

## 并发

### GIL

![image-20210529102433355](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\image-20210529102433355.png)

- 全局锁作用

  解决数据完整性和状态同步（数据加锁）

- 如何规避

  1. 多线程

     在I/O密集时，线程释放GIL，实现CPU和IO并行，大幅提升速度

     在CPU密集型时，会拖慢速度，因为切换线程带来开销，而又只允许一个线程进行运算

  2. 多进程

     multiprocessing实现并行，提升速度

### 多线程

1. 函数准备（任意）

   ```python
   def my_func(a,b):
   	...
   ```

2. 创建线程

   ```python
   import threading
   
   t = threading.Thread(target=my_func, args=(100, 200))
   ```

3. 启动线程

   ```python
   t.start()
   ```

4. **等待**结束

   ```python
   t.join()
   ```

#### 生产者消费者

一个线程生产，另一个消费

- queue.Queue

  多线程之间的、线程安全的数据通信

  ```python
  def producer(origin_queue:queue.Queue, produce_queue:queue.Queue):
      origin = origin_queue.get()
      # 对origin数据操作
      produce = ...origin..
      produce_queue.put(produce)
      
  def consume(produce_queue:queue.Queue):
      produce =  produce_queue.get()
      #	对produce数据进行消费
      ...
  threading.Thread(target=producer, args=(q1,q2))
  threading.Thread(target=consume, args=(q2))
  ```

#### 线程安全

```python
import threading 

lock1 = threading.lock()
lock2 = threading.lock()


with lock1:
    #do something
```

#### 线程池

- 原因

  ![image-20210529133655710](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\image-20210529133655710.png)

  新建线程和回收线程都需要分配资源，费时

  若可以重用线程，则可以减去开销

- 使用语法

  ```python
  from concurrent.futures import ThreadPoolExecutor, as_completed
  
  with ThreadPoolExecutor() as pool:
      results = pool.map(func, args)
      # 每个func放入线程池中作为线程，args为传入的参数，与func一一对应，results和args一一对应
  ```

  

#### 补（计时模块）

```python
import time
start = time.time()
print(start)
...
end = time.time()
print(end)
```

### 多进程

![	](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\image-20210529121937031.png)



## import

import时

绝对导入：

- `import module_name`。即import后直接接模块名。

  Python会在两个地方寻找这个模块，第一是sys.path（通过运行代码`import sys; print(sys.path)`查看），os这个模块所在的目录就在列表sys.path中，一般安装的Python库的目录都可以在sys.path中找到（前提是要将Python的安装目录添加到电脑的环境变量），所以对于安装好的库，我们直接import即可。第二个地方就是运行文件（这里是m1.py）所在的目录，因为m2.py和运行文件在同一目录下，所以上述写法没有问题。

- `from package_name import module_name`。一般把模块组成的集合称为包（package）。与第一种写法类似，Python会在sys.path和运行文件目录这两个地方寻找包，然后导入包中名为module_name的模块。

相对导入：

- `from . import module_name`。导入和自己同目录下的模块。
- `from .package_name import module_name`。导入和自己同目录的包的模块。
- `from .. import module_name`。导入上级目录的模块。
- `from ..package_name import module_name`。导入位于上级目录下的包的模块。
- 当然还可以有更多的`.`，每多一个点就多往上一层目录。



非运行入口文件：a1.py 导入 a2.py，而a2.py又导入a3.py，则a2.py为非运行入口文件，导入a3.py时用绝对导入可能出错

```
python -m Tree.m1
```
