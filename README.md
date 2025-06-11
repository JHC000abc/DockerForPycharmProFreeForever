1 .此版本Dockerfile 支持Pycharm PRO 2025.1 版本
2. 镜像启动后可以在容器中访问本机的 Cuda 环境
3. 到期后(30天),重启个新的容器,即可再次试用30天
4. 此版本仅在Ubuntu Desktop 环境下通过xhost 跑通,其他环境自行测试
5. 每次重启新容器都会重置预设的配置和插件(预设不多,如有需要自行更改插件包和配置包下载目录)
6. 如不想自行build 可以采用 pycharm-container.tar 文件 直接load
7. 实现原理:利用docker 无限重置系统环境,达到无限次重试Pycharm Pro 目的

