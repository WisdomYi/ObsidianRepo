---
aliases:
  - AIDevGallery
tags:
  - 性能优化
  - GPU管理
  - GPU工具
核心功能:
  - 动态设备切换
  - 模型仓库集成
  - 实时监控面板
creation: 2025-02-28
---

# AIDevGallery工具指南

## 🎯 核心特性
1. **动态设备切换**
   - 通过`DeviceSelector.AutoSwitch()`方法实现运行时自动选择最佳计算设备
   - 支持优先级配置：`GPU > DirectML > CPU`

2. **模型仓库集成**
   ```csharp
   var model = await ModelHub.DownloadAsync("bert-base-uncased");
   model.ExecutionProvider = ExecutionProvider.Cuda;
     ```

3. **实时监控面板**
   ```json
   // 监控数据输出示例
   {
     "gpu_usage": 72%, 
     "vram_used": "4.3/8GB",
     "inference_time": "43ms"
   }
   ```

## 🚀 快速开始
1. 安装NuGet包：
   ```bash
   dotnet add package Microsoft.AI.DevGallery
   ```

2. 设备检测代码：
   ```csharp
   var devices = await DeviceScanner.GetAvailableDevices();
   if(devices.GpuCount > 0) {
       config.EnableGpuAcceleration();
   }
   ```

## ⚠️ 已知限制
- 暂不支持多GPU负载均衡
- Windows系统需安装WSL2才能使用DirectML后端
```

### 知识库使用建议：
1. 安装Obsidian插件：
   - **Dataview**：自动生成工具状态仪表盘
   - **Excalidraw**：绘制GPU集群架构图
   - **Tasks**：跟踪GPU任务调度问题

2. 建立索引关系：
   ```markdown
   [[ML.NET]] 与 [[GPUStack平台]] 都支持 --> [[动态显存分配策略]]
   ```

3. 添加监控模板：
   ```markdown
   ## 🕹️ 今日GPU状态
   ```query
   path:"Monitoring" 
   date(today)
   ```
   

