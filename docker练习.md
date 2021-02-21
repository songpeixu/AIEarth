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
![](file:///E:/Github/%E5%9B%BE%E7%89%87/pullpush.png)
