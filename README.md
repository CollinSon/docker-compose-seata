# docker-compose-seata
## 软件环境

| 名称         | 版本   |
| ------------ | ------ |
| mysql        | 8      |
| nacos-server | v2.2.0 |
| seata        | 1.6.0  |

### 步骤：
1. 安装docker和安装docker-compose，怎么安装，自行百度   
2. git clone https://github.com/CollinSon/docker-compose-seata.git
3. 查询自己电脑的ip，例如mac输入ifconfig则可以查看到ip，类似192.168.0.8，然后修改.env里面的SEATA_IP     
4. 调整.env文件中的SEATA_IP 调整为部署的机器ip
5. 调整.env文件中的MYSQL_SERVICE_HOST 调整为部署的机器ip
6. 调整nacos/conf/application.properties中的mysql连接串，将jdbc连接串改为部署机器的ip
7. 调整seata.properties中的jdbc连接串
8. cd docker-compose-seata && docker compose up -d 
9. 打开nacos,http://**部署的ip**:8848/nacos, 账户：nacos 密码：nacos  
10. 点击新建命名空间，命名空间id:9fdcbe8r-4511-4a30-b5f0-7a4fab022ac7,名称为seata    
11. 进入配置列表，选择刚刚新建的seata命名空间，点击新建配置，Data ID输入为：seata.properties Group输入为：seata-server   
12. 复制seata/conf/seata.properties内容到配置内容里面，类型选择properties  

### 注意：
1、如果nacos启动失败，有可能是mysql初始化数据库数据还没有初始化完，可以使用下面命令重启    
`docker-compose restart seata-server`     
2、seata-service真的太坑了，k8s集群内的pod注册到nacos，是pod的ip，pod之间使用ip可以连接，但是用docker-compose去部署，拿到的也是容器内的ip，如果你的应用是在宿主机上，不是跟seata、nacos在同一个网段的容器，你必须制定SEATA_IP为你的宿主机ip，否则会连接不上   

