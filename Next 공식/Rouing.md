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
---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)
