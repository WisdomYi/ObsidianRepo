Guid WorkerServerId  工作者服务器id[[WorkerServer]]
string Name 名称
WorkerType Type 类型
DateTime? StartTime 启动时间-记录jobworker最后一次启动时间
DateTime? LastActiveTime 最后活动时间-记录最后一次执行任务的结束时间
WorkerStartupState StartupState 启动状态
WorkerLoadState LoadState 负载状态

> [!WorkerType]
> - 1生图
> - 2处理图片
> - 3打标
> - 4训练

> [!WorkerStartupState]
> - 0已停止
> - 1已启动


> [!WorkerLoadState]
> - 0空闲
> - 1运行中
