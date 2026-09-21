# AI Agent Skills 本地工作空间 (`~/Documents/ai`)

本目录集中管理由 Antigravity / Gemini AI 驱动的所有独立开源技能仓库与中央索引库，方便日常本地查看、手动编辑、演进与 Git 提交。

## ⚡ 理论基石与架构优化手册
- 📘 **[Agent Optima (`../agent-optima`)](https://github.com/Garfield247/agent-optima)** — AI Agent 架构优化与多生态生产工程手册 (长会话抗腐化 / 上下文防混乱 / Token 极致能效)

## 📂 仓库全景目录

### 1. 技能元治理与索引中心
- [`agent-skills`](./agent-skills) - **中央技能索引库与 Hub** (全量技能注册表 `registry.json` 与分类导航)
- [`agent-skill-manager`](./agent-skill-manager) - 技能规范全生命周期治理与脚手架

### 2. 软件研发生命周期 (Lifecycle Superpowers)
- [`agent-skill-codebase-reconnaissance`](./agent-skill-codebase-reconnaissance) - 宏观摸骨五步法与代码逆向检索
- [`agent-skill-technical-design`](./agent-skill-technical-design) - 方案先行、RFC 设计与状态机建模
- [`agent-skill-systematic-debugging`](./agent-skill-systematic-debugging) - 根因排障 SOP (RCA) 与红绿灯测试门禁
- [`agent-skill-git-workflow`](./agent-skill-git-workflow) - Conventional Commits 中文原子提交与变基治理
- [`agent-skill-project-documentation`](./agent-skill-project-documentation) - Diátaxis 架构、MADR 与项目文档体系
- [`agent-skill-tmux-cockpit`](./agent-skill-tmux-cockpit) - Tmux 赛博战斗舱、三联屏拓扑协同与外置观测 CLI

### 3. 语言与微服务框架栈 (Language & Framework Stacks)
- [`agent-skill-golang-base`](./agent-skill-golang-base) - 地道 Go 语言规范、*x 扩展包与指针内存语义
- [`agent-skill-go-zero`](./agent-skill-go-zero) - go-zero 高并发微服务架构与生产闭环
- [`agent-skill-python-base`](./agent-skill-python-base) - 现代 Python 3.10+ 原生类型、*x 工具包与流式防 OOM
- [`agent-skill-python-fastapi`](./agent-skill-python-fastapi) - FastAPI 现代异步高并发 Web API 与 Pydantic v2
- [`agent-skill-python-crawler`](./agent-skill-python-crawler) - 逆向解析、反反爬对抗与大规模采集
- [`agent-skill-python-scripting`](./agent-skill-python-scripting) - Typer CLI、安全 Subprocess 与自动化脚本
- [`agent-skill-hyperf-framework`](./agent-skill-hyperf-framework) - Hyperf 协程常驻内存安全与四层架构

### 4. 前端与 Web 工程 (Frontend & Web Engineering)
- [`agent-skill-frontend-vanilla`](./agent-skill-frontend-vanilla) - 语义化 HTML5、CSS Tokens 与 Vanilla JS 现代规范
- [`agent-skill-react-nextjs`](./agent-skill-react-nextjs) - React 18、Next.js 15 (App Router) 与 Vite 现代工程规范
- [`agent-skill-vue3`](./agent-skill-vue3) - Vue 3 Composition API、Nuxt 3 与 Pinia 状态管理规范

### 5. 基础设施与中间件存储架构 (Infrastructure & Middlewares)
- [`agent-skill-mysql-mastery`](./agent-skill-mysql-mastery) - MySQL 8.0 架构设计、覆盖索引与慢查锁治理
- [`agent-skill-redis-mastery`](./agent-skill-redis-mastery) - Redis 规范键命名空间、缓存三灾难终结与 Lua 分布式锁
- [`agent-skill-rabbitmq-mastery`](./agent-skill-rabbitmq-mastery) - RabbitMQ 100% 消息零丢失闭环与背压限流
- [`agent-skill-docker-mastery`](./agent-skill-docker-mastery) - Docker 多阶段构建极致瘦身、Non-root 安全与 PID 1 优雅停机

---

## 🛠️ 便捷管理命令

- **一键批量拉取所有仓库更新**：
  ```bash
  cd ~/Documents/ai && ./sync-all.sh
  ```
- **日常手动编辑**：
  直接使用 VSCode / Cursor / Sublime 打开本目录即可：
  ```bash
  code ~/Documents/ai
  ```
