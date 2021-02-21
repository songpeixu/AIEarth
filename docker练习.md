# Docker练习场学习笔记
第一次尝试参加阿里的比赛，也是萌新一枚，对docker是首次接触，主要参考了如下两篇文章：
[https://tianchi.aliyun.com/competition/entrance/231759/tab/174?spm=5176.12281978.0.0.377222326z7lSg](https://tianchi.aliyun.com/competition/entrance/231759/tab/174?spm=5176.12281978.0.0.377222326z7lSg)
[https://tianchi.aliyun.com/forum/postDetail?spm=5176.12586969.1002.6.51df41273vTDuA&postId=165595](https://tianchi.aliyun.com/forum/postDetail?spm=5176.12586969.1002.6.51df41273vTDuA&postId=165595)

## 一、安装Docker环境
我是用的windows，所以就主要写windows啦：
Windows：[https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)

我觉得可能是运气比较好，一路点next就成功了，这是中间打开Docker时说缺少linux内核不能启动，但是Docker给了个链接，点进去下载重启就可以了。

## 二、开通阿里云镜像服务
ok，接下来还是抄作业：
阿里云容器镜像服务 [https://www.aliyun.com/product/acr?](https://www.aliyun.com/product/acr?）
免费开通镜像托管，本次练习任务请将仓库地域选择**深圳**（AIEarth和Docker练习场选深圳都可以）。建议设置私有仓库，并一定牢记仓库密码，后续提交需要使用。
![](https://tianchi-public.oss-cn-hangzhou.aliyuncs.com/public/files/forum/156974381981245981569743819615.png)
切换标签页到命名空间，创建地址唯一的命名空间
![](https://tianchi-public.oss-cn-hangzhou.aliyuncs.com/public/files/forum/156974384416019021569743844047.png)
根据任务/比赛要求选择对应的地域（本次练习选择 **深圳**），其他的按照自己需求选择或填写。
![](https://tianchi-public.oss-cn-hangzhou.aliyuncs.com/public/files/forum/156974386444455191569743864270.png)
选择代码源为本地仓库，灵活度大，完成创建。
![](https://tianchi-public.oss-cn-hangzhou.aliyuncs.com/public/files/forum/156974389164783061569743891566.png)
点击**管理**，可查看仓库的基本信息，以及**登录**、**拉取**、**推送**等简单操作的指南。
![](E:/Github/pic/pullpush.png)
拉取，推送，公网地址一定要注意，之后会经常用到

## 三、 拉取镜像并编译文件
继续抄作业，在自己创建的文件夹路径下，打开cmd或者powershell都可以
### 1.首先输入命令登录:
`docker login --username your_username --password your_password your_registry_url`
密码就是刚才的密码
### 2.然后拉取阿里云镜像：
`docker pull registry.cn-shanghai.aliyuncs.com/tcc-public/python:3`
### 3.在之前创建的文件夹路径下，准备好几个文件：
我直接复制粘贴了哦
Dockerfile文件：
```
3.2.5 编写Dockerfile文件（有多种方法创建Dockerfile文件，最简单的方法就是用文本编辑器生成一个.txt文件，把后缀去掉）：
# Base Images
## 从天池基础镜像构建
FROM registry.cn-shanghai.aliyuncs.com/tcc-public/python:3

## 把当前文件夹里的文件构建到镜像的根目录下（.后面有空格，不能直接跟/）
ADD . /

## 指定默认工作目录为根目录（需要把run.sh和生成的结果文件都放在该文件夹下，提交后才能运行）
WORKDIR /

## Install Requirements（requirements.txt包含python包的版本）
## 这里使用清华镜像加速安装
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt

## 镜像启动后统一执行 sh run.sh
CMD ["sh", "run.sh"]
```

requirements.txt文件代码如下：
```
pandas==0.25.1
numpy==1.19.4
```

run.sh文件：
`python hello.py`

hello.py文件：
```
!/usr/bin/env python
# coding: utf-8
import pandas as pd
import numpy as np
import json
#data=np.random.randint(1,100,200)
#data=pd.DataFrame(data)
#data.to_csv("./tcdata/num_list.csv",index=False,header=False)
data=pd.read_csv("/tcdata/num_list.csv",header=None)#天池python镜像默认包含此文件，自己测试用如下指令
#data=pd.read_csv("./tcdata/num_list.csv",header=None)

#第一题
result_1="Hello world"
#第二题
result_2=0
for i,num in enumerate(data[0]):
    result_2+=num
#第三题
data.sort_values(by=0,ascending=False,inplace=True)
result_3=data[0][:10]
result_3=list(result_3)

result={"Q1":result_1,
        "Q2":result_2,
        "Q3":result_3
       }
with open('result.json', 'w', encoding='utf-8') as f:
json.dump(result, f)
```

差不多就是下面这张图一样
![](E:/Github/pic/file.png)

## 四、构建竟然并推送
执行代码`docker build -t registry.cn-shenzhen.aliyuncs.com/test_for_tianchi/test_for_tianchi_submit:1.0 .`
注意：`registry.~~~`是上面创建仓库的公网地址，用自己仓库地址替换。地址后面的`：1.0 .`为自己指定的版本号

构建完成后可先验证是否正常运行，正常运行后再进行推送。
CPU镜像：`docker run your_image sh run.sh`
GPU镜像：`nvidia-docker run your_image sh run.sh`

推送到镜像仓库 `docker push registry.cn-shenzhen.aliyuncs.com/test_for_tianchi/test_for_tianchi_submit:1.0`
如果这步出错，可能你没有登录，按照仓库里描述操作登录即可。

## 五、提交验证运行结果
在左侧【提交结果】中填写推送的镜像路径、用户名和密码，即可提交。根据【我的成绩】中的分数和日志可以查看运行情况。
![](https://tianchi-public.oss-cn-hangzhou.aliyuncs.com/public/files/forum/157709457395764771577094573908.png)
