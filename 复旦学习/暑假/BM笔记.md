 v相关

# 名词解释

- 综合管理平台

  - 业务系统

    例子：WLXM检测服务平台、数据资源管理与服务系统

    url：业务系统界面给出平台访问入口

    - 引擎

      归属：业务系统

      例子：深度解析引擎、关键词检测引擎等

      引擎也有专属服务器

      启动方式：

      - 数据输入

        对深度解析引擎，输入有：facebook、暗网、...

      - 数据输出

        对深度解析引擎，输出有msg_txt、msg_picf

        其中msg_picf是我们需要使用的，数据类型为日志，具体格式查看“综管文档”

      - 数据订阅、发布

        数据订阅：归属于任务，订阅的数据就是引擎数据输出

        数据发布：归属于任务，由引擎发布数据到总线，数据类型自己定义

  - 业务线

    例：WLXM检测 业务线1028

    - 任务

      例：WLXM检测任务1028

      归属：属于业务线

      可选择需要的业务系统和相应引擎

      - 规则

        归属：属于任务

        新建规则：

        1. 新建业务线
        2. 新建任务，属于某条业务线
        3. 新建规则，属于某个业务线-》任务-》规则分组

# 代码解读

核心业务流程

![image-20210527175702740](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\image-20210527175702740.png)

①**引擎注册**：由引擎提供注册信息，由平台管理员操作，在基础平台管理界面注册引擎，注册引擎需要系统的信息包括：引擎名称、引擎版本号、引擎秘钥、**引擎 IP 地址**、引擎输入数据类型、引擎输出数据类型、联系人、从属单位和引擎简介，经过验证字段合法性后，完成引擎的注册，引擎完成注册后，在平台反馈的完成页面中，会看到引擎初始化信息，包括**平台服务地址**、引擎编号、引擎版本号、引擎证书文件下载，引擎需要拷贝这些初始化信息并配置到引擎软件的配置参数中，完成引擎的初始化。

②**引擎认证**：引擎初始化后向基础平台发认证请求，认证请求需要携带正确的引擎编号、引擎版本号信息，基础平台收到认证请求，除了校验引擎编号、引擎版本号以外，还会校验引擎认证请求的 IP 是否是注册时填写的 IP，还会校验证书编号的一致性，如果校验通过，则完成引擎的合法通信连接建立，连接建立以后，会返回给引擎 Cookie 作为连接标识，后续请求后要携带此 Cookie 作为连接凭证。连接是有过期时间的，当连接过期后，对于过期连接的请求平台会返回"SESSION 过期，请重新登录"，此时需要引擎重新进行认证过程。

③**引擎获取配置**：引擎在完成认证接入以后，引擎需要通过心跳保活，在有配置下发的情况下，基础平台通过心跳返回给引擎相应的配置，引擎接收后本地生效，然后反馈配置生效状态给基础平台，引擎按照配置中的检测策略，完成具体的检测工作，实现对数据的检测能力。给引擎下发的配置具体分为四种类型，分别是**业务配置、命令、软件参数和检测策略**，业务配置是指引擎发布数据、订阅数据的数据类型，数据子类型等信息；命令是指重启，休眠等引擎指令；软件参数是指心跳周期、重试次数等引擎软件的配置参数；检测策略是指关键词、木马规则等检测规则。

④**引擎上报状态**：引擎向基础平台上报状态，包括系统基本状态、软件参数、审计日志和异常日志，字段详情见第五节状态接口，平台通过引擎上报的状态信息，全面建立引擎的展示视图。

⑤**引擎总线授权**：首先引擎在总线管理页面注册引擎，然后注册数据标签，最后选择开通对应数据标签的生产消费权限，具体操作见《总线接口》。

⑥**引擎总线接入**：引擎提交接入请求时应携带数字证书，使用数字证书与总线建立 HTTPS 连接。在请求报头参数中应包含 systemId。认证通过，总线在认证请求的响应包的报头中增加 Set-Cookie 参数设置会话标识（sessionId），引擎后续的所有请求都应携带该会话标识，会话标识失效后应重新进行认证；认证不通过，反馈认证失败理由（如“系统编码不合法”），具体接口见《总线接口》。

⑦**引擎消费日志**：引擎在总线上订阅数据（待检测日志），经过步骤③得到的配置检测以后产生告警（事件），引擎调用消息消费接口向总线发起针对指定标签的信息消费请求，以获取指定标签缓存在总线上的待分发信息。在请求报头参数中应包含 systemId、sessionId。具体接口见《总线接口》。

⑧**生产事件**：引擎向总线写回检测产生的事件消息，引擎调用消息上传接口针对指定的标签向总线上传消息。在请求报头参数中应包含 systemId、sessionId。支持以集合形式一次上传多个消息，消息集合不能大于 10MB。具体接口见《总线接口》。

## 配置文件

与总线交互：

```python
busip = http://192.168.0.215:18088
up_down_file_ip = http://192.168.0.213:18005
```

与平台交互：

```python
ip = http://192.168.0.22:9002
```

引擎（组件）ip：

```python
redis_ip = 192.168.0.17   即引擎所在服务器ip
```

消费格式：

**数据对象内容** + **数据标签内容**

```python
[consume_schema_map]
msg_picf = schema_File/msg_picf.json

[consume_xTag_map]
msg_picf = 3758096869
```

生产格式：



```python
[produce_schema_map]
log_ocr = schema_File/produce_schema_File/log_ocr.json

[produce_xTag_map]
log_ocr = {"task_id": [28798], "data_source": 10, "data_type": 3, "producer_id": 29001, "tag_version": "1.0", "data_subtype": 15896}

```



**理解：**

从总线上下载文件（图像）存储在引擎服务器中/tmp_dir/，下载图像信息（json）存储在redis中（redis中图像二进制码也可提取图像），我们可以从这俩个地方取图像进行检测，并且告警信息重新存储回redis中，总线再从redis中取出来上传

- init_eng.cfg

  引擎配置：引擎数据订阅ip（  `busip = http://192.168.0.215:18088`）（？）、数据订阅格式路径（`msg_picf = schema_File/msg_picf.json`）、数据发布格式路径（`log_ocr = schema_File/produce_schema_File/log_ocr.json`）

- logger.cfg

  与WLXM检测服务平台配置，包括：心跳、服务平台ip、总线ip、redis ip	

- recvFromPlatform.cfg

  与服务平台配置参数，仅有心跳间隔时间

## 格式文件

schema_File路径下

- 

## 日志文件

- handleFileNum

  /root/V0.5/Pic_ocr/code/connect/handleFileNum/

  log_ocr_num ： 告警信息个数

  msg_picf_num：处理图片数量
  
  job.txt：device_lib.py中处理多任务时用到，存储多任务列表

## init_eng



## device_lib

与基础平台（网络泄密服务平台）对接

- main

  ```python
  Device_interface.init()
  ```

- Device_interface.init()

  ```python
  class Device_interface:
      @classmethod
      def init(cls):
          global instance_error_list
          # os.system("sh start_eng.sh")
          device_auth()
          device_config()
          device_heartbeat()  # 启动之后先发次心跳
          h_thrd.start()  # 启动心跳上报的线程
          timer.start()  # 启动历史状态上报的线程
          audit_state_error_rule.start()
          #start_eng_thread.start()
  ```
  - device_auth()

    功能：引擎认证，获取会话session

  -  device_config()

    功能：引擎配置上报接口: 引擎上报给基础平台的配置信息，一般只有心跳频率

  - device_heartbeat() 

    功能：引擎心跳接口

  - h_thrd

    ```python
    h_thrd = threading.Thread(target=scheduler_fun, args=(session, heart_freq, device_heartbeat))  # 心跳线程
    ```

    - scheduler_fun()

      ```python
      def scheduler_fun()
      功能：任务解析函数。
          def nesting_scheduler()：
          ...
          :param session:    发送请求的session
          :param interval:    定时间隔时间
         :param execute_fun: 定时调用的函数名称
         :param args:      位置参数(如果需要)
         :return:
      
      ```

      包含def  task_analysis(**msg):用于任务解析，即对从心跳中获取到的任务进行解析

      - nesting_scheduler()

        处理device_heartbeat()返回的参数，对其返回的r.status_code, r.text进行处理，

        对单任务def task_analysis(**msg):

        - def task_analysis(**msg):

          ```python
          '''
          功能：对从心跳中获取到的任务进行解析
          参数：心跳返回的任务信息
          注意：任务有多种，引擎需要判断并提取对自己有效的规则，进行解析、生效。根据rule_type字段进行判断。举例：本引擎生效的rule_type=9
          '''
          ```

          最终将规则写到rule_expression.txt文件中去

        对多任务处理def multiple_info()：

        - def multiple_info():

          ```python
          '''
          功能：离线处理心跳下发的“多任务”。
          实现方式：接收到多任务，会在函数scheduler_fun中写入任务文件job.txt，然后在multiple_info()中读去任务文件job.txt，一条一条执行（ task_analysis），每执行一条删一条
          说明：任务分为：配置、规则/策略、命令、指令配置、多任务五种类型，其中“多任务”包括配置、规则/策略、命令中的一种到四种。
          '''
          
          ```

          

      - device_heartbeat()

        ```python
        '''
        功能：引擎心跳接口
        与引擎通信，返回心跳信息以及相应传输txt
        '''
        ...
        return r.status_code, r.text
        ```

        

## redis_server.py

代码部分主要分为主节点与从节点。主节点包括与总线、服务平台通信，将获取到的数据与告警进行解析处理，将待处理数据写入redis并从redis获取告警功能。从节点包括引擎本身，从redis获取待处理数据，将告警传回redis，规则获取功能。

- main()

  ```python
  def main():
      # threading.Thread(target=thread_control).start()
      initialization()
  ```

- initialization()

  ```python
  #若业务配置成功，即收到['general', 'produce_schema_map', 'produce_xTag_map', 'consume_schema_map', 'consume_xTag_map']
  print("=========业务配置完成，开启数据接收=========")
  start_thread(list(), "send_rule")
  
  all_producer = init_parameter.get_all_produce()
  print("当前生产数据类型包括：", all_producer)
  start_thread(all_producer, "upload")
  
  all_consumer = init_parameter.get_all_consumer()
  print("当前消费数据类型包括：", all_consumer)
  start_thread(all_consumer, "download")
  
  # all_producer 即 [produce_xTag_map]中的log_ocr = {"task_id": [28798], "data_source": 10, "data_type": 3, "producer_id": 29001, "tag_version": "1.0", "data_subtype": 15896}
  
  #all_consumer 即[consume_xTag_map]中的 msg_picf = 3758096869
                  
  ```

- start_thread()

  `threading.Thread(target=send_rule).start()` target指向开线程的函数

   all_msg_type_list 存储所有生产消费类别，例如 log_ocr、msg_picf等

  1.  若是“send_rule”直接创建线程

  2. 若是“upload”且 all_msg_type_list中没有生产该类

     ```python
     print("{}不存在列表{}中，开始创建线程去消费".format(msg_type, all_msg_type_list))
     threading.Thread(target=send_data_bus1, args=(msg_type,)).start()
     all_msg_type_list.append(msg_type)
     ```

  3. 若是“download”且 all_msg_type_list中没有消费该类

     ```python
     print("{}不存在列表{}中，开始创建线程去消费".format(msg_type, all_msg_type_list))
     consume_schema_file = init_parameter.get_consume_schema(msg_type)
     with codecs.open(os.path.join("", consume_schema_file), encoding='utf-8') as f:
     consume_schema = json.load(f)
     
     tag_name = os.path.join(os.path.dirname(os.path.abspath(__file__)), "schema_File",
     "tag_schema_consume_log.json")
     with codecs.open(tag_name, encoding='utf-8') as f:
     tag_schema = json.load(f)
     
     threading.Thread(target=get_data_bus, args=(msg_type, consume_schema, tag_schema)).start()
     all_msg_type_list.append(msg_type)
     ```

- send_data_bus1()

  ```python
  '''
  采用内置规则产生的告警上传
  1.从redis读取引擎检测完后的告警信息；
  2.将告警信息进行封装，生产到总线。
  '''
  ```

- send_data_bus()

  封装参考log_ocr.json格式分装，硬编码

  ```python
  '''
  1.从redis读取引擎检测完后的告警信息；
  2.将告警信息进行封装，生产到总线。
  '''
  ```

- get_data_bus()

  解析中硬编码，直接对照msg_picf.json中格式进行解析
  
  ```python
  '''
  1.从总线消费；
  2.解析；
  3.根据file_id下载文件到本地；
  4.将待检测json消息写入redis。
  '''
  ```

## redis_client2.py

`redis.StrictRedis(host=redis_ip, port='6379', decode_responses=True)`：建立与redis的连接

- def main()

  ```python
  def main():
      model = model_init()
      # threading.Thread(target=read_rule).start()
      # threading.Thread(target=get_data_redis, args=(model,)).start()  # 采用从管理平台下发的规则
      
      threading.Thread(target=get_redis_data, args=(model,)).start()  # 采用内置规则
      #  删除图片
      del_img_dir = "./tmp_dir/"
      threading.Thread(target=delete_img_scheduler, args=(del_img_dir,)).start()  # 可能会有问题
      # delete_img_scheduler("./tmp_dir/")
      #  删除日志
      del_log_dir = "./log/"
      threading.Thread(target=delete_log_scheduler, args=(del_log_dir,)).start()
  ```

- get_redis_data()

  先直接从总线上下载图像，若不行，则从引擎的服务器上的redis下载图像；

  下载后输入引擎的模型中进行预测，用`pred_result_analysis`预测（调用`ocr_result_analysis`预测绝密文字）；

  【在redis_client2中使用内置规则，而在redis_client中使用rule_expression中的外部规则】

  若产生告警，调用`send_data_redis(item_copy)`将告警保存到引擎的redis中
  
  ```python
  '''
  从redis消费数据，采用内置规则，启动引擎进行检测，如果为SM图片，则生产告警到redis中
  '''
  .....
  item = rc.brpop(consume_topic)[1]	# rc即为引擎redis，consume_topic即为msg_picf；移除并获取redis中msg_picf列表中第一个元素，即取出一张图片信息
  ....
  if file_type in file_type_dict.keys():
      .....
      if not os.path.exists(save_img_dir + save_img_name):
          #直接在引擎文件夹下搜索是否下载，没有下载则从redis中下载
          try:  # 直接从总线下载失败，尝试从redis中取出图片
              file_content = item_json['file_content']
              file_content = file_content[1:].encode("utf-8")
              file_content = base64.b64decode(file_content)
              img = Image.open(io.BytesIO(file_content))
              img = img.convert("RGB")  # 防止错误OSError: cannot write mode RGBA as JPEG
              ImageFile.MAXBLOCK = img.size[0] * img.size[1]  # 保存大图片
              img.save(save_img_dir + img_name)
              get_from_redis = True
              .....
              res = result_analysis(pred_result, "", use_device)
              # 预测结果
              .....
              	item_copy = copy.deepcopy(item_json)
                  item_copy["msg"] = msg
                  send_data_redis(item_copy)
                 	#	将告警上传到reids上面
  ```

## shouhu.sh

## main

- def main(test_data_path, step4_kind=1)

```python
test_data_dir = "../test_data/"
test_data_name = test_data_path.split('/')[-2]
step0_main(test_data_dir, test_data_name)
step1_main(test_data_tmp_path, step1_out_path)
```

## step0

- def step0_main(image_path, out_dir)

  ```python
  end_flag, pic_size = to_jpg(image_path, out_dir)
  return end_flag, pic_size
  ```

- def to_jpg(image_path, out_dir)

  把输入的图片（包括gif图片）都转换为jpg格式输入到out_dir中


## step1_2

作用：把图片分成10类

- def step1_2_init(use_cuda = False)

  `torch.load`：从文件中加载一个用torch.save()保存的对象

  `classifer_handle`：从state_dict中获取网络参数，生成分类模型

  `__dict__`：由此可见， 类的静态函数、类函数、普通函数、全局变量以及一些内置的属性都是放在**类__dict__**里的

- def step1_2_main(image_dir, image_out_dir, classifer_handle, use_cuda=True)

  把图像存储到 image_out_dir并判定类别

## step_3

作用：截取roi区域

- def main(image_path, out_dir, cnt=1)

  截取roi区域，固定比例固定区域简单截取

- def step3_main(step2_out_dir, step3_out_dir)

## step4_new

作用：检测并识别图片中的文本

psenet：是一种新的实例分割网络，它有两方面的优势。 首先，psenet作为一种基于分割的方法，能够对任意形状的文本进行定位.其次，该模型提出了一种渐进的尺度扩展算法，该算法可以成功地识别相邻文本实例（该算法在下文会详细介绍）。

cnn：**重点是如何对已经定位好的文字区域图片进行识别**

https://zhuanlan.zhihu.com/p/43534801

`crnn_handle`：crnn识别模型

```python
if crnn_type == "full_lstm" or crnn_type == "full_dense":
   crnn_net = FullCrnn(32, 1, len(alphabet) + 1, nh, n_rnn=2, leakyRelu=False, lstmFlag=LSTMFLAG)
elif crnn_type == "lite_lstm" or crnn_type == "lite_dense":
   crnn_net = LiteCrnn(32, 1, len(alphabet) + 1, nh, n_rnn=2, leakyRelu=False, lstmFlag=LSTMFLAG)

assert crnn_type is not None
crnn_handle = CRNNHandle(crnn_model_path, crnn_net, gpu_id=GPU_ID)
```

`text_handle`：文本检测模型

```python
if pse_model_type == "mobilenetv2":
	text_detect_net = PSENet(backbone=pse_model_type, pretrained=False, result_num=6, scale=pse_scale)

text_handle = PSENetHandel(pse_model_path, text_detect_net, pse_scale, gpu_id=GPU_ID)
```

`angle_handle`：角度预测网络

- def crnnRec(im, rects_re, leftAdjust=False, rightAdjust=False, alph=0.2, f=1.0)image-20210527141112458]



# 部署

- anaconda

  bash Anaconda3-2018.12-Linux-x86_64 

  版本？ 2018 ？ 2020

  使用source ~/.bashrc 更新环境变量

- redis

# 高斯混合模型

![image-20210703013104600](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\image-20210703013104600.png)

两个角度看高斯混合模型：

1. 多个高斯分布叠加，加权平均
2. 生成模型，即求x的概率P(x)为先取x对应某个高斯分布（z = 1，2，...，k），再求该高斯分布中x的概率，再将这些概率值相加即为P(x)

计算方法：给定x1，x2...xn，求P(z)以及每个高斯分布的参数

EM算法：

引入隐变量（γ_(t,k)）表示第t个样本xt采样于第k个高斯分布，因此可以将xt这个数据扩充为完全数据（xt，0，0，...，1，...，0）在索引k+1处取1，其它都为0表示样本xt采样于第k个高斯分布。

EM算法每一轮先估算每个样本的γ，得到γ后就可以计算每一轮的P(z)以及每个高斯分布的参数

**注意**：γ为每个样本归属于哪个高斯分布，而P(z)为每个高斯分布所占总体的比重（即所分配样本数的多少 ）



https://blog.csdn.net/jinping_shi/article/details/59613054/

https://blog.csdn.net/lin_limin/article/details/81048411

- 图像分割综述

  https://zhuanlan.zhihu.com/p/49512872

  



sklearn 

# 分类模型

