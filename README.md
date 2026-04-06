# QQ 农场多账号挂机 + Web 面板

基于 Node.js 的 QQ 农场自动化工具，支持多账号管理、Web 控制面板、实时日志与数据分析。

> 默认账号密码都是 `admin`，端口 `3007`，请部署登录后尽快修改密码！

## 技术栈

**后端**

[<img src="https://skillicons.dev/icons?i=nodejs" height="48" title="Node.js 20+" />](https://nodejs.org/)
[<img src="https://skillicons.dev/icons?i=express" height="48" title="Express 4" />](https://expressjs.com/)
[<img src="https://skillicons.dev/icons?i=socketio" height="48" title="Socket.io 4" />](https://socket.io/)

**前端**

[<img src="https://skillicons.dev/icons?i=vue" height="48" title="Vue 3" />](https://vuejs.org/)
[<img src="https://skillicons.dev/icons?i=vite" height="48" title="Vite 7" />](https://vitejs.dev/)
[<img src="https://skillicons.dev/icons?i=ts" height="48" title="TypeScript 5" />](https://www.typescriptlang.org/)
[<img src="https://cdn.simpleicons.org/pinia/FFD859" height="48" title="Pinia 3" />](https://pinia.vuejs.org/)
[<img src="https://skillicons.dev/icons?i=unocss" height="48" title="UnoCSS" />](https://unocss.dev/)

---

## 功能特性

### 功能截图
<img src="https://free.picui.cn/free/2026/03/27/69c638ef27e36.png" alt="图片失效"/>
<img src="https://free.picui.cn/free/2026/03/27/69c638eff412b.png" alt="图片失效"/>
<img src="https://free.picui.cn/free/2026/03/27/69c638f005734.png" alt="图片失效"/>
<img src="https://free.picui.cn/free/2026/03/27/69c638f02d18d.png" alt="图片失效"/>

### 多账号管理
- 账号新增、编辑、删除、启动、停止
- QQ 只能抓包 code，微信提供了可视化接口配置
- 账号被踢下线自动删除
- 账号连续离线超时自动删除
- 账号离线推送通知（支持 Bark、自定义 Webhook 等）

### 自动化能力
- 农场：收获、种植、浇水、除草、除虫、铲除、土地升级
- 仓库：收获后自动出售果实
- 好友：自动偷菜 / 帮忙 / 捣乱
- 任务：自动检查并领取
- 好友黑名单：跳过指定好友
- 静默时段：指定时间段内不执行好友操作

### Web 面板
- 概览 / 农场 / 背包 / 好友 / 分析 / 账号 / 设置页面
- 实时日志，支持按账号、模块、事件、级别、关键词、时间范围筛选
- 深色 / 浅色主题切换

### 分析页
支持按以下维度排序作物：
- 经验效率 / 普通肥经验效率
- 净利润效率 / 普通肥净利润效率
- 等级要求

---

## Docker 部署（推荐）

### 前置要求
- 已安装 [Docker](https://docs.docker.com/get-docker/) 及 Docker Compose

### 快速启动

```bash
# 1. 克隆仓库
git clone https://github.com/XyhTender/qq-farm-automation-bot.git
cd qq-farm-automation-bot

# 2. 构建并后台启动
docker compose up -d --build

# 3. 查看实时日志
docker compose logs -f

# 4. 停止并移除容器
docker compose down
```

浏览器访问 `http://你的IP:3007`

### 环境变量（可选）

在项目根目录创建或编辑 `.env` 文件：

```env
# 修改面板监听端口（默认 3007）
PORT=3007
# 时区（默认 Asia/Shanghai）
TZ=Asia/Shanghai
```

### 数据持久化

容器使用 Docker Volume 持久化数据，重建容器不会丢失账号和配置：

| Volume | 用途 |
|--------|------|
| `qq-farm-data` | 账号、配置等运行时数据 |
| `qq-farm-logs` | 运行日志 |

```bash
# 查看 Volume
docker volume ls | grep qq-farm

# 备份数据
docker run --rm -v qq-farm-data:/data -v $(pwd):/backup alpine tar czf /backup/qq-farm-backup.tar.gz /data
```

---

## 登录与安全

- 面板首次访问需要登录
- 默认管理账号：`admin / admin`
- **建议部署后立即修改为强密码**

---

## 项目结构

```
qq-farm-automation-bot/
├── core/                  # 后端（Node.js 机器人引擎）
│   ├── src/
│   │   ├── config/        # 配置管理
│   │   ├── controllers/   # HTTP API
│   │   ├── gameConfig/    # 游戏静态数据
│   │   ├── models/        # 数据模型与持久化
│   │   ├── proto/         # Protobuf 协议定义
│   │   ├── runtime/       # 运行时引擎与 Worker 管理
│   │   └── services/      # 业务逻辑（农场、好友、任务等）
│   ├── data/              # 运行时数据（accounts.json、store.json）
│   ├── Dockerfile
│   └── client.js          # 主进程入口
├── web/                   # 前端（Vue 3 + Vite）
│   ├── src/
│   │   ├── api/           # API 客户端
│   │   ├── components/    # Vue 组件
│   │   ├── stores/        # Pinia 状态管理
│   │   └── views/         # 页面视图
│   └── dist/              # 构建产物
├── docker-compose.yml
├── pnpm-workspace.yaml
└── package.json
```

---

## 特别感谢
- 基于 [Penty-d/qq-farm-bot-ui](https://github.com/Penty-d/qq-farm-bot-ui) 二改
- 核心功能：[linguo2625469/qq-farm-bot](https://github.com/linguo2625469/qq-farm-bot)
- 部分功能：[QianChenJun/qq-farm-bot](https://github.com/QianChenJun/qq-farm-bot)
- 扫码登录：[lkeme/QRLib](https://github.com/lkeme/QRLib)
- 推送通知：[imaegoo/pushoo](https://github.com/imaegoo/pushoo)

## 免责声明

本项目仅供学习与研究用途。使用本工具可能违反游戏服务条款，由此产生的一切后果由使用者自行承担。
