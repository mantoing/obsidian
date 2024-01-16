---
날짜: '"2024-01-16"'
넘버: 
태그: Next.js
출처: Next.js 공식문서
aliases:
---
### 날짜  2024-01-16 09:08

### 태그: #Next 

>[!메모]
>

# useRouter

`useRouter` 훅은 클라이언트 컴포넌트 내에서 프로그래밍 방식으로 라우트를 변경할 수 있게 해줍니다.
```tsx
'use client'
 
import { useRouter } from 'next/navigation'
 
export default function Page() {
  const router = useRouter()
 
  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

## `useRouter()`

- `router.push(href: string)`: 제공된 라우트로 클라이언트 사이드 Navigation을 수행합니다. 브라우저의 히스토리\ 스택에 새 항목을 추가합니다.
    
- `router.replace(href: string)`: 제공된 라우트로 클라이언트 사이드 Navigation을 수행하되, [브라우저의 히스토리 스택](https://developer.mozilla.org/en-US/docs/Web/API/History_API)에 새 항목을 추가하지 않습니다.
    
- `router.refresh()`: 현재 라우트를 새로 고칩니다. 서버에 새 요청을 만들고, 데이터 요청을 다시 가져오며, 서버 컴포넌트를 다시 렌더링합니다. 클라이언트는 영향을 받지 않는 클라이언트 측 React (예: `useState`) 또는 브라우저 상태 (예: 스크롤 위치)를 잃지 않고 업데이트된 React 서버 컴포넌트 페이로드를 병합합니다.
    
- `router.prefetch(href: string)`: 더 빠른 클라이언트 사이드 전환을 위해 제공된 라우트를 프리펫칭합니다.
    
- `router.back()`: 소프트 Navigation을 사용하여 브라우저의 히스토리 스택에서 이전 라우트로 돌아갑니다.
    
- `router.forward()`: 소프트 Navigation을 사용하여 브라우저의 히스토리 스택에서 다음 페이지로 이동합니다.
>  > **알아두면 좋은 점:**
> 
> - `push()`와 `replace()` 메소드는 새 라우트가 사전 가져온 경우, 그리고 새 라우트가 [동적 세그먼트](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)를 포함하지 않거나 현재 라우트와 동일한 동적 매개변수를 가지고 있는 경우에 소프트 Navigation]을 수행합니다.
> - `next/link`는 뷰포트에 보이는 라우트를 자동으로 사전 가져옵니다.
> - `refresh()`는 fetch 요청이 캐시된 경우 동일한 결과를 다시 생성할 수 있습니다. `cookies`와 `headers`와 같은 다른 동적 함수도 응답을 변경할 수 있습니다.

**`pages` 디렉토리에서 마이그레이션:**

- **새로운 `useRouter` 훅은 `next/router`가 아닌 `next/navigation`에서 가져와야 합니다.**
- `pathname` 문자열이 제거되었으며, 이는 [`usePathname()`](https://nextjs.org/docs/app/api-reference/functions/use-pathname)으로 대체되었습니다.
- `query` 객체가 제거되었으며, 이는 [`useSearchParams()`](https://nextjs.org/docs/app/api-reference/functions/use-search-params)으로 대체되었습니다.
- 현재 `router.events`는 지원되지 않습니다. 

# 라우터 이벤트

`usePathname`과 `useSearchParams`와 같은 다른 클라이언트 컴포넌트 훅을 구성하여 페이지 변경을 듣을 수 있습니다.

```tsx
'use client'
 
import { useEffect } from 'react'
import { usePathname, useSearchParams } from 'next/navigation'
 
export function NavigationEvents() {
  const pathname = usePathname()
  const searchParams = useSearchParams()
 
  useEffect(() => {
    const url = `${pathname}?${searchParams}`
    console.log(url)
    // You can now use the current URL
    // ...
  }, [pathname, searchParams])
 
  return null
}
```

이것은 레이아웃에 가져올 수 있습니다.

```tsx
import { Suspense } from 'react'
import { NavigationEvents } from './components/navigation-events'
 
export default function Layout({ children }) {
  return (
    <html lang="en">
      <body>
        {children}
 
        <Suspense fallback={null}>
          <NavigationEvents />
        </Suspense>
      </body>
    </html>
  )
}
```

**알아두세요:** [`useSearchParams()`](https://nextjs.org/docs/app/api-reference/functions/use-search-params)는 [정적 렌더링](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering#static-rendering-default) 동안 가장 가까운 `Suspense` 경계까지 클라이언트 측 렌더링을 일으키기 때문에, `<NavigationEvents>`는 [`Suspense` 경계](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#example) 내에 래핑됩니다.
### 원문
---

---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)