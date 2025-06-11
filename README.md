1. 此版本Dockerfile 支持Pycharm PRO 2025.1 版本

2. 镜像启动后可以在容器中访问本机的 Cuda 环境
3. 到期后(30天),重启个新的容器,即可再次试用30天
4. 此版本仅在Ubuntu Desktop 环境下通过xhost 跑通,其他环境自行测试
5. 每次重启新容器都会重置预设的配置和插件(预设不多,如有需要自行更改插件包和配置包下载目录)
6. 实现原理:利用docker 无限重置系统环境,达到无限次重试Pycharm Pro 目的

![image-20250611212018620](https://collection-data.bj.bcebos.com/jiaohaicheng/selfspace/142ee9e7_033f_4ed7_916a_c832b5770804/image-20250611212018620.png?authorization=bce-auth-v1%2F359794b9ccff4c03a01bdaaf0ede3be2%2F2025-06-11T13%3A20%3A25Z%2F-1%2F%2F9ea30eff6b04d97e9374519c049e399e27b9d0db7f31bd775886a05ee0379f84)

![image-20250611212039478](https://collection-data.bj.bcebos.com/jiaohaicheng/selfspace/787bc85b_e15b_4f18_bfec_ac89ca0112eb/image-20250611212039478.png?authorization=bce-auth-v1%2F359794b9ccff4c03a01bdaaf0ede3be2%2F2025-06-11T13%3A20%3A45Z%2F-1%2F%2F59c8ee4e7e9a099ddfd032943b1609953fbf119a0063bd1f0689d399620e6d44)
