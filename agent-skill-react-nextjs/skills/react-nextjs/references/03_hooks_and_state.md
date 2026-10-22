# 自定义 Hook 规范与状态管理选型 (Hooks & State Management)

## 1. 自定义 Hook 设计铁律与副作用清理

```typescript
// hooks/useMediaQuery.ts
import { useState, useEffect } from 'react';

// ✅ 1. use 前缀 + 明确返回基础类型或元组
export function useMediaQuery(query: string): boolean {
  const [matches, setMatches] = useState(false);

  useEffect(() => {
    const mediaQuery = window.matchMedia(query);
    setMatches(mediaQuery.matches);

    const handleChange = (event: MediaQueryListEvent) => {
      setMatches(event.matches);
    };

    mediaQuery.addEventListener('change', handleChange);
    // ✅ 2. 必须返回清理函数，防止内存泄漏
    return () => mediaQuery.removeEventListener('change', handleChange);
  }, [query]); // ✅ 3. 严格声明完整依赖，禁止遗漏

  return matches;
}
```

---

## 2. 状态管理分级选型矩阵

| 场景维度 | 推荐架构方案 | 核心价值与边界 |
| :--- | :--- | :--- |
| **组件私有状态** | `useState` / `useReducer` | 状态仅供单个组件树使用，不跨层传播 |
| **浅层跨组件（≤3 层）** | React Context (`createContext`) | 主题色、全局国际化、轻量 UI 开关 |
| **客户端全局状态** | **Zustand** | 无 Provider 样板代码、按需订阅防多余 re-render |
| **服务端状态与异步缓存**| **TanStack Query (React Query)** | 自动重试、缓存失效、后台静默刷新、去重 |
| **高性能表单** | **React Hook Form** + Zod | 非受控组件极速响应，结合 Zod 严格校验 |

---

## 3. Zustand 全局 Store 标准模版

```typescript
// stores/userStore.ts
import { create } from 'zustand';

export interface User {
  id: string;
  name: string;
  email: string;
}

interface UserState {
  user: User | null;
  setUser: (user: User | null) => void;
  clearUser: () => void;
}

export const useUserStore = create<UserState>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));
```
