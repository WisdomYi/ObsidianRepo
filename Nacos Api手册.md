## API规则
统一返回
{
  "code": 0,
  "message": "success",
  "data": {}
}

错误码

| 错误码     | 提示信息                         | 含义                 |
| ------- | ---------------------------- | ------------------ |
| `0`     | `success`                    | 成功执行               |
| `10000` | `parameter missing`          | 参数缺失               |
| `10001` | `access denied`              | 访问拒绝               |
| `10002` | `data access error`          | 数据访问错误             |
| `20001` | `'tenant' parameter error`   | `tenant`参数错误       |
| `20002` | `parameter validate error`   | 参数验证错误             |
| `20003` | `MediaType Error`            | 请求的`MediaType`错误   |
| `20004` | `resource not found`         | 资源未找到              |
| `20005` | `resource conflict`          | 资源访问冲突             |
| `20006` | `config listener is null`    | 监听配置为空             |
| `20007` | `config listener error`      | 监听配置错误             |
| `20008` | `invalid dataId`             | 无效的`dataId`（鉴权失败）  |
| `20009` | `parameter mismatch`         | 请求参数不匹配            |
| `21000` | `service name error`         | `serviceName`服务名错误 |
| `21001` | `weight error`               | `weight`权重参数错误     |
| `21002` | `instance metadata error`    | 实例`metadata`元数据错误  |
| `21003` | `instance not found`         | `instance`实例不存在    |
| `21004` | `instance error`             | `instance`实例信息错误   |
| `21005` | `service metadata error`     | 服务`metadata`元数据错误  |
| `21006` | `selector error`             | 访问策略`selector`错误   |
| `21007` | `service already exist`      | 服务已存在              |
| `21008` | `service not exist`          | 服务不存在              |
| `21009` | `service delete failure`     | 存在服务实例，服务删除失败      |
| `21010` | `healthy param miss`         | `healthy`参数缺失      |
| `21011` | `health check still running` | 健康检查仍在运行           |
| `22000` | `illegal namespace`          | 命名空间`namespace`不合法 |
| `22001` | `namespace not exist`        | 命名空间不存在            |
| `22002` | `namespace already exist`    | 命名空间已存在            |
| `23000` | `illegal state`              | 状态`state`不合法       |
| `23001` | `node info error`            | 节点信息错误             |
| `23002` | `node down failure`          | 节点离线操作出错           |
| …       | …                            | …                  |
| 30000   | `server error`               | 其他内部错误             |

## 获取配置

	GET /nacos/v2/cs/config

| 参数名           | 类型       | 必填    | 参数描述                     |
| ------------- | -------- | ----- | ------------------------ |
| `namespaceId` | `String` | 否     | 命名空间，默认为`public`与 `''`相同 |
| `group`       | `String` | **是** | 配置分组名                    |
| `dataId`      | `String` | **是** | 配置名                      |
| `tag`         | `String` | 否     | 标签                       |

---
## 发布配置

	POST  /nacos/v2/cs/config
	Content-Type:application/x-www-form-urlencoded

| 参数名           | 类型       | 必填    | 参数描述                     |
| ------------- | -------- | ----- | ------------------------ |
| `namespaceId` | `String` | 否     | 命名空间，默认为`public`与 `''`相同 |
| `group`       | `String` | **是** | 配置组名                     |
| `dataId`      | `String` | **是** | 配置名                      |
| `content`     | `String` | **是** | 配置内容                     |
| `tag`         | `String` | 否     | 标签                       |
| `appName`     | `String` | 否     | 应用名                      |
| `srcUser`     | `String` | 否     | 源用户                      |
| `configTags`  | `String` | 否     | 配置标签列表，可多个，逗号分隔          |
| `desc`        | `String` | 否     | 配置描述                     |
| `use`         | `String` | 否     | -                        |
| `effect`      | `String` | 否     | -                        |
| `type`        | `String` | 否     | 配置类型                     |
| `schema`      | `String` | 否     | -                        |
