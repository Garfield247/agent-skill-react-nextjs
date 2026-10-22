# Server Component 与 Client Component 架构规范 (RSC Patterns)

## 1. Server Component 优先与下推原则

Next.js App Router 中，所有组件**默认为 Server Component (RSC)**：
- **核心优势**：在服务端直接访问数据源/数据库、零客户端 JS Bundle、自动流式传输；
- **下推原则**：只在需要 `onClick` / `onChange`、`useState` / `useEffect` 或浏览器 API 时，才在**叶子组件第一行**添加 `'use client'`。严禁在根页面或布局随意加 `'use client'` 导致整棵组件树退化为客户端渲染。

---

## 2. Server Component 标准模板

```tsx
// app/dashboard/page.tsx - 默认 Server Component（无 'use client'）
import { Suspense } from 'react';
import { getUserData } from '@/lib/api';
import { UserProfile } from '@/components/features/user/UserProfile';
import { LoadingSpinner } from '@/components/ui/LoadingSpinner';

interface PageProps {
  params: { id: string };
  searchParams: { tab?: string };
}

// ✅ 1. 顶层支持 async/await 直接拉取数据
export default async function DashboardPage({ params, searchParams }: PageProps) {
  const user = await getUserData(params.id);

  return (
    <main>
      <h1>用户面板</h1>
      {/* ✅ 2. 使用 Suspense 实现服务端局部流式渲染 */}
      <Suspense fallback={<LoadingSpinner />}>
        <UserProfile user={user} tab={searchParams.tab} />
      </Suspense>
    </main>
  );
}

// ✅ 3. 静态/动态 SEO 元数据生成
export async function generateMetadata({ params }: PageProps) {
  const user = await getUserData(params.id);
  return {
    title: `${user.name} 的面板`,
    description: `查看 ${user.name} 的工作台数据`,
  };
}
```

---

## 3. Client Component 标准交互模板

```tsx
'use client'; // 必须位于文件物理第一行

import { useState, useCallback } from 'react';

interface LoginFormProps {
  onSuccess: (token: string) => void;
  redirectUrl?: string;
}

export function LoginForm({ onSuccess, redirectUrl = '/' }: LoginFormProps) {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  // ✅ handle 前缀，useCallback 规避子组件无意义 re-render
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
