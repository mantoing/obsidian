---
날짜: '"2024-01-13"'
넘버: 
태그: Next.js
출처: Next.js 공식문서
aliases:
---
### 날짜  2024-01-13 21:37

### 태그: #Next 

>[!메모]
>

### 원문
---

> 모든 응용 프로그램의 골격은 라우팅이다.
> 이페이지는 next.js 에서  웹에서 라우팅의 기본 개념과  라우팅을 처리하는 방법을 공부한다.


![Terminology for Component Tree](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fterminology-component-tree.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2)

- # Terminology
	`Tree` : 계층구조를 시각화 하기위한 규약이다. 
	`Subtree` : 나무의 일부로 처음부터 기작해서 마지막으로 끝난다.
	`Root` : 트리 또는 서브트리의 첫번째 노드 ex) root layout
	`Leaf` : URl 경로의 마지막 세그먼트와 같이 자식이 없는 하위 세그먼트
		![Terminology for URL Anatomy](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fterminology-url-anatomy.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2)
	`URL Segment` : /로 구분되는 URL경로의 일부이다.
	`URL Path` :  도메인 뒤에오는 url의 일부.


# `app` Router

- `app` 이라는 이름을 가진 새 디렉토리에서 작동한다. 
  앱 디렉토리는 페이지 디렉토리와 함께 작동하여 증분채택이 가능하다.
   > 앱 라우터가 페이지 라우터보다 우선적으로 작동한다.
   

# Roles of Folders and Files

- Next.js는 파일시스템을 기반으로한 라우터를 사용한다. 
	 `Folders` 는 경로를 지정하는데 사용된다 .
	 경로는 루트 폴더부터 파일이 포함된 최종 리프 폴더까지 파일 시스템 계층구조를 따라가는 중첩된 폴더의 단일 경로이다.
	`files` 경로 세그먼트에 표시되는 UI를 만드는 데 사용된다.


# Route Segments

- 경로의 라우터는 라우트의 세그먼트를 나타낸다. 각각의 라우트 세그먼트는 URL경로에서 해당 세그먼트로 매핑된다.
	![How Route Segments Map to URL Segments](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Froute-segments-to-path-segments.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2)


# Nested Routes

중첩 라우트를 만드려면 중첩 폴더들을 서로 안에 중첩되게 만들수 있다.
예를 들어 새로운 앱 디렉토리에 두개의 새로운 폴더를 중첩해 `/dashboard/settings` 를 추가 해줄 수 있다.

# Component Hierarchy
 특정한 파일에서 정의되는 리액트 컴포넌트들의 라우터 세그먼드는 특정 계층에서 랜더링 된다.
 
 - `layout.js`
- `template.js`
- `error.js` (React error boundary)
- `loading.js` (React suspense boundary)
- `not-found.js` (React error boundary)
- `page.js` or nested `layout.js`

	![Component Hierarchy for File Conventions](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Ffile-conventions-component-hierarchy.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2)

	> 중첩 라우트에서 세그먼트의 구성 요소는 상위 세그먼트에서 정의된다.

# Advanced Routing Patterns
	
 [[Parallel Routes]] : 독립적으로 탐색할 수 있는 동일한 보기에  두개 이상의 페이지를 동시에 표시할 수 있다. 자체 하위 탐색이 있는 분활 보기에 사용할 수 있다.

[[Intercepting Routes]] : 경로를 가로채고 다른 경로의 컨텍스트에 표시할 수 있다.
현재 페이지를 유지하는게 중요할 때 사용할 수 있다. 피드에서 하나의 작업을 편집하거나 사진을 확정하는 동안 모든 작업을 본다.


요약 : 
- **라우팅**: 라우팅은 모든 애플리케이션의 핵심이며, 웹 라우팅의 기본 개념과 Next.js에서의 라우팅 처리 방법을 설명. 일부 중요한 용어로는 트리, 서브트리, 루트, 잎, URL 세그먼트 등이 있음.
    
- **app 라우터**: Next.js의 버전 13에서 도입된 App Router는 공유 레이아웃, 중첩 라우팅, 로딩 상태, 에러 처리 등을 지원하는 기능. React 서버 컴포넌트를 기반으로 하며, pages 디렉토리와 함께 사용되어 애플리케이션의 라우트를 선택적으로 업데이트할 수 있음. pages 디렉토리를 사용하는 경우 페이지 라우터 문서를 참조해야 함.
    
- **폴더와 파일의 역할**: Next.js는 파일 시스템 기반 라우터를 사용하며, 폴더는 라우트를 정의하는 데 사용. 폴더는 루트에서 leaf까지의 계층 구조를 따르며, leaf 폴더에는 해당 라우트 세그먼트의 UI를 생성하는 파일이 포함.
    
- **라우트 세그먼트**: 각 폴더는 라우트 세그먼트를 나타냄. 라우트 세그먼트는 URL 경로의 해당 세그먼트에 매핑.
    
- **중첩 라우트**: Next.js에서는 중첩된 라우트를 생성할 수 있습니다. 중첩 라우트는 폴더를 서로 중첩하여 구현됨. app 디렉토리에 두 개의 새로운 폴더를 중첩하면 Path가 추가돼 라우트를 중첩할 수 있습니다. 중첩된 라우트는 여러 세그먼트로 구성되며, `/dashboard/settings`는 루트 세그먼트인 `/`에 `dashboard` 세그먼트와 `settings` 리프 세그먼트로 구성됨.
    
- **파일 규칙**: Next.js는 중첩된 라우트에서 특정 동작을 가진 UI를 생성하기 위해 다양한 특별한 파일을 제공. 이 파일들은 특정한 역할을 수행하며 표에 정리가 돼있음.
    
- **컴포넌트 계층 구조**: 라우트 세그먼트의 특정 파일에 정의된 React 컴포넌트는 특정 계층 구조로 렌더링. 중첩된 라우트의 경우 세그먼트의 컴포넌트는 부모 세그먼트의 컴포넌트 안쪽에 중첩됨.
    
- **Colocation**: 컴포넌트 외에도 파일(컴포넌트, 스타일, 테스트 등)을 app 디렉토리의 폴더에 함께 배치할 수 있는 옵션이 있음. 이는 폴더가 경로를 정의하지만, page.js 또는 route.js에서 반환된 내용만 공개적으로 접근 가능한 이점을 제공.
    
- **클라이언트 측 탐색을 위한 서버 중심 라우팅**: 앱 라우터는 서버 중심 라우팅을 사용하여 클라이언트 측 탐색을 구현. 서버 컴포넌트와 서버에서의 데이터 가져오기와 일치시켜 클라이언트가 라우트 맵을 다운로드할 필요 없이 동일한 요청으로 라우트를 조회할 수 있음. 이를 통해 클라이언트 측 탐색과 부분 렌더링이 가능해지며, 라우터는 결과를 클라이언트 캐시에 저장하여 성능을 향상.
    
- **부분 렌더링**: 동일한 레이아웃을 공유하는 라우트에서 형제 페이지 간의 전환 시 변경된 라우트의 레이아웃과 페이지만 가져와서 렌더링하고, 상위 서브트리 세그먼트 이상의 내용은 다시 가져오거나 렌더링하지 않음. 이를 통해 데이터 양과 실행 시간을 줄이고 성능을 개선할 수 있음.
    

- **고급 라우팅 패턴**: Next.js의 App Router는 복잡한 라우팅 패턴을 구현하기 위한 규칙을 제공. 이를 활용하여 병렬 라우트와 라우트 가로채기와 같은 기능을 구현할 수 있으며, 이를 통해 더 풍부하고 복잡한 UI를 구축할 수 있음.

---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)
