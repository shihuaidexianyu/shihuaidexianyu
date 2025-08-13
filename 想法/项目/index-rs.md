### 项目目的

`index-rs` 是一个现代化的服务器监控和多服务导航仪表盘。其核心目标是为服务器管理员提供一个清晰、实时的界面，用于监控系统核心指标，并能方便地访问部署在服务器上的各种服务。

主要功能包括：

- **实时系统监控**：全面监控CPU、内存、磁盘、网络、GPU等关键性能指标。
    
- **服务健康检查与导航**：通过配置文件定义服务器上的各项服务，仪表盘会自动检查它们的在线状态，并以卡片形式提供快速访问入口。
    
- **端口和进程监控**：查看系统中所有端口的使用情况以及正在运行的进程列表。
    
- **Docker容器管理**：监控服务器上运行的Docker容器，并能执行启动、停止、重启、暂停和查看日志等基本操作。
    
- **文件管理**：提供一个网页界面，用于浏览、上传、下载、创建和删除服务器指定目录下的文件。
    

### 技术栈

该项目采用前后端分离的架构，具体技术栈如下：

**后端 (Backend)**:

- **语言**: Rust。
    
- **Web 框架**: Axum，一个基于 `Tokio` 的高性能Web框架。
    
- **异步运行时**: Tokio，用于处理所有异步任务，如数据采集和网络通信。
    
- **系统信息采集**: `sysinfo` crate，用于获取CPU、内存、磁盘和进程等核心系统信息。
    
- **配置管理**: 使用 TOML 格式的配置文件 (`config.toml`)。
    
- **序列化/反序列化**: `Serde` 和 `serde_json`。
    
- **日志**: `tracing` 和 `tracing-subscriber`。
    

**前端 (Frontend)**:

- **UI 框架**: React。
    
- **构建工具**: Vite，提供极速的开发体验和优化的构建输出。
    
- **状态管理**: Zustand，一个轻量、灵活的全局状态管理库。
    
- **数据可视化**: `ECharts` (通过 `echarts-for-react`)，用于绘制各种实时性能图表。
    
- **样式**: Tailwind CSS，一个功能优先的原子化CSS框架。
    
- **HTTP 请求**: `Axios`。
    

### 设计文档与架构

项目的核心设计是围绕通过 WebSocket 实现的实时数据流，将后端采集的系统指标以广播形式推送给所有连接的前端客户端。

**后端架构**:

1. **数据采集器 (`src/collectors.rs`)**
    
    - 这是一个在后台异步运行的任务，使用 `Tokio` 定期（默认为每秒）采集系统数据。
        
    - 它通过 `sysinfo` 库获取 CPU、内存、磁盘和网络的基础信息。
        
    - 对于更专门的信息，它会调用外部命令行工具，例如：
        
        - 使用 `nvidia-smi` 获取 NVIDIA GPU 的详细指标（如使用率、温度、功耗）。
            
        - 使用 `sensors` 命令获取 CPU 温度和功耗。
            
        - 优先使用 `ss` 命令，如果失败则降级使用 `netstat` 来获取端口占用信息。
            
        - 使用 `docker ps` 和 `docker stats` 获取 Docker 容器信息。
            
    - 为了减少性能开销，对执行外部命令获取的数据（如GPU和CPU传感器信息）设计了缓存机制。
        
2. **Web 服务器 (`src/main.rs`, `src/handlers.rs`)**
    
    - 基于 Axum 框架，默认监听 `9876` 端口。
        
    - **REST API**: 提供多个HTTP端点，用于获取非实时数据：
        
        - `GET /api/system/static`: 获取系统静态信息（如操作系统、主机名、CPU型号等）。
            
        - `GET /api/services`: 根据配置文件获取服务列表及其健康状态。
            
        - `POST /api/docker/action` 和 `GET /api/docker/logs/:id`: 用于管理Docker容器。
            
        - 文件管理相关的API（列表、上传、下载等）。
            
    - **WebSocket 端点 (`/ws/realtime`)**: 这是实现实时监控的核心。当客户端连接时，服务器会订阅一个广播通道。数据采集器将采集到的最新数据发送到这个通道，然后由通道广播给所有连接的客户端。
        
3. **配置模块 (`src/config.rs`)**
    
    - 负责加载 `config.toml` 文件。如果文件不存在或解析失败，则会使用一套默认配置。
        
    - 服务导航卡片的内容完全由该配置文件驱动。
        

**前端架构**:

1. **状态管理 (`frontend/src/store/serverStore.js`)**
    
    - 使用 Zustand 创建一个全局的 `store`，用于存放从后端接收的所有数据。
        
    - 状态分为几大类：连接状态 (`isConnected`)、系统静态信息 (`systemInfo`)、服务列表 (`services`)、实时数据 (`realtimeData`) 和用于图表的历史数据 (`history`)。
        
    - 当新的实时数据通过 WebSocket 到达时，会更新 `realtimeData`，同时将关键指标（如CPU使用率）推入 `history` 数组，以供图表渲染。历史数据有固定长度限制（如60个点），防止内存无限增长。
        
2. **WebSocket 管理 (`frontend/src/api/websocket.js`)**
    
    - 这是一个单例管理器，负责建立和维护与后端 `/ws/realtime` 的连接。
        
    - 它实现了自动重连机制，当连接意外断开时，会采用指数退避策略尝试重新连接，直到达到最大尝试次数。
        
    - 提供事件监听接口 (`onMessage`, `onConnectionChange`)，让React组件可以订阅数据更新和连接状态变化。
        
3. **组件化UI (`frontend/src/components/`)**
    
    - 采用模块化、基于小部件（Widget）的架构。
        
    - 每个关键的监控项（如CPU、内存、网络）都有一个独立的React组件。
        
    - 这些组件从 Zustand `store` 中获取所需的数据，并在数据更新时自动重新渲染，从而实现界面的实时刷新。
        
    - 使用 `echarts-for-react` 将历史数据渲染成动态图表。
        

### 具体实现机制

- **实时数据流**: 后端 `SystemCollector` 在一个独立的 Tokio 任务中循环运行，每秒采集一次数据，然后通过 `broadcast::Sender` 发送 `RealtimeData` 结构体。在 `handlers.rs` 中，每个 WebSocket 连接都会创建一个 `broadcast::Receiver` 来接收这些数据，并将其序列化为 JSON 字符串发送给前端。前端的 `WebSocketManager` 接收到消息后，反序列化并通知 `serverStore` 更新状态，最终触发UI组件的重新渲染。
    
- **服务健康检查**: 当前端请求 `/api/services` 时，后端会遍历配置文件中的服务列表。对于每个服务，它会异步地向其健康检查URL（或服务根URL）发送一个HTTP GET请求。根据响应状态码（2xx或3xx视为在线）来确定服务的 `online` 或 `offline` 状态，并将最终列表返回给前端。
    
- **Docker 容器管理**: Docker容器的信息采集与其他系统指标类似，通过执行 `docker` 命令行工具完成。而容器的操作（启动、停止等）则是通过专门的REST API端点 (`/api/docker/action`) 实现的。前端点击操作按钮后，会发送一个POST请求到该端点，后端接收到请求后，再通过 `tokio::process::Command` 执行相应的 `docker` 命令来操作容器。
    
- **文件管理**: 后端提供了一整套符合REST风格的API来操作服务器上的文件系统（在一个预设的安全目录下）。前端的 `FileManager` 组件通过调用这些API来实现文件的展示、上传（使用 `FormData`）、下载（通过直接打开URL）和删除等功能。UI上提供了列表和网格两种视图模式，并支持拖拽上传。