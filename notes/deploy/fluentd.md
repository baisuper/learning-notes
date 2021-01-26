+++
title = "监控部署"
description = "监控部署"
date = "2017-08-01"
draft = false
weight = 16
+++
# 一，概览

# 二，部署

### 2.1 前置条件  
- 安装docker  
- 安装docker-compose  


### 2.2 部署


***克隆项目***  
```
git clone https://github.com/qihaiyan/fluentd-boot.git;
cd fluentd-boot
```

***编辑docker-compose.yml文件***  
```
es:
  image: elasticsearch:6.4.2
  volumes:
    - ./es:/usr/share/elasticsearch/data
    - ./elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml
  ports:
    - 9200:9200
    - 9300:9300

kibana:
  image: kibana
  ports:
    - 5601:5601
  links:
    - es:elasticsearch

fluentd:
  build: fluent-es/
  ports:
    - 24224:24224
  links:
    - es:es
```
配置文件中启用了3个容器，分别是elasticsearch、kibana、fluentd。其中elasticsearch、kibana直接从仓库中下载，fluentd是自己建立的容器