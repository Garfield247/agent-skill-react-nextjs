---
name: react-nextjs
description: >-
  React 18 + Next.js 15 (App Router) + Vite 现代前端工程规范技能。
  当任务涉及 React 组件开发、Next.js 全栈应用、Vite SPA 工程或 TypeScript 前端架构时自动激活。
  涵盖 App Router Server/Client Component 分层铁律、TypeScript 类型安全规范、
  React Hooks 最佳实践、性能优化（Code Splitting/Suspense/缓存）、
  状态管理选型矩阵、样式方案与调试排障武器库。
---

# React 18 / Next.js 15 / Vite 现代前端工程规范技能 (React & Next.js Skill)

## 概述 (Overview)

本技能定义了在 **React 18 + Next.js 15（App Router）及 Vite SPA** 项目中的工业级工程规范。
践行 **"Server Component 优先、TypeScript 全覆盖、最小 Client Bundle、可组合性优先"** 的现代前端第一性原理，
吸收社区 awesome-cursorrules、Vercel 官方最佳实践与 TanStack 生态标准精华。

---

# 1. 核心铁律与绝对红线 (Core Redlines)

### 🚨 铁律一：Next.js App Router - Server Component 优先
- 所有组件默认为 Server Component；
- 只在以下场景才加 "use client" 指令：需要 onClick/onChange 等用户交互、useState/useEffect、浏览器专属 API；
- **严禁** 在 Server Component 中使用 useState、useEffect、Event Handler；
- "use client" 边界必须尽可能下推到叶子组件层级（减少客户端 Bundle）。

### 🚨 铁律二：TypeScript 类型安全铁律
- 严禁使用 any 类型；用 unknown + 类型守卫代替；
- 严禁使用 @ts-ignore；优先修复类型而非压制报错；
- 组件 Props 必须定义显式 interface，禁止 inline type object 重复定义；
- API 响应必须定义强类型接口，禁止裸 JSON.parse 无类型断言。

### 🚨 铁律三：函数组件与 Hooks 规范
- 严禁使用 Class Component（除非维护遗留代码）；
- 自定义 Hook 必须以 use 前缀命名；
- useEffect 必须声明完整依赖数组，禁止空数组掩盖依赖缺失；
- 事件处理函数统一以 handle 前缀命名（handleClick、handleSubmit）。

### 🚨 铁律四：导入与路径规范
- 使用绝对路径导入（@/ 别名），禁止 ../../../ 相对路径地狱；
- 第三方库导入在自有模块之前；
- 禁止从包内部路径直接导入（如 react/cjs/react.development.js）。

### 🚨 铁律五：性能红线
- 严禁在渲染函数中定义对象/数组字面量作为 Props（每次 render 都是新引用）；
- 严禁在列表渲染中使用 index 作为 key（数据有稳定 ID 时）；
- Image 组件必须使用 next/image，禁止裸 img 标签（避免 LCP 劣化）。

---

# 2. 项目目录结构规范 (Project Architecture)

### 2.1 Next.js App Router 标准结构

```
src/
├── app/                          # App Router 路由层
│   ├── layout.tsx                # 根布局（字体/全局样式/Provider）
│   ├── page.tsx                  # 首页（Server Component）
│   ├── globals.css               # 全局样式
│   ├── (auth)/                   # 路由分组（不影响 URL）
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   └── dashboard/
│       ├── layout.tsx
│       └── page.tsx
├── components/                   # 共享组件
│   ├── ui/                       # 纯展示原子组件（Button、Input、Card）
│   └── features/                 # 业务功能组件
│       └── auth/
│           ├── LoginForm.tsx     # Client Component
│           └── UserAvatar.tsx
├── lib/                          # 工具函数与配置
│   ├── utils.ts                  # 通用工具（cn、formatDate 等）
│   ├── validations.ts            # Zod Schema 校验
│   └── api.ts                    # 请求封装
├── hooks/                        # 自定义 Hooks
│   └── useMediaQuery.ts
├── types/                        # 全局类型定义
│   └── index.ts
└── styles/                       # 补充样式（非 Tailwind 时）
```

### 2.2 Vite SPA 标准结构

```
src/
├── main.tsx                      # 入口
├── App.tsx                       # 根组件 + Router
├── pages/                        # 页面级组件
├── components/
│   ├── ui/                       # 原子组件
│   └── features/                 # 功能组件
├── hooks/
├── stores/                       # 状态管理（Zustand 等）
├── lib/
└── types/
```

---

# 3. 组件规范 (Component Patterns)

### 3.1 Server Component 标准模板

```tsx
// app/dashboard/page.tsx - Server Component（无 "use client"）
import { Suspense } from 'react';
import { getUserData } from '@/lib/api';
import { UserProfile } from '@/components/features/user/UserProfile';
import { LoadingSpinner } from '@/components/ui/LoadingSpinner';

interface PageProps {
  params: { id: string };
  searchParams: { tab?: string };
}

export default async function DashboardPage({ params, searchParams }: PageProps) {
  // ✅ 直接 await，无需 useEffect
  const user = await getUserData(params.id);

  return (
    <main>
      <h1>用户面板</h1>
      <Suspense fallback={<LoadingSpinner />}>
        <UserProfile user={user} tab={searchParams.tab} />
      </Suspense>
    </main>
  );
}

// ✅ 生成静态元数据
export async function generateMetadata({ params }: PageProps) {
  const user = await getUserData(params.id);
  return {
    title: `${user.name} 的面板`,
    description: `查看 ${user.name} 的工作台数据`,
  };
}
```

### 3.2 Client Component 标准模板

```tsx
'use client'; // 必须在文件顶部第一行

import { useState, useCallback } from 'react';

interface LoginFormProps {
  onSuccess: (token: string) => void;
  redirectUrl?: string;
}

export function LoginForm({ onSuccess, redirectUrl = '/' }: LoginFormProps) {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  // ✅ handle 前缀，useCallback 避免子组件不必要重渲染
  const handleSubmit = useCallback(async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    if (isLoading) return; // 防重复提交

    setIsLoading(true);
    setError(null);

    try {
      const formData = new FormData(event.currentTarget);
      const res = await fetch('/api/auth/login', {
        method: 'POST',
        body: JSON.stringify(Object.fromEntries(formData)),
        headers: { 'Content-Type': 'application/json' },
      });
      if (!res.ok) throw new Error(await res.text());
      const { token } = await res.json();
      onSuccess(token);
    } catch (err) {
      setError(err instanceof Error ? err.message : '登录失败，请重试');
    } finally {
      setIsLoading(false);
    }
  }, [isLoading, onSuccess]);

  return (
    <form onSubmit={handleSubmit} noValidate>
      {error && <p role="alert" className="error">{error}</p>}
      <button type="submit" disabled={isLoading} aria-busy={isLoading}>
        {isLoading ? '登录中...' : '登录'}
      </button>
    </form>
  );
}
```

### 3.3 自定义 Hook 规范

```typescript
// hooks/useMediaQuery.ts
import { useState, useEffect } from 'react';

// ✅ use 前缀 + 明确返回类型
export function useMediaQuery(query: string): boolean {
  const [matches, setMatches] = useState(false);

  useEffect(() => {
    const mediaQuery = window.matchMedia(query);
    setMatches(mediaQuery.matches);

    const handleChange = (event: MediaQueryListEvent) => {
      setMatches(event.matches);
    };

    mediaQuery.addEventListener('change', handleChange);
    return () => mediaQuery.removeEventListener('change', handleChange); // ✅ 清理副作用
  }, [query]); // ✅ 完整依赖声明

  return matches;
}
```

---

# 4. TypeScript 类型规范 (TypeScript Patterns)

### 4.1 API 响应类型定义

```typescript
// types/index.ts
export interface ApiResponse<T> {
  code: number;
  msg: string;
  data: T;
}

export interface User {
  id: string;
  name: string;
  email: string;
  avatarUrl: string | null;
  createdAt: string; // ISO 8601
}

// ✅ 使用 Zod 进行运行时校验
import { z } from 'zod';

export const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1).max(50),
  email: z.string().email(),
  avatarUrl: z.string().url().nullable(),
  createdAt: z.string().datetime(),
});

export type User = z.infer<typeof UserSchema>;
```

### 4.2 类型守卫范式

```typescript
// ✅ 用 unknown + 类型守卫代替 any
function isApiError(value: unknown): value is { code: number; msg: string } {
  return (
    typeof value === 'object' &&
    value !== null &&
    'code' in value &&
    'msg' in value
  );
}
```

---

# 5. 状态管理选型矩阵 (State Management)

| 场景 | 推荐方案 | 说明 |
| :--- | :--- | :--- |
| **组件局部状态** | `useState` / `useReducer` | 单组件内，不跨层 |
| **跨组件共享（轻量）** | React Context + `useContext` | ≤3层，避免过度 re-render |
| **客户端全局状态** | **Zustand** | 轻量、无 Provider 样板代码 |
| **服务器状态/缓存** | **TanStack Query（React Query）** | 自动缓存/失效/后台刷新 |
| **表单状态** | **React Hook Form** + Zod | 零受控组件，性能极佳 |

```typescript
// ✅ Zustand Store 标准写法
import { create } from 'zustand';

interface UserStore {
  user: User | null;
  setUser: (user: User | null) => void;
}

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}));
```

---

# 6. 性能优化规范 (Performance)

### 6.1 代码分割与懒加载

```tsx
import dynamic from 'next/dynamic';
import { lazy, Suspense } from 'react';

// ✅ Next.js：动态导入重型组件
const HeavyChart = dynamic(() => import('@/components/features/Chart'), {
  loading: () => <div>图表加载中...</div>,
  ssr: false, // 仅在客户端渲染（避免 SSR 报错）
});

// ✅ Vite：React.lazy + Suspense
const HeavyPage = lazy(() => import('@/pages/HeavyPage'));
```

### 6.2 图片优化（Next.js）

```tsx
import Image from 'next/image';

// ✅ 推荐：next/image 自动优化
<Image
  src="/hero.webp"
  alt="首页主视觉"
  width={1200}
  height={630}
  priority // 首屏大图加 priority，其余默认 lazy
  sizes="(max-width: 768px) 100vw, 1200px"
/>
```

### 6.3 memo 与 useCallback 使用原则

```tsx
// ✅ 仅当子组件渲染成本高且父组件频繁 re-render 时才用 memo
const ExpensiveChild = React.memo(({ data }: { data: Data[] }) => {
  return <DataTable data={data} />;
});

// ✅ 仅当函数作为 Props 传给 memo 组件或作为 useEffect 依赖时才用 useCallback
// ❌ 过度优化：普通 onClick 不需要 useCallback
```

---

# 7. Next.js 数据获取规范 (Data Fetching)

```typescript
// ✅ App Router - Server Component 直接 fetch（自动去重与缓存）
async function getProducts(): Promise<Product[]> {
  const res = await fetch('https://api.example.com/products', {
    next: { revalidate: 60 }, // ISR：60 秒重新验证
    // cache: 'no-store',      // 动态数据：禁用缓存
    // cache: 'force-cache',   // 静态数据：永久缓存
  });
  if (!res.ok) throw new Error('获取产品列表失败');
  return res.json();
}
```

---

# 8. 排障武器库 (Troubleshooting)

| 症状 | 根因 | 解决方案 |
| :--- | :--- | :--- |
| Hydration mismatch 报错 | Server/Client 渲染结果不一致 | 检查 Date.now()、Math.random() 或浏览器 API 在 Server 侧调用；用 suppressHydrationWarning 或 dynamic(ssr:false) |
| "use client" 组件 re-render 过频 | Context 值引用变化 | 拆分 Context 或用 useMemo 稳定引用 |
| Image 优化失效 | 使用了裸 img 标签 | 替换为 next/image |
| TypeScript 报 any 绕过 | 误用 as any | 用 unknown + 类型守卫 |
| useEffect 依赖缺失警告 | 依赖数组不完整 | 补全依赖；若确实需要排除，用 useRef 存储 |
| 路由切换时状态丢失 | 组件被重新挂载 | 使用 layout.tsx 持久化公共 UI 层 |
| API 路由 CORS 问题 | Next.js API 未配置响应头 | 在 route.ts 中手动设置 Access-Control-Allow-Origin |

---

# 9. 现代版本适配基线 (Version Baseline)

| 技术 | 推荐版本 | 关键变更点 |
| :--- | :--- | :--- |
| React | 18.3+ | Concurrent Features、useTransition、use() Hook |
| Next.js | 15.x | App Router、Server Actions、Partial Prerendering |
| TypeScript | 5.x | 装饰器标准化、const 类型参数 |
| Vite | 5.x | Rollup 4、环境变量类型安全 |
| Node.js | 20 LTS+ | Next.js 15 最低要求 |

> **存量项目适配**：优先检查 `package.json` 中的 next/react 版本，严禁强塞高版本不兼容 API（如 App Router 与 Pages Router 混用规范有差异）。
