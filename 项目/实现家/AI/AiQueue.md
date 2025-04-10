AiQueueTaskType TaskType 任务类型
Guid TaskId 任务Id
AiQueueStatus Status 状态
double Priority 任务优先级(数值越高，优先级越高)如果为0不处理优先级
string? Variable 变量，填充在排队结束之后的操作需要使用的json字符串
string? Result 返回结果
long? ExpectQueueTimeSpan 预计排队时间
DateTime EnterQueueTime 进入队列时间
DateTime? PushZeebeTime 推送zeebe时间
DateTime? ExecuteTime 执行时间，慢速队列执行时间，快速队列此处为null

> [!AiQueueStatus]
> - 0排队中
> - 1已推送
> - 2已取消