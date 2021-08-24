# 安装

- sdk

  SDK相当于**开发集成工具环境**，API就是数据接口。在SDK环境下调用API数据。

  实际上SDK包含了API的定义，API定义一种能力，一种接口的规范，而SDK可以包含这种能力、包含这种规范。但是SDK又不完完全全只包含API以及API的实现，它是一个软件工具包，它还有很多其他辅助性的功能。

  SDK 包含了使用 API 的必需资料，所以人们也常把仅使用 API 来编写 Windows 应用程序的开发方式叫做“SDK编程”。

  - sdk路径

    可以在setting中设置sdk的路径

- avd模拟器

  avd

# 工程解析

## 结构

![img](https://www.runoob.com/wp-content/uploads/2015/06/18951197.jpg)

### 资源

### 使用资源

res文件夹中所有的资源文件都会在R.java文件下生成对应的资源id，我们可以直接通过id访问到对应的资源。使用情况有两种：java代码中的使用（响应使用）和xml代码的使用（布局使用）

- java代码使用

  - 文字

    ```java
    txtName.setText(getResources().getText(R.string.name)); 
    ```

  - 图片

    ```java
    imgIcon.setBackgroundDrawableResource(R.drawable.icon); 
    ```

  - 颜色

    ```java
    txtName.setTextColor(getResouces().getColor(R.color.red)); 
    ```

  - 布局

    ```java
    setContentView(R.layout.main);
    ```

  - 空间

    ```java
    txtName = (TextView)findViewById(R.id.txt_name);
    ```

- xml代码使用

  通过@xxx即可得到

  - 获取文本和图片

    ```xml
    <TextView android:text="@string/hello_world" android:layout_width="wrap_content" android:layout_height="wrap_content" android:background = "@drawable/img_back"/>
    ```

## 主要文件解析

- Activity:是一个人机交互程序，相当于人和机器操作的桥梁，类似于shell，在里面写Java代码，从而达到想要实现的业务处理。

  ---》对应src\main\java文件夹中的各类代码

- activity_main.xml:是Android界面显示的视图，所有的配置控件，各种控件可以通过这里进行设计。

  ---》对应src\main\res文件夹里的代码

- AndroidManifest.xml：主配置文件，用于配置各个组件的访问权限。

- R.java:简单说就是android_main.xml里的控件的id号，方便在MainActivity里找到id来确定这个控件，从而做出业务处理。

- app:通常Android的各个组成部分放在此目录里，其中res存放一些资源文件，如图片、layout、values 等资源。

### 理解

：activity_main.xml配置视图，里面的部件可以考andriod studio 的可视化窗口直接布置，并设置id。在Activity进行用户操作程序，响应用户操作时就可以靠id调用部件并加以改变从而响应用户操作。（资源的使用就可以参照上一节）

UI

# andriod的UI解析

- 结构

  在Android APP中，所有的用户界面元素都是由View和ViewGroup的对象构成的。View是绘制在屏幕上的用户能与之交互的一个对象。而ViewGroup则是一个用于存放其他View（和ViewGroup）对象的布局容器。

- xml完成布局

  XML类似与HTML 使用XML元素的名称代表一个View。所以< TextView >元素会在你的界面中创建一个TextView控件，而一个< LinearLayout >则会创建一个LinearLayout的容器

![1567996771730](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1567996771730.png)

## 理解

- activity文件加载res中的xml来显示界面给用户
- resource下的AndriodMainifest.xml来定义所有的activity文件并且布置他们
- drawable等等文件夹中的都是资源，可以供xml调用（如当作背景之类的）
- 核心就是java代码中导入res中的xml文件

```java
package jay.com.example.firstapp;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //activity_main就是布局的xml文件
    }
}
```

![img](https://www.runoob.com/wp-content/uploads/2015/06/activity_main.jpg)

## LinearLayout线性布局

- linearLayout的布局都是相对于父布局的，即xml中它所在的大括号下进行布局，比如match-parent就是匹配父布局

## RelativeLayout相对布局

- 它有相对同级布局的相对位置选项，通过调用别的布局来确定自己的位置

## TextView

同图片

## Button

- Button是TextView的子类

- 背景等属性可以插入图片，background属性即可点击并且插入图片。（类如drawable等等实际上就是定义的资源，可以用来作为背景等等作用）在xml中用 @包名/文件名引用

  - drawble中新建资源

    new->drawable resource file 建立selector资源，

    那么button中的属性就能引用它

- 事件

  仍然在xml中设置，如触发

## EditText

- 大小位置设计起来都差不多，并且都可以可视化设计
- 可以设计输入内容限制，比如只能输入邮件格式

# 监听

## 理解

- activity用于与用户的交互

## 基于监听的时间处理机制模型

事件监听机制中由**事件源**，**事件**，**事件监听器**三类对象组成
处理流程如下:
**Step 1:**为某个事件源(组件)设置一个监听器,用于监听用户操作
**Step 2:**用户的操作,触发了事件源的监听器
**Step 3:**生成了对应的事件对象
**Step 4:**将这个事件源对象作为参数传给事件监听器
**step 5:**事件监听器对事件对象进行判断,执行对应的事件处理器(对应事件的处理方法)

- 直接用匿名内部类

```java
public class MainActivity extends Activity {    
    private Button btnshow;    
    //定义button组件
    
    @Override    
    protected void onCreate(Bundle savedInstanceState) {    
        super.onCreate(savedInstanceState);    
        setContentView(R.layout.activity_main);    
        //加载布局
        
        btnshow = (Button) findViewById(R.id.btnshow); 
        //找到button组件，通过在xml中button的id找到它
        
        btnshow.setOnClickListener(new OnClickListener() {    
            //重写点击事件的处理方法onClick()    
            @Override    
            public void onClick(View v) {    
                //显示Toast信息    
                Toast.makeText(getApplicationContext(), "你点击了按钮", Toast.LENGTH_SHORT).show();    
            }    
        });    
    }        
} 
```

但是直接匿名内部类有太多重复代码

- 使用内部类

  可以在类中复用

  ```java
  public class MainActivity extends Activity {    
      private Button btnshow;    
      @Override    
      protected void onCreate(Bundle savedInstanceState) {    
          super.onCreate(savedInstanceState);    
          setContentView(R.layout.activity_main);    
          btnshow = (Button) findViewById(R.id.btnshow);    
          //直接new一个内部类对象作为参数    
          btnshow.setOnClickListener(new BtnClickListener());    
      }     
      //定义一个内部类,实现View.OnClickListener接口,并重写onClick()方法    
      class BtnClickListener implements View.OnClickListener    
      {    
          @Override    
          public void onClick(View v) {    
              Toast.makeText(getApplicationContext(), "按钮被点击了", Toast.LENGTH_SHORT).show();   
          }    
      }    
  } 
  ```

- 使用外部类

  外部新建类实现OnClickListener，复写onClick函数，从而在点击时响应。

  但是外部类还需要传入对象，这个对象就是在点击时要改变的对象。

  主activity中找到相应点击响应的部件（onClick的部件），其调用	响应部件.setOnClickListener(new MyClick(想改变对象))

- 直接使用activity作为事件监听器

  只需要让Activity类实现XxxListener事件监听接口,在Activity中定义重写对应的事件处理器方法
  eg:Actitity实现了OnClickListener接口,重写了onClick(view)方法在为某些组建添加该事件监听对象
  时,直接setXxx.Listener(this)即可

- 直接绑定标签

  xml文件中相应组件设置onclick=“方法名”属性

  activity中定义方法“方法名”并且传入view组件，然后内容写出所改变的内容

## 基于回调的事件处理机制

- 假设说事件监听机制是一种托付式的事件处理，那么回调机制则与之相反，对于基于回调的事件处理模型来说，事件源和事件监听器是统一的，或者说事件监听器全然**消失**了，当用户在**GUI控件**上激发某个事件时，控件自己特定的方法将会负责处理该事件。

## Handle消息传递机制

- Android为了线程安全，并不允许我们在UI线程外操作UI；很多时候我们做界面刷新都需要**通过Handler来通知UI组件更新**（还可以使用runOnUiThread()来更新，甚至更高级的事务总线）

- 为什么引入handle类

  ![1568823902930](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1568823902930.png)

- 执行handle流程图

  ![1568823932833](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1568823932833.png)

- 例子效果

  界面展现动画

## 监听EditText的内容变化

- 用途

  控制edittext在写入前后的效果（比如写入后提示文字消失，控制密码可不可见）

## 响应系统设置的事(Configuration类)

- 用途

  描述手机的配置信息，比如屏幕方向，触摸屏触摸放式等等

- 通过

  Configuration cfg = getResources().getConfiguration();

  获取配置然后调用cfg的函数即可修改

# 四大组件

## Activity

### 概念

Activity是一个应用程序的**组件**，他在屏幕上提供了一个**区域**，允许用户在上面做一些**交互性的操作**，比如打电话，照相，发送邮件，或者显示一个地图！**Activity可以理解成一个绘制用户界面的窗口，而这个窗口可以填满整个屏幕，也可能比屏幕小或者浮动在其他窗口的上方！**

### activity生命周期

![img](https://www.runoob.com/wp-content/uploads/2015/08/18364230.jpg)

### activity创建流程

![img](https://www.runoob.com/wp-content/uploads/2015/08/48768883.jpg)

### activity启动方式

即启动一个新的activity，在当前的activity下打开一个新的activity

用到intent

### 系统给我们提供的常见的Activity

我们只要复制粘贴即可使用它们

```java
//1.拨打电话
// 给移动客服10086拨打电话
Uri uri = Uri.parse("tel:10086");
Intent intent = new Intent(Intent.ACTION_DIAL, uri);
startActivity(intent);

等等
```

### Activity间的数据传递

- 使用Bundle存储数据，使用intent.putExtra来将数据传递到下一个activity中

- Bundle主要用于传递数据；它保存的数据，是以key-value(键值对)的形式存在的。

![img](https://www.runoob.com/wp-content/uploads/2015/08/7185831.jpg)



- 多个Activity间的交互(后一个传回给前一个)

  ![img](https://www.runoob.com/wp-content/uploads/2015/08/67124491.jpg)

# Trick

- 新建project 

  其name即app名字，PackgeName即内部包结构

- 新建java文件会给出快速创建activity、service等等模块的选项

- manifest中的.java名，.其实就是所在的包名

# 绑定git

- File->Settings->Version Control(展开)->Git

  选定git路径

- File->Settings->Version Control(展开)->GitHub

  输入git账号密码

- VCS->Import into Version Control->Share Project on GitHub

  分享到github上面

# API DEMO

- 功能极度分层和清晰，有关于动画的讲解（Animation），有关于图片的讲解（Graphics），每个包对应一种开发的讲解。

# 真机运行

设置中连续点击版本号，开启开发者模式，在其中勾选USB调试，即可。andriod studio就会搜索到手机。run后自动安装到手机里你所开发的app

# 问题

- ERROR: Could not determine the class-path for class com.android.tools.idea.gradle.project.sync.ng.SyncAction.

  gradle版本不适配，在project选择andriod，在gradle中将所有配置都从我们成功的其它app中的gradle配置复制过去