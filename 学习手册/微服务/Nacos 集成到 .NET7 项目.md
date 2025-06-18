
## 前言

在微服务架构中，服务的注册与发现、配置管理是关键环节。Nacos 是一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。本文将介绍如何将 Nacos 集成到 .NET7 项目中，实现服务注册与发现、配置管理等功能。

## Nacos 环境准备

### 安装 Nacos

从 Nacos 官方 GitHub 地址 [https://github.com/alibaba/nacos](https://github.com/alibaba/nacos) 或者 [https://github.com/alibaba/nacos/releases](https://github.com/alibaba/nacos/releases) 下载编译好的服务端安装包或者源代码，解压安装包，在 bin 启动脚本文件中通过 cmd 或者 sh 脚本命令运行服务端、将服务注册成 windows 系统服务或者使用 nssm 注册成系统服务；config 配置文件中配置端口和持久化服务数据库连接字符。如果运行成功则在浏览器访问 [http://127.0.0.1:8848/nacos/index.html](http://127.0.0.1:8848/nacos/index.html)，默认账户密码 nacos。

## .NET7 项目集成 Nacos

### 创建项目

创建一个 WebApi 项目，作为示例项目。

### 添加 Nacos 依赖

在项目中添加 Nacos 相关的 NuGet 包引用：

```xml
<PackageReference Include="nacos-sdk-csharp" Version="1.3.4" />
<PackageReference Include="nacos-sdk-csharp.AspNetCore" Version="1.3.4" />
<PackageReference Include="nacos-sdk-csharp.Extensions.Configuration" Version="1.3.4" />
<PackageReference Include="nacos-sdk-csharp.IniParser" Version="1.3.4" />
<PackageReference Include="nacos-sdk-csharp.YamlParser" Version="1.3.4" />
```

### 修改配置文件

在 appsettings.json 文件中添加 Nacos 相关配置：

```json
"NacosConfig": {
  "Listeners": [
    {
      "Optional": false,
      "DataId": "netTestConfig",
      "Group": "zltest"
    }
  ],
  "Namespace": "bf644fca-1276-415a-89de-428331e96a46",
  "ServerAddresses": [ "172.16.9.88:8848" ],
  "ServiceName": "netTest",
  "GroupName": "zltest"
}
```

### 在 Nacos 上添加配置文件

在 Nacos 控制台中，添加对应的配置文件，配置内容与 appsettings.json 中的 NacosConfig 配置一致。

### 编写代码

在 Program.cs 文件中，加载 Nacos 配置并注册服务：

```csharp
// 注册服务
builder.Services.AddNacosAspNet(builder.Configuration, section: "NacosConfig");
// 设置 Nacos 配置
builder.Host.UseNacosConfig(section: "NacosConfig", parser: null, logAction: null);
```

## 示例：获取 Nacos 配置

编写一个接口，用于显示从 Nacos 获取的配置：

```csharp
[ApiController]
[Route("[controller]")]
public class ConfigController : ControllerBase
{
    private readonly IConfiguration _configuration;

    public ConfigController(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    [HttpGet]
    public IActionResult GetConfig()
    {
        var dbConfig = _configuration.GetSection("DbConfig").Get<DbConfig>();
        return Ok(dbConfig);
    }
}

public class DbConfig
{
    public int DbType { get; set; }
    public string ConnectionString { get; set; }
    public bool IsAutoCloseConnection { get; set; }
}
```

## 注意事项

1. 在使用 Nacos 的过程中，要注意版本兼容性问题，确保所使用的 Nacos SDK 版本与 .NET7 兼容。
2. 对于生产环境，建议对 Nacos 进行集群部署，以提高高可用性和性能。
3. 在配置 Nacos 时，要注意安全问题，如设置访问权限、使用 HTTPS 等。

## 总结

通过以上步骤，我们成功地将 Nacos 集成到了 .NET7 项目中，实现了服务注册与发现、配置管理等功能。这为构建微服务架构的应用程序提供了坚实的基础。在实际项目中，可以根据具体需求进一步扩展和优化 Nacos 的使用方式。

## 参考链接

- Nacos 官方文档：[https://nacos.io/zh-cn/docs/quick-start.html](https://nacos.io/zh-cn/docs/quick-start.html)
- .NET 使用 Nacos 配置教程：[https://www.cnblogs.com/raokun/p/17348508.html](https://www.cnblogs.com/raokun/p/17348508.html)