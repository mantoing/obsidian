---
날짜: '"2024-01-13"'
넘버: 
태그: Next.js
출처: Next.js 공식문서
aliases:
---

### 날짜  2024-01-13 21:36

### 태그: #Next 

>[!메모]
>

### 원문
---
# Creating Routes

> Next.js는 폴더가 경로를 정의하는데 사용하는 파일 시스템 기반 라우터를 사용한다.

각 폴더는 URL 세그먼트에 매핑되는 라우트 세그먼트를 대표한다.
중첩 라우트를 만드려면 폴더들을 서로안에 중첩으로 생성할 수 있다.

![Route segments to path segments](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Froute-segments-to-path-segments.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2)

특별한 page.js 파일을 사용해서 공개적으로 라우트 세그먼트를 엑세스 할수 있게 한다.
![Defining Routes](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fdefining-routes.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2)

위의 예에서 `/dashboard/analytics` URL 경로는 해당 page.js에 엑세스 할 수 없기 때문에 공개적으로 엑세스 할 수 없다.

---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)