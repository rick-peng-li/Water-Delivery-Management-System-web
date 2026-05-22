# WRS-DMS: 水站配送管理系统 (Water Refilling Station Delivery Management System)

WRS-DMS 是一个基于 MERN 技术栈的全栈应用，专为水站配送管理设计。它集成了库存管理、线下门店销售 (POS)、配送调度、实时 GPS 追踪以及财务报表等功能。

## 项目需求介绍

本项目旨在解决单体水站日常运营中的核心需求：

- **库存管理**: 实时追踪不同规格水桶（如 Slim, Round, Gallon）的库存情况。
- **线下门店销售 (Walk-in POS)**: 支持店内直接销售，快速处理现场客户订单。
- **配送订单管理**: 创建、分配和追踪配送订单。
- **实时 GPS 追踪**: 管理员和员工可以实时查看司机的地理位置（基于 Socket.IO）。
- **司机工作流**: 司机拥有专用的 PWA 视图，查看当日配送任务、导航以及上传送达证明（照片）。
- **财务与支出管理**: 记录燃油费用、行程支出，并生成收入和支出报表。
- **空桶管理 (Jug Accountability)**: 精确追踪发出和收回的空桶数量，确保资产不流失。

## 架构

项目采用前后端分离架构：

### 技术栈

- **前端**: React.js + Vite + Tailwind CSS + shadcn/ui
- **后端**: Node.js + Express.js
- **数据库**: MongoDB + Mongoose
- **实时通信**: Socket.IO (用于 GPS 实时更新)
- **地图服务**: Leaflet + OpenStreetMap
- **身份验证**: JWT + bcrypt
- **文件存储**: Cloudinary (用于存储送达证明和收据照片)

### 目录结构

```text
├── client/                # React 前端应用
│   ├── src/api/          # API 请求封装
│   ├── src/components/   # 公共 UI 组件
│   ├── src/context/      # 状态管理 (Auth, Socket)
│   ├── src/hooks/        # 自定义 Hooks (地理位置追踪等)
│   ├── src/pages/        # 各角色功能页面 (Admin, Staff, Driver)
│   └── src/services/     # 业务逻辑服务层
├── server/                # Node.js 后端应用
│   ├── config/           # 数据库及第三方服务配置
│   ├── controllers/      # 控制器层 (处理业务逻辑)
│   ├── middleware/       # 中间件 (身份验证、角色控制、错误处理)
│   ├── models/           # Mongoose 数据模型
│   ├── routes/           # API 路由
│   └── socket/           # Socket.IO 实时通信处理
```

## 启动方式

### 环境准备

1. 确保安装了 [Node.js](https://nodejs.org/) 和 [MongoDB](https://www.mongodb.com/)。
2. 配置环境变量：
   - 在 `server/` 目录下创建 `.env` 文件，配置以下变量：
     ```env
     PORT=5000
     MONGO_URI=mongodb://localhost:27017/wrs-dms
     JWT_SECRET=your_secret_key
     CLOUDINARY_CLOUD_NAME=your_cloud_name
     CLOUDINARY_API_KEY=your_api_key
     CLOUDINARY_API_SECRET=your_api_secret
     ```
   - 在 `client/` 目录下创建 `.env` 文件，配置以下变量：
     ```env
     VITE_API_URL=http://localhost:5000
     VITE_SOCKET_URL=http://localhost:5000
     ```

### 安装与运行

1. **安装所有依赖**:
   在项目根目录下运行：
   ```bash
   npm install
   cd client && npm install
   cd ../server && npm install
   ```

2. **启动开发服务器**:
   在项目根目录下运行：
   ```bash
   npm run dev
   ```
   该命令会使用 `concurrently` 同时启动后端服务器（默认端口 5000）和前端开发服务器（默认端口 5173）。

3. **数据初始化 (可选)**:
   如果需要预填测试数据，可以在 `server` 目录下运行：
   ```bash
   npm run seed
   ```

## 用户角色

- **管理员 (Admin)**: 拥有所有权限，包括用户管理、系统设置和完整报表查看。
- **员工 (Staff)**: 负责 POS 销售、订单调度、司机指派和库存管理。
- **司机 (Driver)**: 查看分配给自己的任务，上传送达照片，上报燃油支出。
