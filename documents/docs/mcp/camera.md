# 摄像头工具 (Camera Tools)

摄像头工具是一个智能视觉识别 MCP 工具，提供了图像捕获、视觉分析和图像理解等功能。

### 常见使用场景

**图像识别分析:**
- "帮我拍张照片看看这是什么"
- "拍照识别一下这个物体"
- "用摄像头看看我面前是什么东西"
- "看看这个东西是什么"
- "识别一下这个是什么"
- "帮我看看这个"
- "拍照分析这个物品"

**场景理解:**
- "拍照描述一下现在的场景"
- "用摄像头看看房间里有什么"
- "拍照分析一下这个环境"
- "看看周围的情况"
- "描述一下这个场景"
- "分析一下这里的环境"

**文字识别:**
- "拍照识别这个文档上的文字"
- "用摄像头读取这个标签上的信息"
- "拍照翻译这段英文"
- "读取这个文字"
- "识别文字内容"
- "帮我读一下这个"
- "翻译这段文字"
- "提取文字信息"

**问题解答:**
- "拍照帮我看看这道题怎么做"
- "用摄像头分析这个图表"
- "拍照解释这个标志是什么意思"
- "这道题怎么解"
- "分析这个图表"
- "解释这个标志"
- "帮我解答这个问题"

**生活助手:**
- "拍照识别这个植物的品种"
- "用摄像头看看这个菜谱"
- "拍照帮我识别这个商品"
- "这是什么植物"
- "识别这个花"
- "看看这个菜谱"
- "这个商品是什么"
- "帮我看看这个产品"

### 使用提示

1. **确保光线充足**: 良好的光线条件有助于提高识别准确率
2. **保持稳定**: 拍照时尽量保持设备稳定，避免模糊
3. **明确问题**: 详细描述您想了解的内容，如"识别这个植物"而不是"这是什么"
4. **合适距离**: 保持适当的拍摄距离，确保目标物体清晰可见

AI 助手会根据您的需求自动调用摄像头工具，捕获图像并进行智能分析。

## 功能概览

### 图像捕获功能
- **智能拍照**: 自动调节摄像头参数，捕获清晰图像
- **尺寸优化**: 自动调整图像尺寸，提高处理效率
- **格式转换**: 将图像转换为标准JPEG格式

### 视觉分析功能
- **物体识别**: 识别图像中的物体和场景
- **文字识别**: 提取图像中的文字内容
- **场景理解**: 分析图像内容并提供描述
- **问题解答**: 基于图像内容回答用户问题

### 设备管理功能
- **摄像头配置**: 自动检测和配置摄像头设备
- **参数调节**: 支持分辨率、帧率等参数设置
- **错误处理**: 完善的错误处理和恢复机制

## 工具列表

### 1. 图像捕获与分析工具

#### take_photo - 拍照并分析
捕获图像并进行智能分析。

**参数:**
- `question` (可选): 对图像的具体问题或分析需求

**使用场景:**
- 物体识别
- 场景分析
- 文字识别
- 问题解答
- 生活助手

## 使用示例

### 基础拍照分析示例

```python
# 简单拍照分析
result = await mcp_server.call_tool("take_photo", {
    "question": "这是什么物体？"
})

# 场景描述
result = await mcp_server.call_tool("take_photo", {
    "question": "描述一下这个场景"
})

# 文字识别
result = await mcp_server.call_tool("take_photo", {
    "question": "识别图片中的文字内容"
})

# 问题解答
result = await mcp_server.call_tool("take_photo", {
    "question": "这道数学题怎么解？"
})
```

## 技术架构

### 摄像头管理
- **单例模式**: 确保全局只有一个摄像头实例
- **线程安全**: 支持多线程环境下的安全访问
- **资源管理**: 自动管理摄像头资源的开启和释放

### 图像处理
- **OpenCV集成**: 使用OpenCV进行图像捕获和处理
- **智能缩放**: 自动调整图像尺寸，保持最佳效果
- **格式优化**: 转换为JPEG格式，减少传输负载

### 视觉服务
- **远程分析**: 支持连接远程视觉分析服务
- **身份验证**: 支持Token和设备ID验证
- **错误处理**: 完善的网络错误处理机制

## 配置说明

### 摄像头配置
摄像头相关配置位于配置文件中：

```json
{
  "CAMERA": {
    "camera_index": 0,
    "frame_width": 640,
    "frame_height": 480
  }
}
```

**配置项说明:**
- `camera_index`: 摄像头设备索引，默认0
- `frame_width`: 图像宽度，默认640
- `frame_height`: 图像高度，默认480

### 视觉服务配置
视觉分析服务需要配置：
- **服务URL**: 视觉分析服务的接口地址
- **身份验证**: Token或API密钥
- **设备信息**: 设备ID和客户端ID

## 数据结构

### 图像数据格式
```python
{
    "buf": bytes,      # JPEG图像字节数据
    "len": int         # 数据长度
}
```

### 分析结果格式
```python
{
    "success": bool,           # 是否成功
    "message": str,            # 结果信息或错误信息
    "analysis": {              # 分析结果（成功时）
        "objects": [...],      # 识别的物体
        "text": str,           # 提取的文字
        "description": str,    # 场景描述
        "answer": str          # 问题答案
    }
}
```

## 图像处理流程

### 1. 图像捕获
1. 初始化摄像头设备
2. 设置捕获参数（分辨率、帧率等）
3. 捕获单帧图像
4. 释放摄像头资源

### 2. 图像预处理
1. 获取图像尺寸信息
2. 计算缩放比例（最长边不超过320像素）
3. 等比例缩放图像
4. 转换为JPEG格式

### 3. 视觉分析
1. 准备请求头信息
2. 构建多媒体请求
3. 发送到视觉分析服务
4. 解析分析结果

## 最佳实践

### 1. 图像质量优化
- 确保充足的光线条件
- 保持摄像头清洁
- 避免过度曝光或阴暗
- 保持拍摄对象清晰

### 2. 问题描述技巧
- 使用具体明确的问题
- 避免模糊不清的表述
- 提供上下文信息
- 指明分析重点

### 3. 性能优化
- 合理设置图像分辨率
- 避免频繁拍照
- 及时释放资源
- 处理网络超时

### 4. 错误处理
- 检查摄像头可用性
- 处理网络连接错误
- 验证分析结果
- 提供用户友好的错误信息

## 支持的分析类型

### 物体识别
- 日常用品识别
- 动植物识别
- 食物识别
- 商品识别

### 文字识别
- 印刷文字识别
- 手写文字识别
- 多语言文字识别
- 文档内容提取

### 场景理解
- 室内场景分析
- 户外环境描述
- 人物动作识别
- 活动场景理解

### 问题解答
- 数学题解答
- 图表分析
- 标志解释
- 技术问题

## 注意事项

1. **隐私保护**: 拍照功能涉及隐私，请谨慎使用
2. **网络依赖**: 视觉分析需要网络连接
3. **设备权限**: 需要摄像头访问权限
4. **处理时间**: 图像分析可能需要一定时间

## 故障排除

### 常见问题
1. **摄像头无法打开**: 检查设备连接和权限
2. **图像模糊**: 检查光线条件和对焦
3. **分析失败**: 检查网络连接和服务状态
4. **结果不准确**: 优化图像质量和问题描述

### 调试方法
1. 检查摄像头设备状态
2. 验证网络连接
3. 查看日志错误信息
4. 测试不同的拍摄条件

通过摄像头工具，您可以轻松实现智能视觉识别和图像分析，为日常生活和工作提供便利。
