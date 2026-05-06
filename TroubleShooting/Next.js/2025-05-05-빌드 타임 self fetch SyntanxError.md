# ☄️ Next.js 빌드 타임 self-fetch SyntaxError

## 1️⃣ 문제 상황

`next build` 실행 시 아래 에러가 발생하며 빌드가 실패했습니다.

```bash
Error occurred prerendering page "/".
SyntaxError: Unexpected token '<', "<!DOCTYPE "... is not valid JSON
    at JSON.parse (<anonymous>)
```

### 🚨 원인

Next.js는 빌드 시점(`next build`)에 페이지를 **prerender(사전 렌더링)** 합니다. 이 과정에서 서버 컴포넌트가 실행되는데, 문제는 **서버가 아직 실행중이지 않은 상태**라는 점 입니다.

```text
next build 실행
    ↓
페이지 prerender 시작 (서버 미실행 상태)
    ↓
서버 컴포넌트 실행 → fetch('http://localhost:3000/api/posts') 호출
    ↓
서버가 없으므로 HTML 에러 페이지 반환
    ↓
JSON.parse("<!DOCTYPE ...") → SyntaxError 발생
```

👉 빌드 중에 **자기 자신의 API route에 HTTP 요청**을 보내는 것이 근본 원인입니다.

---

## 2️⃣ 1차 해결 - SSR 빌드 오류 수정

API route 내부의 데이터 조회 로직을 **별도 함수로 분리**하고, 서버 컴포넌트에서는 HTTP 요청 대신 해당 함수를 **직접 호출**합니다.

### 🔹 데이터 로직 분리 (`lib/post.ts`)

```ts
import { getAllPosts } from "@/lib/github";
import { parsePostFile } from "@/lib/utils";

const PAGE_SIZE = 12;

export async function getPostsData({ page = 1, category, search }: {
  page: number;
  category?: string;
  search?: string;
}) {
  let trees = await getAllPosts();
  let more = false;

  if (category) {
    trees = trees.filter((tree) => tree.path.startsWith(category));
  }
  if (search) {
    trees = trees.filter((tree) =>
      tree.path.toLowerCase().includes(search.toLowerCase())
    );
  }

  const getDate = (name: string) => {
    const [y, m, d] = name.split("-").map(Number);
    return new Date(y, m - 1, d).getTime();
  };

  trees.sort((a, b) => getDate(b.name) - getDate(a.name));
  trees = trees.map((tree) => ({ ...tree, ...parsePostFile(tree.path) }));

  const start = (page - 1) * PAGE_SIZE;
  const selectedFiles = trees.slice(start, start + PAGE_SIZE);
  if (trees.length > start + PAGE_SIZE + 1) more = true;

  return {
    posts: selectedFiles,
    hasMore: more,
    nextPage: more ? page + 1 : null,
  };
}
```

### 🔹 API route는 분리된 함수를 호출 (`app/api/posts/route.ts`)

```ts
import { getPostsData } from "@/lib/posts";
import { NextRequest, NextResponse } from "next/server";

export async function GET(req: NextRequest) {
  const page = parseInt(req.nextUrl.searchParams.get("page") || "1", 10);
  const search = req.nextUrl.searchParams.get("search") || undefined;
  const category = req.nextUrl.searchParams.get("category") || undefined;

  const data = await getPostsData({ page, category, search });
  return NextResponse.json(data);
}
```

### 🔹 서버 컴포넌트에서 직접 호출 (`lib/api/posts.ts`)

```ts
import { getPostsData } from "@/lib/posts";
import { PostResponse } from "@/types/post";

export async function fetchPosts({ page = 1, category, search }: {
  page: number;
  category?: string;
  search?: string;
}): Promise<PostResponse> {
  return await getPostsData({ page, category, search }); // ✅ HTTP 없이 직접 호출
}
```

---

## 3️⃣ 2차 문제 — 클라이언트에서 서버 전용 코드 실행 오류

1차 해결 후 검색, 카테고리 필터, 무한 스크롤이 동작하지 않는 문제가 발생했습니다.

### 🚨 원인 

`fetchPosts`를 서버 직접 호출 방식으로 바꿨더니, 클라이언트 컴포넌트 (`'use client'`)의 `useInfiniteQuery`에서도 동일한 함수를 호출하게 되었습니다. `getPostData → getAllPosts`는 서버 전용 코드 (GitHub API, Node.js 환경 의존)라 클라이언트에서 실행할 수 없었습니다.

--

### 🔹 해결 — 서버용/클라이언트용 함수 분리 (`lib/api/posts.ts`)

```ts
// ✅ SSR 서버 컴포넌트용 — 함수 직접 호출
export async function fetchPostsServer({ page = 1, category, search }: {
  page: number;
  category?: string;
  search?: string;
}): Promise<PostResponse> {
  const { getPostsData } = await import("@/lib/posts");
  return await getPostsData({ page, category, search });
}

// ✅ 클라이언트용 — HTTP fetch
export async function fetchPosts({ page = 1, category, search }: {
  page: number;
  category?: string;
  search?: string;
}): Promise<PostResponse> {
  const query = new URLSearchParams({ page: page.toString() });
  if (category) query.append("category", category);
  if (search) query.append("search", search);

  const res = await fetch(`/api/posts?${query.toString()}`);
  if (!res.ok) throw new Error("게시글 데이터를 불러오지 못했습니다.");
  return res.json();
}
```

```ts
// app/page.tsx — SSR용 함수로 교체
import { fetchPostsServer } from "@/lib/api/posts";

await queryClient.prefetchInfiniteQuery({
  queryKey: [queryKeys.posts, undefined, ""],
  queryFn: ({ pageParam = 1 }) => fetchPostsServer({ page: pageParam }),
  initialPageParam: 1,
});
```

`useInfinitePosts` 훅은 `fetchPosts`(HTTP 버전)를 그대로 사용하므로 수정 불필요했습니다.

---