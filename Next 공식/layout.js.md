
### **# layout.js 요약 정리**

- **루트 레이아웃(Root Layout)**
    
    - 루트 레이아웃(`/app/layout.js)은 반드시 존재해야 한다.
    - 루트 레이아웃은 Nextjs 웹 애플리케이션의 모든 page.js에 적용된다.
    - 루트 레이아웃은 반드시 `<html>`, `<body>` 태그를 포함해야 한다.
    - 루트 레이아웃은 반드시 Server Component여야 하며, Client Component로 전환 불가하다.
    

- **중첩 레이아웃(Nesting Layout)**
    
    - 모든 라우트 경로(route segment)에 별도의 레이아웃(layout.js)를 정의할 수 있으며,  해당 경로의 page.js에만 적용된다.
    - 레이아웃은 기본적으로 중첩되어 적용된다. 따라서 상위 경로의 레이아웃은 하위 경로의 레이아웃을 감싼다.
    

- **서버/클라이언트 컴포넌트(Server/Client Component)**
    
    - Layout은 디폴트로 서버 컴포넌트이다.
        
        - 따라서, asycn 컴포넌트 함수로 정의하여, 직접 await fetch() 호출이 가능하다.
        - 단, 부모-자식 레이아웃 간에는 데이터 전달이 불가하다.
        - 하지만, 부모 레이아웃, 자식 레이아웃에서 동일한 fetch() 각각 호출해도 성능에 전혀 영향이 없다.
        - Request Memoization 를 통해 동일한 경로의 fetch() 호출이 있으면 자동으로 중복 제거(Dedupe)처리 해주기 때문이다.
        
    - Layout은 파일 최상단에 "use client"를 표기하여 Client Component로 전환할 수 있다.
        
        - 단 루트 레이아웃(Root Layout)은 반드시 Server Component 여야만 한다.
        
    

- **SEO(검색 최적화) 지원**
    
    - HTML `<head>` 태그의 title, description 도 개발자가 컨트롤할 수 있다.
    - 단, 직접 루트 레이아웃에서 head 태그를 추가하고,title, meta 태그를 정의하지 말자.
    - Next.js에서 제공하는 MetaData API를 이용하면 된다. layout.js 및 page.js 파일에 서 사용 가능하다.
        
        - **metadata** 객체 : 변하지 않는 정적 메타 데이터(Static meta data)를 정의할 때 사용한다.
        - **generateMetadata()** 함수: 서버 등에서 매번 불러와야 하는 하는 동적 메타 데이터(Dynamic meta data)를 정의할 때 사용한다.
        
    

- **라우트 그룹(Route Groups)**
    
    - /app 내부에서 폴더명을 (그룹명)으로 정의하면, 라우트 그룹화가 가능하다.
    - 라우트 그룹마다 별도의 레이아웃을 적용할 수도 있다.
    

- **접속 경로(Route Segment)**
    
    - /app 내부에서 폴더명을 /[id] 나 /[category] 등으로 정의하면, 해당 위치의 경로는 다양한 주소로 접속 가능한 동적 라우트(Dynaimc Route)가 된다.
    - 실제로 브라우저에서 접속한 경로명 (id나 category 등)은 param 프롭으로 받을 수 있다.
    - 단, 루트 레이아웃이 아닌 중첩 레이아웃에서만 받을 수 있다.
    - 루트 레이아웃은 param 프랍을 통해서는 현재 접속 경로(route segment)를 파악할 수 없으므로, Client Component의 useSelectedLayoutSegment나 useSelectedLayoutSegments 훅을 이용해야 한다.
    

- **Search Params(쿼리 스트링)**
    
    - Layout은 page.js와 다르게 쿼리 스트링을 searchParams 프랍으로 받지 못한다.
    - page.js의 searchParams 프랍이나, Client Component의 `useSearchParms` 훅을 이용해야 한다.

출처: [https://curryyou.tistory.com/549](https://curryyou.tistory.com/549) [카레유:티스토리]