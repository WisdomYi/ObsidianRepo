docker版ElasticSearch镜像 : docker pull docker.elastic.co/elasticsearch/elasticsearch:8.17.5

``` sh
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.17.5
docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.17.5
```

``` sh
# 在es容器中执行
# 修改es密码
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
# 创建kibana访问token
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

单节点部署ES时 可以设置分片为0
``` kibana
PUT /_all/_settings
{
  "index.number_of_replicas": 0
}
```
