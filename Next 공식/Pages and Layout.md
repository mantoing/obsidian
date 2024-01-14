---
날짜: '"2024-01-15"'
넘버: 
태그: Next.js
출처: Next.js 공식문서
aliases:
---
### 날짜  2024-01-15 06:58

### 태그: #Next 

>[!메모]
>

### 원문
---

# 페이지

페이지는 라우트에 고유한 UI이다. page.js파일에서 컴포넌트를 내보내는 방식으로 페이지를 정의할 수 있다. 중첩된 폴더를 사용하여 라우트를 정의하고 page.js를 사용하여 해다 라우트에 공개적으로 접근할 수 있도록 할 수 있다ㅣ.

첫 번째 페이지를 생성하려면 app 디렉초리 내부에 page.js파일을 추가하라.

![page.js special file](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fpage-special-file.png&w=3840&q=75&dpl=dpl_2Hg7YB9FijwmZ7AkNaukczUuTkP7)

```tsx
export default function Page() {
  return <h1>Hello, Home page!</h1>;
}
```

```tsx
export default function Page() {
  return <h1>Hello, Dashboard Page!</h1>;
}
```

- 페이지는 항상 라우트의 서브트리 leaf이다.
- page.js파일은 라우트 세그먼트를 공개로 만드는 데 필요하다.
- 페이지는 기본적으로 서버 컴포넌트 이지만 클라이언트 컴포넌트로 설정할 수도 있다ㅣ.
- 페이지에서 데이터를 펫치 해올수 있다.


# 레이아웃

레이아웃은 여러 페이지에서 공유되는 공유되는ui이다. 페이지 전환 시 레이아웃은 상태를 유지하고 상호작용을 유지하며 다시 렌더링되지 않는다. 레이아웃은 또한 중첩될 수도 있다.

`layout.js` 파일에서 react컴포넌트를 default로 내보내는 것으로 레이아웃을 정의할 수 있다. 해당 컴포넌트는 렌더링 자식 레이아웃 또는 자식 페이지로 채워지는 자식 prop을 허용해야 한다.
![layout.js special file](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Flayout-special-file.png&w=3840&q=75&dpl=dpl_2Hg7YB9FijwmZ7AkNaukczUuTkP7)

```tsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <section>
      {/* 공유 UI를 여기에 포함. 예: 헤더 또는 사이드바 */}
      <nav></nav>

      {children}
    </section>
  );
}
```


- 최상위 레이아웃을 Root Layout이라고 합니다. 이 필수 레이아웃은 애플리케이션의 모든 페이지에서 공유됩니다. 루트 레이아웃은 html과 body 태그를 포함해야 합니다. 
- 어떤 라우트 세그먼트는 선택적으로 자체 레이아웃을 정의할 수 있습니다. 이러한 레이아웃은 해당 세그먼트의 모든 페이지에서 공유됩니다. 
- 라우트의 레이아웃은 기본적으로 중첩됩니다. 각 부모 레이아웃은 React의 children 속성을 사용하여 하위 레이아웃을 감쌉니다. 
- 특정 라우트 세그먼트를 공유 레이아웃에 포함하거나 제외하려면 Route Groups를 사용할 수 있습니다.
- 레이아웃은 기본적으로 서버 컴포넌트입니다만, 클라이언트 컴포넌트로 설정할 수도 있습니다. 
- 부모 레이아웃과 그 하위 컴포넌트 간에 데이터를 전달하는 것은 불가능합니다. 그러나 동일한 데이터를 라우트에서 여러 번 가져올 수 있으며, React는 성능에 영향을 주지 않고 자동으로 요청을 중복 제거할 수 있습니다. 
- 레이아웃은 현재 라우트 세그먼트에 접근할 수 없습니다. 라우트 세그먼트에 접근하려면 useSelectedLayoutSegment 또는 useSelectedLayoutSegments를 클라이언트 컴포넌트에서 사용할 수 있습니다. 
- .js, .jsx, 또는 .tsx 파일 확장자를 레이아웃으로 사용할 수 있습니다. 
- 같은 폴더에 layout.js와 page.js 파일을 정의할 수 있습니다. 레이아웃은 페이지를 감쌀 것입니다.


# 루트 레이아웃 (필수)

루트 레이아웃은 app디렉토리 최상위 수준에서 정의되며 모든 라우트에 적용된다. 이 레이아웃을 사용하면 서버에서 반환된 초기 HTML을 수정할 수 있다.

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

- app 디렉토리에는 반드시 루트 레이아웃이 포함되어야 한다. 루트 레이아웃은 - `<head>` HTML 요소를 관리하기 위해 [내장된 SEO 지원](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)을 사용할 수 있습니다. 예를 들어, `<title>` 요소를 관리할 수 있습니다.
- 여러 개의 루트 레이아웃을 만들기 위해 라우트 그룹(Route Groups)을 사용할 수 있습니다.
- 루트 레이아웃은 기본적으로 서버 컴포넌트(Server Components)이며 클라이언트 컴포넌트(Client Component)로 설정할 수 없습니다.

# 중첩 레이아웃

특정 경로 세그먼트가 활성화될 때 특정 폴더 내에 정의된 레이아웃 (예: app/dashboard/layou.js)은 해당 세그먼트에 적용되며 렌더링 된다. 파일 계층구조의 기본값으로 레이아웃은 자식 레이아웃을 자신의 자식 속성을 통해 둘러싸게 된다.
  
![Nested Layout](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fnested-layout.png&w=3840&q=75&dpl=dpl_2Hg7YB9FijwmZ7AkNaukczUuTkP7)

``` tsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return <section>{children}</section>
}
```

위의 두 레이아웃을 결합하려면 루트 레이아웃 (`app/layout.js`)이 대시보드 레이아웃 (`app/dashboard/layout.js`)을 둘러싸고, 대시보드 레이아웃은 `app/dashboard/*` 내부의 경로 세그먼트를 둘러싸게 된다.


# 템플릿

Templates은 각 자식 레이아웃 또는 페이지를 둘러싸는 레이아웃과 유사하다. 레이아웃은 경로 간에 지속되고 상태를 유지하는 반면, 템플릿은 탐색 시마다 자식의 새로운 인스턴스를 생성한다. 이는 **템플릿을 공유하는 라우트 간에 이동할 때, 컴포넌트의 새 인스턴스가 마운트되고 DOM 요소가 다시 생성되며 상태가 유지되지 않으며** 효과가 동기화된다는 것을 의미한다.

특정 동작이 필요한 경우 레이아웃보다 템플릿이 더 적합한 옵션일 수 있습니다. 예를 들면 다음과 같다:

- CSS 또는 애니메이션 라이브러리를 사용한 진입/이탈 애니메이션
- `useEffect`를 사용하는 기능 (예: 페이지 조회 기록 로깅) 및 `useState`를 사용하는 기능 (예: 각 페이지의 피드백 양식)
- 기본 프레임워크 동작 변경. 예를 들어, 레이아웃 내에서의 Suspense 경계는 페이지 전환 시에만 대체 콘텐츠를 보여주지만 템플릿의 경우 매번 탐색할 때마다 대체 콘텐츠가 표시다.
>권장사항: 특별한 이유가 없는 한 레이아웃을 사용하는 것이 좋습니다

템플릿은 `template.js` 파일에서 기본 React 컴포넌트를 내보내어 정의할 수 있다. 해당 컴포넌트는 자식 세그먼트가 중첩될 것으로 예상되는 `children` 속성을 받아야 한다.

  
![Nested Layouts](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fnested-layouts-ui.png&w=3840&q=75&dpl=dpl_2Hg7YB9FijwmZ7AkNaukczUuTkP7)

```tsx
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}
```

```tsx
<Layout>
  {/* 템플릿에 고유한 키를 부여하는 것에 유의. */}
  <Template key={routeParam}>{children}</Template>
</Layout>
```

# `<head>` 수정하기

`app` 디렉토리에서는 `<title>` 및 `<meta>`와 같은 `<head>` HTML 요소를 내장된 SEO 지원을 사용하여 수정할 수 있다.

메타데이터는 `layout.js` 또는 `page.js` 파일에서 `metadata` 객채 또는 `generateMetadata` 함수를 내보내어 정의할 수 있습니다.

```tsx
import { Metadata } from 'next'
 
export const metadata: Metadata = {
  title: 'Next.js',
}
 
export default function Page() {
  return '...'
}
```

> 알아두면 좋은 사항: 루트 레이아웃에는 `<title>` 및 `<meta>`와 같은 `<head>` 태그를 수동으로 추가하지 않아야 합니다. 대신 Metadata AP를 사용해야 하며, 이는 `<head>` 요소의 스트리밍 및 중복 처리와 같은 고급 요구 사항을 자동으로 처리합니다.

- **페이지**:
    
    - 페이지는 라우트에 대한 고유한 UI입니다.
    - `page.js` 파일을 사용하여 페이지를 정의할 수 있습니다.
    - 폴더 구조를 활용하여 라우트를 정의하고 해당 라우트에 대해 `page.js` 파일을 공개적으로 접근 가능하게 할 수 있습니다.
    - 페이지는 항상 라우트 서브트리의 말단에 위치합니다.
    - 페이지 파일은 `.js`, `.jsx`, 또는 `.tsx` 확장자를 가질 수 있습니다.
    - 페이지는 기본적으로 서버 컴포넌트입니다.
    - 페이지에서 데이터를 가져올 수 있습니다.
- **레이아웃**:
    
    - 레이아웃은 여러 페이지에서 공유되는 UI입니다.
    - 레이아웃은 페이지 전환 시 상태를 유지하고 상호작용을 유지합니다.
    - `layout.js` 파일을 사용하여 레이아웃을 정의할 수 있습니다.
    - 레이아웃은 중첩될 수 있습니다.
    - 레이아웃은 기본적으로 서버 컴포넌트입니다.
    - 레이아웃에서 데이터를 가져올 수 있습니다.
- **루트 레이아웃**:
    
    - 루트 레이아웃은 애플리케이션의 모든 페이지에서 공유되는 레이아웃입니다.
    - 루트 레이아웃은 `<html>`과 `<body>` 태그를 포함해야 합니다.
    - 루트 레이아웃은 app 디렉토리의 최상위 수준에 정의되어야 합니다.
- **중첩 레이아웃**:
    
    - 특정 경로 세그먼트에 정의된 레이아웃은 해당 세그먼트에 적용되고 중첩됩니다.
    - 레이아웃은 부모 레이아웃을 자식 레이아웃으로 감싸기 위해 `children` 속성을 사용합니다.
    - 라우트 그룹(Route Groups)을 사용하여 특정 경로 세그먼트를 선택적으로 포함하거나 제외할 수 있습니다.
- **템플릿**:
    
    - 템플릿은 자식 레이아웃 또는 페이지를 감싸는 레이아웃과 유사합니다.
    - 템플릿은 탐색 시마다 자식의 새로운 인스턴스를 생성합니다.
    - 템플릿은 특별한 동작이 필요한 경우에 유용합니다.
- **`<head>` 수정하기**:
    
    - `app` 디렉토리에서 `<head>` HTML 요소를 수정할 수 있습니다.
    - `layout.js` 또는 `page.js` 파일에서 `metadata` 객체나 `generateMetadata` 함수를 사용하여 메타데이터를 정의할 수 있습니다.
    - 루트 레이아웃에는 `<title>` 및 `<meta>`와 같은 `<head>` 태그를 수동으로 추가하지 않아야 합니다. 대신 Metadata API를 사용해야 합니다.


---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)