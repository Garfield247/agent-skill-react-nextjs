# Next.js 15 (App Router) 与 Vite SPA 目录结构与版本基线 (Project Structure & Baseline)

## 1. Next.js 15 App Router 标准工程结构

```text
src/
├── app/                          # App Router 路由与服务端渲染层
│   ├── layout.tsx                # 根布局（字体/全局样式/全局 Provider）
│   ├── page.tsx                  # 首页（默认 Server Component）
│   ├── globals.css               # 全局样式
│   ├── (auth)/                   # 路由分组（组织结构，不影响 URL）
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   └── dashboard/
│       ├── layout.tsx            # 局部共享布局
│       └── page.tsx
├── components/                   # 共享组件
│   ├── ui/                       # 纯展示原子组件（Button、Input、Dialog）
│   └── features/                 # 业务功能组件（内聚完整业务交互）
│       └── auth/
│           ├── LoginForm.tsx     # 明确标注 'use client'
│           └── UserAvatar.tsx
├── lib/                          # 工具函数与外部库封装
│   ├── utils.ts                  # 通用样式合并 (cn) 与格式化
│   ├── validations.ts            # Zod 数据校验 Schema
│   └── api.ts                    # 请求封装
├── hooks/                        # 自定义 Hooks
│   └── useMediaQuery.ts
├── types/                        # TypeScript 全局契约定义
│   └── index.ts
└── styles/                       # 补充样式模块
```

---

## 2. Vite SPA 标准前端工程结构

```text
src/
├── main.tsx                      # 应用主入口
├── App.tsx                       # 根组件与路由装配
├── pages/                        # 页面级组件
├── components/
│   ├── ui/                       # 原子展示组件
│   └── features/                 # 业务功能组件
├── hooks/                        # 状态与副作用抽象
├── stores/                       # 客户端全局状态（Zustand 等）
├── lib/                          # 基础辅助库与网络客户端
└── types/                        # 类型定义
```

---

## 3. 现代技术版本基线 (Version Baseline)

| 技术栈 | 推荐基线版本 | 关键架构特性 |
| :--- | :--- | :--- |
| **React** | 18.3+ | 并发特性 (Concurrent Features)、`useTransition`、Action 机制 |
| **Next.js** | 15.x | App Router 默认开启、Partial Prerendering (PPR)、Turbopack |
| **TypeScript** | 5.x | `const` 类型参数、满足严格 `strict: true` |
| **Vite** | 5.x | 基于 Rollup 4、快速冷启动与 HMR |
| **Node.js** | 20 LTS+ | Next.js 15 生产级运行时最低要求 |

> **存量感知铁律**：前置检查 `package.json`，若是 Pages Router 遗留项目（`pages/` 目录），严禁混入 App Router 专有 Server Actions 语法。
