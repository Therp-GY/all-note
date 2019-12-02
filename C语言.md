# C语言

## 函数

- 申请空间

  ```c
  int *tmp = (int *)malloc((t)*sizeof(int));
  ```

  即申请了t个int大小的tmp空间。

  **！！！注意free掉，不然一直存在**

- memcp函数

  ```c
  void *memcpy(void *str1, const void *str2, size_t n)
  ```

  - <string.h>
  - **str1** -- 指向**用于存储**复制内容的目标数组，类型强制转换为 void* 指针。
  - **str2** -- 指向要复制的**数据源**，类型强制转换为 void* 指针。
  - **n** -- 要被复制的**字节数**。注意要 * sizeof()；
  - 该函数返回一个指向目标存储区 str1 的指针。

- strcmp

  判断字符串是否相等

  若str1=str2，则返回零；若str1<str2，则返回负数；若str1>str2，则返回正数

- strcpy

  char *strcpy(char *str1, const char *str2);

  把字符串str2(包括'\0')拷贝到字符串str1当中，并返回str1

### 函数返回字符串

如果直接在函数中定义 char *a，返回a，因为a为局部变量，在主函数中会释放掉，因此不行，需要用以下几种方式。

- 【使用最多】传入指针，返回这个指针，即直接操作传入的变量的地址上的内容
- 在函数中malloc一个局部变量并且返回它
- 返回一个静态局部变量 static char name[10]
- 使用全局变量

### 函数传入

- 传入二维数组：char[]\[a] a必须填写
- 传入变量写char *说明的是我要改变这个字符串量，如果你此时传入"asd"等常量就会警告，因为这是常量是无法修改的，这时候你应该将传入变量改为const char *

## define

- 代替函数

  #define isDigit(a) (a >= '0' && a <= '9') 即可表示数字

## 文件操作

- c = fgetc（fp）一个一个字符读，读取指针指的字符并且指针指向下一位
- fseek()   读文件将现在指针移动，可以将读到的字符前移或者后移
  - ​	fseek(IN, -1, SEEK_CUR); 前移一位
  - ​	fseek(IN, -1, SEEK_CUR);后移一位
    - 当读到文件结尾，指针就不会移动了

## 头文件问题

### 作用

- 方便开发:包含一些文件需要的共同的常量,结构,类型定义,函数,变量申明；

- 使函数的作用域从函数声明的位置开始，而不是函数定义的位置(实践总结)

- 提供接口:对一个软件包来说可以提供一个给外界的接口(例如: stdio.h)。

### 该有的和不该有的

- h文件里应该有什么:常量,结构,类型定义,函数,【变量申明】。

  **变量声明最好也不要有，因为很多.c文件都可以包含.h文件，也就是说这个变量会在很多.c文件中存在一个副本。假如这是一个多文件项目，在连接阶段，连接器就会抱怨存在多个相同变量名的全局变量，导致连接出错。**

- h文件不应该有什么:变量定义, 函数定义。

  **不要在头文件中定义形如int size = 32等等操作，不要定义变量**

### 函数声明

在.h文件中**声明函数**，在别的文件中**定义函数**，那么只要include头文件的所有.c文件都可以使用这个函数，这个函数根源就是.h声明的。

### 其它细节

- #ifndef	

  使用是为了防止头文件重复引用，只要def过就不再引入头文件

- 头文件中不要做具体函数定义而做抽象定义，因为可能导致链接时重复定义问题。

### extern

- 基本解释

  extern可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。此外extern也可用来进行链接指定。

- extern 变量

  - 注意事项

    - extern 说明的东西得和所要引用的东西**严格相同**。

      例如：在一个源文件里定义了一个数组：char a[6];

      在另外一个文件里用下列语句进行了声明：extern char *a；

      这是**错误**的。

- extern 函数声明

  - 意义

    如果函数的声明中带有关键字extern，仅仅是暗示这个函数可能在别的源文件里定义，没有其它作用。

## 数组下标计数

- arr[low] -> arr[high] 有 high - low + 1 个 元素

- arr[t++]操作一次   (t<= len)

  则当t == len时并未操作完成，而是需要再操作一次即当 t > len 才算完成

- arr[t]  等价  arr + t

## typedef struct

```c
typedef struct Student
{
int a;
}Stu;
```

于是在声明变量的时候就可：Stu stu1;(如果没有typedef就必须用struct Student stu1;来声明)
这里的Stu实际上就是struct Student的别名。Stu==struct Student

```c
typedef struct
{
int a;
}Stu;
```

另外这里也可以不写Student（于是也不能struct Student stu1;了，必须是Stu stu1;）

```c
typedef struct{
	int size;
	HashNode *table;
}HashMap,*hashmap;
```

- 结构HashMap, 指向结构的指针*hashmap

## trick

- ctrl+f 搜索并且替换
- 录入书签方便跳转