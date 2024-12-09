# Docker Images Pusher

使用Github Action将国外的Docker镜像转存到阿里云私有仓库，供国内服务器使用，免费易用<br>
- 支持DockerHub, gcr.io, k8s.io, ghcr.io等任意仓库<br>
- 支持最大40GB的大型镜像<br>
- 使用阿里云的官方线路，速度快<br>

视频教程：https://www.bilibili.com/video/BV1Zn4y19743/

作者：**[技术爬爬虾](https://github.com/tech-shrimp/me)**<br>
B站，抖音，Youtube全网同名，转载请注明作者<br>

## 使用方式


### 配置阿里云
登录阿里云容器镜像服务<br>
https://cr.console.aliyun.com/<br>
启用个人实例，创建一个命名空间（**ALIYUN_NAME_SPACE**）
![](/doc/命名空间.png)

访问凭证–>获取环境变量<br>
用户名（**ALIYUN_REGISTRY_USER**)<br>
密码（**ALIYUN_REGISTRY_PASSWORD**)<br>
仓库地址（**ALIYUN_REGISTRY**）<br>

![](/doc/用户名密码.png)


### Fork本项目
Fork本项目<br>
#### 启动Action
进入您自己的项目，点击Action，启用Github Action功能<br>
#### 配置环境变量
进入Settings->Secret and variables->Actions->New Repository secret
![](doc/配置环境变量.png)
将上一步的**四个值**<br>
ALIYUN_NAME_SPACE,ALIYUN_REGISTRY_USER，ALIYUN_REGISTRY_PASSWORD，ALIYUN_REGISTRY<br>
配置成环境变量

### 添加镜像
打开images.txt文件，添加你想要的镜像 
可以加tag，也可以不用(默认latest)<br>
可添加 --platform=xxxxx 的参数指定镜像架构<br>
可使用 k8s.gcr.io/kube-state-metrics/kube-state-metrics 格式指定私库<br>
可使用 #开头作为注释<br>
![](doc/images.png)
文件提交后，自动进入Github Action构建

### 使用镜像
回到阿里云，镜像仓库，点击任意镜像，可查看镜像状态。(可以改成公开，拉取镜像免登录)
![](doc/开始使用.png)

在国内服务器pull镜像, 例如：<br>
```
docker pull registry.cn-hangzhou.aliyuncs.com/shrimp-images/alpine
```
registry.cn-hangzhou.aliyuncs.com 即 ALIYUN_REGISTRY(阿里云仓库地址)<br>
shrimp-images 即 ALIYUN_NAME_SPACE(阿里云命名空间)<br>
alpine 即 阿里云中显示的镜像名<br>

### 多架构
需要在images.txt中用 --platform=xxxxx手动指定镜像架构
指定后的架构会以前缀的形式放在镜像名字前面
![](doc/多架构.png)

### 镜像重名
程序自动判断是否存在名称相同, 但是属于不同命名空间的情况。
如果存在，会把命名空间作为前缀加在镜像名称前。
例如:
```
xhofe/alist
xiaoyaliu/alist
```
![](doc/镜像重名.png)

### 定时执行
修改/.github/workflows/docker.yaml文件
添加 schedule即可定时执行(此处cron使用UTC时区)
![](doc/定时执行.png)

```
redis:7.2.5
redis:7.0.15
redis:6.2.14
redis:6
redis:7.2.4
redis:6.2.13
redis:5.0

mysql:8.0.35
mysql:8.0.34
mysql:8.0.33
mysql:8.0.29
mysql:5.7.37
mysql:5.7.29

nacos/nacos-server:v2.3.2
nacos/nacos-server:v2.3.1
nacos/nacos-server:v1.4.7
nacos/nacos-server:v2.3.0
nacos/nacos-server:v2.2.3
nacos/nacos-server:v1.4.6
nacos/nacos-server:v1.4.3

elasticsearch:8.14.3
kibana:8.14.3
logstash:8.14.3
elasticsearch:8.14.2
kibana:8.14.2
logstash:8.14.2
elasticsearch:7.17.22
kibana:7.17.22
logstash:7.17.22
elasticsearch:7.17.21
kibana:7.17.21
logstash:7.17.21
elasticsearch:7.17.15
kibana:7.17.15
logstash:7.17.15
elasticsearch:6.8.13
kibana:6.8.13
logstash:6.8.13
elasticsearch:6.8.12
kibana:6.8.12
logstash:6.8.12

bitnami/minio:2024.7.13
bitnami/minio:2024.7.10
bitnami/minio:2024.6.28
bitnami/minio:2023.12.7
bitnami/minio:2023.12.6
bitnami/minio:2023.11.6

rabbitmq:3.13.4-management
rabbitmq:3.13-management
rabbitmq:3.13.2-management
rabbitmq:3.13.1-management

emqx:5.7.1
emqx:5.6.1
emqx:5.4.1
emqx:5.3.2

bitnami/mongodb:7.0
bitnami/mongodb:7.0.11

portainer/portainer-ce:2.20.3
portainer/portainer-ce:2.20.2
portainer/portainer-ce:2.19.3

nginx:1.27.0
nginx:1.26.1

bitnami/kafka:3.3.2
bitnami/kafka:3.3
bitnami/kafka:3.4.1
bitnami/kafka:3.6.1
bitnami/kafka:3.6.2
bitnami/kafka:3.7.1

apache/rocketmq:5.2.0
apache/rocketmq:5.1.4
apache/rocketmq:5.1.3
apache/rocketmq:5.1.2
apache/rocketmq:4.9.5
apache/rocketmq:4.6.0

apache/activemq-artemis:2.35.0
apache/activemq-artemis:2.34.0
apache/activemq-artemis:2.31.2
apache/activemq-artemis:2.30.0

apache/skywalking-ui:10.0.1
apache/skywalking-oap-server:10.0.1
apache/skywalking-ui:10.0.1-java21
apache/skywalking-oap-server:10.0.1-java21
apache/skywalking-ui:10.0.1-java17
apache/skywalking-oap-server:10.0.1-java17
apache/skywalking-ui:9.7.0
apache/skywalking-oap-server:9.7.0
apache/skywalking-ui:9.6.0
apache/skywalking-oap-server:9.6.0
apache/skywalking-ui:9.5.0
apache/skywalking-oap-server:9.5.0
apache/skywalking-ui:8.8.1
apache/skywalking-oap-server:8.8.1
apache/skywalking-ui:8.6.0
apache/skywalking-oap-server:8.6.0-es7
apache/skywalking-oap-server:8.7.0-es6

xuxueli/xxl-job-admin:2.4.2
xuxueli/xxl-job-admin:2.4.0
xuxueli/xxl-job-admin:2.3.0
xuxueli/xxl-job-admin:2.2.0
xuxueli/xxl-job-admin:2.0.2

bladex/sentinel-dashboard:1.8.8
bladex/sentinel-dashboard:1.8.4
bladex/sentinel-dashboard:1.8.0
bladex/sentinel-dashboard:1.7.2
bladex/sentinel-dashboard:1.4.2

seataio/seata-server:1.8.0
seataio/seata-server:1.8.0.2
seataio/seata-server:2.0.0
seataio/seata-server:1.7.0
seataio/seata-server:1.6.0
