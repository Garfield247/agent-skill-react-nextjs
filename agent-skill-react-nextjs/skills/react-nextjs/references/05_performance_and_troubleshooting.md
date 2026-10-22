# React / Next.js 性能优化与排障武器库 (Performance & Troubleshooting)

## 1. 性能优化核心准则

### 1.1 动态导入与客户端隔离 (`next/dynamic`)
- 重型图表（如 ECharts/Three.js）或富文本编辑器必须使用 `next/dynamic` 配合 `ssr: false`：
  ```tsx
  import dynamic from 'next/dynamic';

  const HeavyChart = dynamic(() => import('@/components/features/Chart'), {
    loading: () => <div>图表加载中...</div>,
    ssr: false, // 规避 window is not defined
  });
  ```

### 1.2 强制使用 `next/image`
- **严禁** 在 Next.js 中直接书写裸 `<img>` 标签，必须采用 `next/image` 提供自动 WebP 转码、尺寸占位与首屏 `priority` 预加载。

### 1.3 `React.memo` 与 `useCallback` 审慎使用
- **严禁过度优化**：普通原子组件或单次点击不需要 `useCallback`；
- 当且仅当子组件包裹了 `React.memo` 且作为参数传递的函数引起严重重绘时，才使用 `useCallback` 稳定函数引用。

---

## 2. 经典排障速查表 (Troubleshooting Matrix)

| 异常现象 | 核心根因 (RCA) | 解决方案 |
| :--- | :--- | :--- |
| **Hydration mismatch 报错** | 服务端渲染的 HTML 与客户端首屏初始结果不一致（在组件内直接读取 `localStorage`、`window`、`Date.now()`） | 提取到 `useEffect` 中挂载后执行，或使用 `<div suppressHydrationWarning>` |
| **Server Component 报 useState 错误** | 在服务端组件中直接调用了客户端 Hook | 检查该组件是否需要用户交互，若需要则在顶部声明 `'use client'` |
| **"use client" 组件 re-render 过频** | Context Provider 的 `value` 未使用 `useMemo`，父组件每次渲染均生成新对象引用 | 使用 `useMemo` 稳定 Provider 的 value 引用，或将 Context 拆分为多个细粒度状态 |
| **路由切换后公共 UI 重新加载并闪烁** | 将共享布局写在 `page.tsx` 中 | 提取到各目录同级的 `layout.tsx` 中实现持久化状态保持 |
| **API 接口跨域 (CORS) 拦截** | Next.js API Route 未配置跨域响应头 | 在 `route.ts` 中显式设置 `Access-Control-Allow-Origin` 头 |
