---
날짜: '"2024-01-15"'
넘버: 
태그: Next.js
출처: Next.js 공식문서
aliases:
---

### 날짜  2024-01-15 07:43

### 태그: #Next 

>[!메모]
>

# `<Link>` 컴포넌트

`<Link>`는 React 컴포넌트로 HTML `<a>` 요소를 확장하여 프리페칭(Prefetching)]과 라우트 간의 클라이언트 사이드 Navigation을 제공합니다. Next.js에서 라우트 간 이동하는 주요 방법입니다.

`<Link>`를 사용하려면 `<Link>`를 `next/link`에서 임포트하고 컴포넌트에 `href` 속성을 전달하면 됩니다:

```tsx
import Link from 'next/link'
 
export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

`<Link>`에는 선택적인 props를 전달할 수 있습니다. 자세한 내용은 API 참조를 참조하십시오.

## 예시

### 동적 세그먼트로 링크 연결하기

동적 세그먼트에 링크를 연결할 때, 템플릿 리터럴과 보간을 사용하여 링크 목록을 생성할 수 있습니다. 예를 들어, 블로그 글 목록을 생성하려면 다음과 같이 할 수 있습니다:

```tsx
import Link from 'next/link'
 
export default function PostList({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

### 활성 Links확인하기

링크가 활성 상태인지 확인하려면 `usePathname()`을 사용할 수 있습니다. 예를 들어, 현재 `pathname`이 링크의 `href`와 일치하는지 확인하여 활성 링크에 클래스를 추가할 수 있습니다:
```tsx
'use client'
 
import { usePathname } from 'next/navigation'
import { Link } from 'next/link'
 
export function Navigation({ navLinks }) {
  const pathname = usePathname()
 
  return (
    <>
      {navLinks.map((link) => {
        const isActive = pathname.startsWith(link.href)
 
        return (
          <Link
            className={isActive ? 'text-blue' : 'text-black'}
            href={link.href}
            key={link.name}
          >
            {link.name}
          </Link>
        )
      })}
    </>
  )
}
```

### id로 스크롤링하기
`<Link>`의 기본 동작은 변경된 라우트 세그먼트의 맨 위로 스크롤링하는 것입니다. `href`에 정의된 `id`가 있는 경우 일반 `<a>` 태그와 마찬가지로 해당 `id`로 스크롤링됩니다.

라우트 세그먼트의 맨 위로 스크롤링되지 않도록 하려면 `scroll={false}`로 설정하고 `href`에 해시된 `id`를 추가하면 됩니다:

```tsx
<Link href="/#hashid" scroll={false}>
  Scroll to specific id.
</Link>
```

## `useRouter()` 훅

`useRouter` 훅을 사용하면 클라이언트 컴포넌트 내에서 프로그래밍 방식으로 라우트를 변경할 수 있습니다.

`useRouter`를 사용하려면 `next/navigation`에서 임포트하고 클라이언트 컴포넌트 내에서 훅을 호출하면 됩니다:

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

`useRouter`는 `push()`, `replace()`, `reload()` 등과 같은 메서드를 제공합니다.

> 권장 사항: `useRouter`의 특정 요구 사항이 없는 한 라우트 간 이동에는 `<Link>` 컴포넌트를 사용하는 것이 좋습니다.

## Navigation 작동 방식

1. `<Link>`나 `router.push()`를 사용하여 라우트 전환을 시작합니다.
2. 라우터는 브라우저의 주소 표시줄의 URL을 업데이트합니다.
3. 라우터는 변경되지 않은 세그먼트(예: 공유 레이아웃)를 클라이언트 캐시에서 재사용하여 불필요한 작업을 피합니다. 이를 부분 렌더링(partial rendering)이라고도 합니다.
4. 소프트 Navigation 조건을 충족하는 경우, 라우터는 새 세그먼트를 서버에서 가져오는 대신 캐시에서 새 세그먼트를 가져옵니다. 그렇지 않으면 라우터는 하드 Navigation을 수행하고 서버에서 서버 컴포넌트 페이로드를 가져옵니다.
5. 생성된 경우, 로딩 UI가 페이로드를 가져오는 동안 서버에서 표시됩니다.
6. 라우터는 캐시된 또는 새로운 페이로드를 사용하여 클라이언트에서 새로운 세그먼트를 렌더링합니다.
### 렌더링된 서버 컴포넌트의 클라이언트 사이드 캐싱

새로운 라우터에는 서버 컴포넌트(페이로드)의 렌더링 결과를 저장하는 **인메모리 클라이언트 사이드 캐시**가 있습니다. 이 캐시는 라우트 세그먼트별로 분할되어 어떤 수준에서도 무효화하고 동시 렌더링 간 일관성을 보장합니다.

사용자가 앱 내에서 이동할 때, 라우터는 이전에 가져온 세그먼트의 페이로드 및 [**프리페치된**](https://velog.io/@okko8522/Routing-Linking-and-Navigating-%EB%B2%88%EC%97%AD-%EB%B0%8F-%EC%9A%94%EC%95%BD#%ED%94%84%EB%A6%AC%ED%8E%98%EC%B9%ADprefetching) 세그먼트를 캐시에 저장합니다.

이는 특정 경우에는 라우터가 캐시를 재사용하여 새로운 요청을 서버에 보내지 않아도 됩니다. 이렇게 하면 데이터를 다시 가져오고 컴포넌트를 불필요하게 다시 렌더링하는 것을 피함으로써 성능을 향상시킵니다.

### 캐시 무효화하기

데이터를 필요할 때 경로별(`revalidatePath`) 또는 캐시 태그별(`revalidateTag`)로 다시 유효성을 검사하기 위해 서버 액션을 사용할 수 있습니다.

### 프리페칭(Prefetching)

프리페칭은 방문하기 전에 백그라운드에서 라우트를 미리 로드하는 방법입니다. 프리페치된 라우트의 렌더링 결과는 라우터의 클라이언트 사이드 캐시에 추가됩니다. 이렇게 하면 프리페치된 라우트로 이동할 때 거의 즉시 화면이 나타납니다.

기본적으로 뷰포트에 표시되는 순간에 `<Link>` 컴포넌트를 사용하여 라우트가 프리페치됩니다. 이는 페이지가 처음로드되거나 스크롤할 때 발생할 수 있습니다. 라우트는 또한 `useRouter()` 훅의 `prefetch` 메서드를 사용하여 프로그래밍 방식으로 프리페치될 수 있습니다.

**정적 및 동적 라우트**:

- 라우트가 정적인 경우, 라우트 세그먼트의 모든 서버 컴포넌트 페이로드가 프리페치됩니다.
- 라우트가 동적인 경우, 첫 번째 공유 레이아웃부터 첫 번째 `loading.js` 파일까지의 페이로드가 프리페치됩니다. 이렇게 하면 동적 라우트의 전체 프리페치 비용이 감소하고 동적 라우트에 대한 즉시 로딩 상태가 가능해집니다.

> - 프리페칭은 프로덕션 환경에서만 활성화됩니다.
> - `<Link>`에 `prefetch={false}`를 전달하여 프리페칭을 비활성화할 수 있습니다.

### 소프트 Navigation

Navigation 발생 시 변경된 세그먼트의 캐시(있는 경우)를 재사용하고 데이터를 서버에 새로 요청하지 않습니다.

#### 소프트 Navigation 조건

Next.js는 소프트 Navigation을 사용하여 다음 조건을 충족하는 경우 소프트 Navigation을 수행합니다:

- 이동하려는 라우트가 [**프리페치**](https://velog.io/@okko8522/Routing-Linking-and-Navigating-%EB%B2%88%EC%97%AD-%EB%B0%8F-%EC%9A%94%EC%95%BD#%ED%94%84%EB%A6%AC%ED%8E%98%EC%B9%ADprefetching)되었고, 동적 세그먼트를 포함하지 않거나 현재 라우트와 동일한 동적 매개변수를 가지고 있는 경우.

예를 들어, 다음과 같은 동적 `[team]` 세그먼트를 포함하는 라우트를 고려해보세요: `/dashboard/[team]/*`. 아래의 캐시된 세그먼트는 `/dashboard/[team]/*`가 변경될 때만 무효화됩니다.

- `/dashboard/team-red/*`에서 `/dashboard/team-red/*`로 이동하는 경우 소프트 Navigation이 수행됩니다.
- `/dashboard/team-red/*`에서 `/dashboard/team-blue/*`로 이동하는 경우 하드 Navigation이 수행됩니다.

### 하드 Navigation

Navigation 발생 시 캐시가 무효화되며 서버가 데이터를 다시 가져오고 변경된 세그먼트를 다시 렌더링합니다.

### 뒤로/앞으로 Navigation

뒤로/앞으로 Navigation(popstate 이벤트)는 소프트 Navigation 동작을 수행합니다. 이는 클라이언트 사이드 캐시가 재사용되고 Navigation이 거의 즉시 진행됨을 의미합니다.

### 초점 및 스크롤 관리

기본적으로 Next.js는 Navigation 시 변경된 세그먼트에 초점을 설정하고 스크롤을 보정하여 보여줍니다


Next.js의 라우터는 클라이언트 사이드 네비게이션을 위해 서버 중심의 라우팅을 사용합니다. 이는 클라이언트 사이드 상태를 유지하고 다시 렌더링을 피하며, 중단 가능하고 경합 조건을 발생시키지 않는 네비게이션을 지원합니다.

라우트 간 이동에는 다음 두 가지 방법이 있습니다:

1. `Link` 컴포넌트
2. `useRouter` 훅

`<Link>` 컴포넌트는 클라이언트 사이드 네비게이션을 위해 HTML `<a>` 요소를 확장한 React 컴포넌트입니다. `<Link>`를 사용하려면 href 속성에 이동할 경로를 전달합니다. 이를 통해 프리페칭과 클라이언트 사이드 네비게이션을 제공할 수 있습니다.

`useRouter` 훅을 사용하면 클라이언트 컴포넌트 내에서 프로그래밍 방식으로 라우트를 변경할 수 있습니다. `useRouter`를 사용하여 라우터 객체를 가져온 후 `push()`, `replace()`, `reload()` 등의 메서드를 사용하여 라우트를 변경할 수 있습니다.

`<Link>` 컴포넌트와 `useRouter` 훅은 네비게이션 기능을 제공하며, 프리페칭과 클라이언트 캐싱을 통해 성능을 향상시킬 수 있습니다.

### 원문
---



---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)