ELK日志收集
#### 1. 背景
各个微服务系统的日志都保存在各自指定的目录中，如果这些微服务部署在不同的服务器上，那么日志文件也是分散在各自的服务器上。分散的日志不利于我们快速通过日志定位问题，我们可以借助ELK来收集各个微服务系统的日志并集中展示。

#### 2. 背景
ELK即Elasticsearch、Logstash和Kibana首字母缩写。Elasticsearch用于存储日志信息，Logstash用于收集日志，Kibana用于图形化展示。

#### 3. Docker Compose搭建ELK
```lua
3.1、增加内存

Elasticsearch默认使用mmapfs目录来存储索引。操作系统默认的mmap计数太低可能导致内存不足，我们可以使用下面这条命令来增加内存：

sysctl -w vm.max_map_count=262144

[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html)

3.2、创建Elasticsearch数据挂载路径

mkdir -p /elk/elasticsearch/data

3.3、对该路径授予777权限

chmod 777 /elk/elasticsearch/data

3.4、创建Elasticsearch插件挂载路径：

mkdir -p /elk/elasticsearch/plugins

3.5、创建Logstash配置文件存储路径：

mkdir -p /elk/logstash

3.6、创建配置文件

在/elk/logstash路径下创建logstash-elk.conf配置文件

vim /elk/logstash/logstash-elk.conf 如例

3.7、创建ELK docker compose文件存储路径

mkdir -p /elk/DockerCompos

在该目录下创建docker-compose.yml文件：

vim /elk/DockerCompos/docker-compose.yml 如例

3.8、启动

docker-compose up -d

3.9、查看启动状态

docker ps -a
```

#### 4. Logstash中安装json_lines插件
```lua
4.1、进入Logstash容器
使用如下命令进入到Logstash容器中：

docker exec -it logstash /bin/bash

4.2安装步骤
切换到/bin目录
cd /bin/

安装json_lines插件
logstash-plugin install logstah-codec-json_lines

退出
exit
```
#### 5. 访问Kibana http://IP:5601/
