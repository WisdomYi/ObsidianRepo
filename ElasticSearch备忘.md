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

### 校验
 curl -X GET "<Elasticsearch 服务地址>:9200/_cluster/health?pretty"
### 部署
1.拉取版本镜像（8.17.0为例）
2.挂载elasticsearch.yml文件。目录为 `/usr/share/elasticsearch/config/elasticsearch.yml`
3.配置yml。
``` elasticsearch.yml
cluster.name: my-application
node.name: test_node1
network.host: 0.0.0.0
http.cors.enabled: true
http.cors.allow-origin: "*"
```
4.设置环境变量：
```
discovery.type=single-node
ES_JAVA_OPTS=-Xms2g -Xmx2g
```

### 配置项

node 节点
	name  名称
	master (true) 是否有资格为主节点
	data（true）是否存储索引数据
discovery 发现
	type 发现机制类型，单节点中设置为`single-node`
	seed_hosts指定其他节点主机名或ip
cluster 集群
	name  名称
	initial_cluster_manager_nodes 指定集群管理节点
network
	host 绑定的网络地址，0.0.0.0绑定所有可用网络接口
http
	port 指定端口号，默认9200
	cors
		enabled 启用跨域资源共享，默认false
		allow-origin 设置跨域请求来源
action
	auto_create_index 是否允许自动创建索引
indices
	max_result_window 索引中能检索的结果的最大偏移量 默认10000
thread_pool
	bulk
		queue_size 文档写入队列 默认200
	search
		queue_size 文档搜索队列大小 默认1000
xpack
	security
		enabled  启用或禁用 X-Pack 安全功能，默认是  false 
		enrollment
			enabled  自动进行安全注册，即节点之间会自动交换证书等安全信息 默认 false
		transport
			ssl
				enabled  在节点间传输层通信时启用 SSL 加密，默认是  false 。
		http
			ssl
				enabled  在 HTTP 通信时启用 SSL 加密，默认是  false。
	watcher
		enabled  
## kibana部署
1.拉取镜像
2.挂载kibana.yml 默认``