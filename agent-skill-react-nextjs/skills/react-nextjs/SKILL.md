---
name: react-nextjs
description: >-
  React 18 + Next.js 15 (App Router) + Vite 现代前端工程规范技能。
  当任务涉及 React 组件开发、Next.js 全栈应用、Vite SPA 工程或 TypeScript 前端架构时自动激活。
  涵盖 App Router Server/Client Component 分层铁律、TypeScript 类型安全规范、
  React Hooks 最佳实践、性能优化（Code Splitting/Suspense/缓存）、
  状态管理选型矩阵、样式方案与调试排障武器库。
---

# React 18 / Next.js 15 / Vite 现代前端工程规范 (React & Next.js Skill)

本技能定义了基于 **React 18+、Next.js 15 (App Router) 及 Vite SPA** 开发、重构或审查现代前端系统时的架构准则、质量红线与渐进式手册索引。

---

## 1. 核心铁律与绝对红线 (Core Redlines)

1. 🚨 **Server Component 优先与客户端边界下推**：
   - Next.js App Router 中所有组件默认为 Server Component (RSC)；
   - 仅在需要用户交互（`onClick` / `onChange`）、组件状态（`useState` / `useEffect`）或浏览器专属 API 时，才在**叶子组件第一行**声明 `'use client'`；
   - 严禁在 Server Component 中调用客户端 Hooks，严禁在顶层根布局无脑挂载 `'use client'`。
2. 🚨 **TypeScript 严苛类型安全**：
   - 坚决杜绝 `any` 与 `@ts-ignore`；未知类型使用 `unknown` + 类型守卫；
   - 组件 Props 必须使用显式 `interface` 定义，禁止内联匿名对象；外部 API 必须声明契约接口。
3. 🚨 **函数组件与 Hooks 闭包依赖**：
   - 坚决淘汰 Class Component；自定义 Hook 统一以 `use` 为前缀；
   - `useEffect` 必须声明完整依赖项，凡涉及订阅与事件监听，**必须在 return 函数中执行副作用清理**，严禁内存泄漏。
4. 🚨 **性能与引用稳定性门禁**：
   - 严禁在可变列表循环中使用 `index` 作为 `key`；
   - Next.js 中严禁使用裸 `<img>` 标签，必须统一使用 `next/image` 确保自动 WebP 转码与 LCP 优化；
   - 严禁在组件渲染体内定义内联空对象/数组字面量直接传给子组件，防止无意义重绘。

---

## 2. 标准开发工作流 (Standard Workflow)

1. **边界规划**：优先将页面拆解为服务端数据拉取层（RSC）与可交互交互叶子节点（Client Component）；
2. **契约先行**：在 `types/` 中定义数据模型，结合 Zod 编写运行时校验 Schema；
3. **状态分层**：局部状态使用 `useState`，服务端缓存使用 TanStack Query / fetch 缓存，跨组件全局状态使用 Zustand；
4. **流式组装**：通过 `<Suspense fallback={<Loading />}>` 实现服务端并行流式渲染；
5. **验证交付**：运行 `npm run build` 或 `tsc --noEmit` 验证类型与打包完整性。

---

## 3. 渐进式专业参考手册 (Progressive Disclosure References)

根据具体涉及的业务场景，**按需查阅 `references/` 目录下的专门规约**，严禁将全量子文档一次性无脑全部展开：

| 场景模块 | 参考子文档 | 核心涵盖规范 |
| :--- | :--- | :--- |
| **工程拓扑与版本基线** | [01_project_structure.md](references/01_project_structure.md) | • Next.js 15 App Router 标准工程结构<br>• Vite SPA 目录规范<br>• React 18.3+ / Next.js 15 / Node 20 LTS 版本适配基线 |
| **RSC 与客户端交互模式** | [02_server_and_client_components.md](references/02_server_and_client_components.md) | • Server Component 异步数据拉取与 Metadata 生成<br>• Suspense 局部流式渲染与骨架屏<br>• Client Component `'use client'` 下推与防抖防重提交 |
| **Hooks 与状态管理** | [03_hooks_and_state.md](references/03_hooks_and_state.md) | • 自定义 Hook 规范与事件监听清理闭环<br>• 状态管理分级选型矩阵（局部/Context/Zustand/TanStack Query）<br>• Zustand 全局 Store 标准模版 |
| **数据获取与缓存机制** | [04_data_fetching_and_cache.md](references/04_data_fetching_and_cache.md) | • App Router 扩展 fetch 缓存控制（force-cache, no-store, revalidate）<br>• Server Actions 变更与 `revalidateTag` / `revalidatePath` 按需失效 |
| **性能优化与排障武器库** | [05_performance_and_troubleshooting.md](references/05_performance_and_troubleshooting.md) | • `next/dynamic` 动态导入与重型库客户端隔离<br>• `next/image` 关键性能优化<br>• Hydration mismatch、useState 跨界等经典排障表 |
