IZeebeClient _zeebeClient [[Zeebe.Client.IZeebeClient]]

/// 获取集群结构
Task<ITopology> GetTopologyAsync();

/// 部署流程
Task<IDeployResourceResponse> DeployProcessAsync(string path);

/// 部署流程
Task<IDeployResourceResponse> DeployProcessAsync(Stream stream, string name);

/// 创建流程实例
Task<IProcessInstanceResponse> CreateProcessInstanceAsync(string bpmnProcessId);

/// 创建流程实例
Task<IProcessInstanceResponse> CreateProcessInstanceAsync(string bpmnProcessId, int version);

/// 创建流程实例
Task<IProcessInstanceResponse> CreateProcessInstanceAsync(string bpmnProcessId, string variables);

/// 创建流程实例
Task<IProcessInstanceResponse> CreateProcessInstanceAsync(string bpmnProcessId, int version, string variables);

/// 取消流程实例
Task<ICancelProcessInstanceResponse> CancelProcessInstanceAsync(long processInstanceKey);

/// 发布消息
Task<IPublishMessageResponse> PublishMessageAsync(string messageName, string correlationKey);

/// 发布消息
Task<IPublishMessageResponse> PublishMessageAsync(string messageName, string correlationKey, string variables);

/// 发布消息
Task<IPublishMessageResponse> PublishMessageAsync(string messageName, string correlationKey, string messageId, TimeSpan timeToLive, string variables);

/// 设置变量
Task<ISetVariablesResponse> SetVariablesAsync(long elementInstanceKey, string variables, bool isLocal = true);

/// 新工作者
IJobWorker NewWorker(string jobType, AsyncJobHandler handler, string workerName, int maxJobsActive = 1, byte threadCount = 1, double pollingTimeout = 60 * 2, double timeout = 60 * 2);

