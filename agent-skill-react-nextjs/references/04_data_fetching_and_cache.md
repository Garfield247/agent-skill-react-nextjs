# Next.js 15 数据获取与缓存策略 (Data Fetching & Caching)

## 1. App Router 扩展 Fetch 缓存语义

在 Server Component 中，扩展的 `fetch` API 提供声明式缓存控制：

```typescript
// 静态数据：强制缓存（SSG 效果）
const staticData = await fetch('https://api.example.com/config', {
  cache: 'force-cache',
});

// 动态数据：禁用缓存（每次请求动态拉取）
const dynamicData = await fetch('https://api.example.com/user/wallet', {
  cache: 'no-store',
});

// ISR 增量重新验证：按时间缓存（秒）
const isrData = await fetch('https://api.example.com/products', {
  next: { revalidate: 60 },
});

// 基于 Tag 标签的按需失效缓存
const taggedData = await fetch('https://api.example.com/articles', {
  next: { tags: ['articles'] },
});
```

---

## 2. Server Actions 变更与缓存失效

```typescript
// app/actions/article.ts
'use server';

import { revalidateTag, revalidatePath } from 'next/cache';

export async function publishArticle(formData: FormData) {
  const title = formData.get('title') as string;
  await db.article.create({ data: { title } });

  // ✅ 按标签或路径精准失效缓存
  revalidateTag('articles');
  revalidatePath('/dashboard/articles');
}
```
