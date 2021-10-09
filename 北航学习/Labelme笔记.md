# Labelme

## 安装

1. anaconda安装

2. ```cmd
   conda create --name=labelme python=3.6	#conda开一个python环境并命名为labelme
   activate labelme	#进入该环境
   pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyqt5	#清华源下pyqt5
   pip install labelme		#安装lableme
   ```

- 注意网络问题可能导致报错，可切换网络或者使用别的源

- 关于anaconda的[入门解释](https://www.jianshu.com/p/eaee1fadc1e9) 

  

## 使用

```cmd
#anaconda的prompt下
activate labelme	#进入python环境
labelme		#打开labelme
```

### 标注

opendir打开文件夹进行标注。

- 分割数据集

  标注完成后save生成相应jason文件，包含相应label和标注信息

  

### 批量化操作

```
https://blog.csdn.net/yql_617540298/article/details/81110685
```

- ```python
  AttributeError: module 'labelme.utils' has no attribute 'draw_label'
  ```

  https://blog.csdn.net/qq_34997364/article/details/105152927

# 学习

- python相关基础操作 + 文件处理  + 图像处理