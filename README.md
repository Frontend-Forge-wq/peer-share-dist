# PeerShare - 基于 WebRTC 的高性能 P2P 文件传输应用

## 项目概述

PeerShare 是一个功能完善的浏览器端 P2P 文件传输应用，通过 WebRTC DataChannel 实现点对点传输，服务器仅承担信令转发，不参与文件中转。该项目确保数据传输的隐私性、安全性和高效性，特别适合需要快速分享大型文件的场景。

## 核心功能

- 📁 **P2P 单文件传输** - 发送端分块、接收端自动组装，支持任意大小文件
- 📊 **实时传输状态** - 进度条、传输速度与剩余时间实时展示
- 🔄 **房间管理系统** - 支持输入/生成房间 ID、房间列表查看与快速加入
- 🌐 **信令稳定连接** - 断线自动重连机制，服务端实时广播用户状态变化
- 🔐 **可配置 ICE 服务器** - 支持自定义 STUN/TURN 服务器，优化网络穿透能力
- 💻 **响应式设计** - 完美适配桌面和移动设备
- 🔒 **端到端加密** - 利用 WebRTC 内置的 DTLS/SRTP 加密机制

## 技术栈

### 前端
- React + TypeScript
- Vite 构建工具
- WebRTC API
- WebSocket 客户端

### 后端
- Node.js + TypeScript
- Express + WebSocket 服务器
- STUN/TURN 服务器集成（支持 NAT 穿透）

## 目录结构

```
├── client/               # 前端应用
│   ├── src/              # 源代码
│   │   ├── App.tsx       # 单页面 UI 与传输逻辑
│   │   ├── components/   # UI 组件
│   │   ├── config.ts     # 配置文件
│   │   └── styles/       # 样式文件
│   ├── index.html        # 入口 HTML
│   ├── package.json      # 前端依赖
│   └── vite.config.ts    # Vite 配置
├── server/               # 信令服务器
│   ├── src/
│   │   └── index.ts      # 房间管理与 WebSocket 信令实现
│   ├── package.json      # 后端依赖
│   └── tsconfig.json     # TypeScript 配置
├── package.json          # 项目根配置
└── pnpm-workspace.yaml   # pnpm 工作区配置

## 快速开始

### 前置要求
- Node.js 16+
- pnpm (推荐)

### 安装步骤

1. 安装依赖（使用 pnpm workspace）
```bash
pnpm install
```

2. 启动开发环境（前后端并行）
```bash
pnpm dev
```

启动后可访问：
- 客户端：`http://localhost:5173/`
- 信令服务器：`http://localhost:3000/`

3. 服务器 API 端点
   - 健康检查：`/health`
   - 房间列表：`/rooms`
   - WebSocket 连接：`/ws`

4. 构建生产版本
```bash
# 构建客户端
pnpm --filter peershare-client build

# 构建服务器
pnpm --filter peershare-server build

# 预览前端静态构建
pnpm --filter peershare-client preview

# 运行服务器构建产物
pnpm --filter peershare-server start
```

## 环境变量

### 前端环境变量
- `VITE_SIGNAL_HTTP_BASE`：信令服务器 HTTP 地址
- `VITE_SIGNAL_WS_URL`：信令服务器 WebSocket 地址
- `VITE_ICE_SERVERS`：JSON 数组格式的 ICE 服务器配置（示例）

```json
[
  { "urls": "stun:stun.l.google.com:19302" },
  { "urls": ["turn:your.turn.server:3478"], "username": "user", "credential": "pass" }
]
```

如不设置，默认使用 Google STUN。

### 后端环境变量
- `PORT`：服务器端口
- `HOST`：服务器主机地址

## 部署指南

### 部署选项

#### 1. GitHub Pages + Cloudflare Workers
- **前端**: 部署到 GitHub Pages
- **后端**: 使用 Cloudflare Workers 作为 WebSocket 信令服务器
- **优点**: 完全免费无期限，适合个人使用

#### 2. Vercel + Render
- **前端**: 部署到 Vercel
- **后端**: 部署到 Render
- **优点**: 操作简单，免费计划慷慨，WebSocket 支持良好

#### 3. 自托管
- 可以部署到任何支持 Node.js 的服务器
- 需要配置 STUN/TURN 服务器以确保最佳连接性

### 使用建议

- 若存在对称 NAT 或网络受限场景，建议配置可用的 TURN 服务器以提高连接成功率。
- 大文件传输已加入 DataChannel 背压控制，避免缓冲区过载。仍建议在网络质量较佳时进行超大文件传输。
- 生产环境部署建议使用 HTTPS，以确保 WebRTC API 的完整功能支持。

## 待改进方向（规划）

- 多文件队列与并行控制
- 简易断点续传与失败重试
- 拖拽上传与更友好的 UI 反馈
- 文件传输密码保护
- 传输历史记录

## 安全考虑

- 所有 P2P 连接均使用 WebRTC 内置的 DTLS/SRTP 加密
- 信令服务器仅用于建立连接，不传输实际文件数据
- 建议使用 HTTPS 部署以支持所有浏览器功能
- 定期更新依赖以确保安全漏洞得到修复

## 贡献指南

1. Fork 项目
2. 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 开启 Pull Request

## 许可证

MIT License

## 联系方式

如有任何问题或建议，请通过 GitHub Issues 与我们联系。
