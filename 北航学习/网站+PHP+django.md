# 网站基本概念

- 服务器

  提供**计算服务**的设备。（处理器、硬盘、内存...）

  - web服务器：提供web服务（网站访问），需要安装web服务软件，Apache，tomcat....

- IP

  计算机网络相互连接进行通信而设计的协议

- 域名

  IP的便于人记住的名字

- DNS

  域名和IP形成映射用于机器读取域名然后得到IP

- 端口

  设备与外界通讯交流的出口，**虚拟端口**指计算机内部或交换机路由器内的端口(访问软件)

  local:端口  --》 DNS（localhost   127.0.0.1） --》 服务器电脑 --》 软件（服务）

## 静态网站访问原理

![1573441712376](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1573441712376.png)

- 本地DNS加速DNS的查找
- Apache web服务器软件

## 动态网站访问原理

![1573446391390](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1573446391390.png)

- 多了个解析动态文件的引擎

- 文件夹中的.php文件Apache无法直接访问，需要PHP引擎，它交给Apache html文件

# APACHE

## 结构

- bin

  windows下一些可执行文件

  - ab.exe

    用于压力测试，看很多人访问能否承载

  - http.exe

    关键，用于服务运行

    在任务管理器中有说明可以功能了

    用来查看apache具有哪些功能以及配置文件是否有错

    - http.exe -m

      <static> apache启动已经加载好

      <shared> 使用到的时候才加载

    - httpd.exe -t

      测试运行，若ok则可以正常使用

- conf

  配置文件

  - httpd.conf

    主配置文件

  - extra

    子配置文件

- htdocs

  apache默认的主机地址（网站根目录）

- modules

  模块：apache所有功能都是模块化的，想用哪个功能就用哪个模块

## 配置默认站点

- 确定服务器上访问地址，网站文件夹所在位置

  http.conf：DocumentRoot

- 方便用户使用名字访问对应的网站：给文件夹对应取个别名

  httpd.conf：ServerName

  【其后可能跟端口名，但是端口的监听也在httpd.conf：Listen中调节，所以这里可忽略】

- 实现DNS域名解析：通常默认站点都是本地DNS：hosts文件

  C:\Windows\System32\drivers\etc\hosts文件可以增加域名解析  IP - 域名

- 涉及apache配置文件的修改，需要重启apache

# Mysql

## 结构

- bin

  执行文件夹

- data

  数据存储文件夹

- my.ini

  配置文件

- bin\mysql.exe

  访问mysql服务器的客户端

- bin\mysqld.exe

  mysql服务

- bin\mysqldump.exe

  mysql备份服务端

## 访问流程

C\S架构，需要通过客户端访问服务端

1. 启动mysql客户端
2. 客户端访问服务端需要进行匹配寻找：连接认证（如qq，客户端服务端不一定同一台电脑）
   - 连接：IP和端口确认
   - -h 主机地址-----》-hlocalhost（可以是IP）
   - -P端口-----》-P3306
   - 认证：通过用户名和密码进入服务器
   - -u 用户名------》-urooy，不可省略
   - -p密码

## 问题

- 为了使用XAMPP的mysql，在注册表计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL的ImagePath

  中将我之前下载的mysql路径改为

  "E:\XAMPP\mysql\bin\mysqld" --defaults-file="E:\XAMPP\mysql\bin\my.ini" MySQL

- 由于环境变量中用的还是原来的mysql，因此在cmd中会进入原有mysql，而在XAMPP中会进入它的mysql

# XAMPP

这是个集成apache、mysql、php的集成环境，就不需要我们来配置了

# PHP

## 函数

- isset()

  检测变量是否设置

- $connect = mysqli_connect("localhost","root","GanYi319");

  获得数据库连接

- mysqli_select_db($connect,"film");

  选择数据库

- $result1 = mysqli_query($connect,$sql1)

  执行数据库语句

- $num1 = mysqli_num_rows($result1)

  返回查询条数

## 实例

- 网页跳转和信息发送

  ```html
  <form action="./registercheck.php" method="post">
  用户名: <input type="text" class="a" name="usrname">
  <br>
  昵称: <input type="text" class="a" name="usrnickname">
  <br>
  密码: <input type="text" class="a" name="usrpw1">
  <br>
  确认密码: <input type="text" class="a" name="usrpw2">
  <br>
  邮箱: <input type="text" class="a" name="usremail">
  <br>
  手机号: <input type="text" class="a" name="usrphone">
  <br>
  
  <center>
  <p>
  点击注册，开启新世界！
  </p>
  </br>
  <input type="submit" name="submit" value="注册">
  </center>
  ```

  接受信息添加到数据库

  ```php+HTML
  <?php 
      if(isset($_POST["submit"]) && $_POST["submit"] == "注册")
      {
  
          $username = $_POST["usrname"]; 
  	$usernickname = $_POST["usrnickname"];
          $userpsw1 = $_POST["usrpw1"];  
  	$userpsw2 = $_POST["usrpw2"];
  	$useremail = $_POST["usremail"];
  	$userphone = $_POST["usrphone"];
  
          if($username == "" || $usernickname == "" || $userpsw1 == "" ||$userpsw2 == "" ||$useremail == "" ||$userphone == "")
          {
              echo "<script>alert('有档案没有填完！不能留空哦'); history.go(-1);</script>";
          }   
          else   
          {     
              $connect = mysqli_connect("localhost","root","GanYi319");
  			echo"<script>alert('数据库连接成功！')</script>";
              if (!$connect){
                   echo"<script>alert('数据库连接失败！')</script>";
              }else{
  				
  			}
              mysqli_select_db($connect,"film");
  	    $sql0 = "SELECT * FROM user_list WHERE user_name LIKE '$username'";
  	    $result0 = mysqli_query($connect,$sql0);
  	    $num0 = mysqli_num_rows($result0);
  	    if($num0 != 0){
  		echo "<script>alert('已经有位爷用这个名字注册过了，对不住喽'); history.go(-1);</script>";
  	    }
  	    else if($userpsw1 != $userpsw2){
  		echo "<script>alert('爷你俩不一样的，不厚道啊！'); history.go(-1);</script>";
  	    }
  	    else{
  		$sql = "INSERT INTO `user_list` (`user_id`, `phone_num`, `pwd`, `nick_name`, `mail`,`user_name`) 
  			VALUES (NULL, '$userphone', '$userpsw1', '$usernickname', '$useremail', '$username')";
  		$result = mysqli_query($connect,$sql);
  		echo"<script>alert('插入成功！')</script>";
  		header("Location:registerok.php");
  	    }
          }
      }
      else
      {
          echo "<script>alert('注册未成功！'); history.go(-1);</script>";
      }
  ?> 
  ```

  

# djano

## 结构

![1574337272030](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1574337272030.png)

untitled2是整个项目

myapp是某个应用

## url

- myapp.views

  ```python
  from django.http import HttpResponse
  
  def  hello(request):
      return HttpResponse("GG!")
  ```

  定义各种界面上的响应，可用于myapp的url的调用，例如定义hello这个动作以供调用

- myapp.url

  ```python
  from django.conf.urls import url
  from django.urls import path
  
  from myapp import admin
  from . import views
  
  urlpatterns = [
      path('', views.hello, name='hello'),
  ]
  ```

  这个url上应该有什么响应，如调用hello方法，在界面上输出"GG！"

- untitle2/urls

  ```python
  from django.contrib import admin
  from django.urls import include, path
  
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('myapp/', include('myapp.urls')),
  ]
  
  ```

  函数 [`include()`](https://docs.djangoproject.com/zh-hans/2.1/ref/urls/#django.urls.include) 允许引用其它 URLconfs。每当 Django 遇到 :func：~django.urls.include 时，它会截断与此项匹配的 URL 的部分，并将剩余的字符串发送到 URLconf 以供进一步处理。

  我们设计 [`include()`](https://docs.djangoproject.com/zh-hans/2.1/ref/urls/#django.urls.include) 的理念是使其可以即插即用。因为投票应用有它自己的 URLconf( `myapp/urls.py` )，他们能够被放在 "/myapp/" ， "/fun_myapp/" ，"/content/myapp/"，或者其他任何路径下，这个应用都能够正常工作。









