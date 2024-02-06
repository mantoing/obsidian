---
날짜: '"2024-02-07"'
넘버: 
태그: 
출처: 
aliases:
---
### 날짜  2024-02-07 00:40

### 태그:

>[!메모]
>

### 원문
---

### **React Router**

리액트 라우터 돔을 공부하기 전에 먼저 라우팅이라는 개념을 알아야 한다.

라우팅이란?
- 사용자가 요청한 URL에 따라 알맞는 페이지를 보여주는 것을 의미한다.

먼저 Routing이란 사용자가 요청한 URL에 따라 해당 URL에 맞는 페이지를 보여주는 것이다.
하지만 여기서 a태그만 사용하더라도 페이지 이동이 가능한데 굳이 react router을 사용하는 이유가 뭘까 ? 

> a태그는 화면을 새로고침 한 다음 페이지를 이동한다는 단점이 있다. 하지만 react router은 새로운 페이지를 로드하지 않고 하나의 페이지 안에서 필요한 데이터만 가져오는 형태를 가지기 때문에 불필요하게 필요한 렌더링을 막을수 있다.



### **사용법**

1. 리액트 라우터 라이브러리 설치

```
	npm install react-router-dom
```

2. 프로젝트에 적용

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { BrowserRouter } from 'react-router-dom';
ReactDOM.render( 
	<BrowserRouter> 
		<App /> 
	</BrowserRouter>,
	document.getElementById('root')
);
```

3. Route 컴포넌트로 득정 경로에 원하는 컴포넌트 보여주기

```jsx
	<Route path="주소규칙" element={보여 줄 컴포넌트 JSX} />
```

``` jsx
	import { Route, Routes } from 'react-router-dom';
	import About from './pages/About';
	import Home from './pages/Home';
	const App = () => {
		return (
			<Routes> 
				<Route path="/" element={<Home />} /> 
				<Route path="/about" element={<About />} /> 
				</Routes> 
		); 
	}; 
	
	export default App;
```
4. Link 컴포넌트를 사용하여 다른 페이지로 이동하는 링크

```jsx
	<Link to="경로">링크 이름</Link>
```

``` jsx
	import { Link } from 'react-router-dom';
	const Home = () => { 
		return ( 
			<div> 
				<h1>홈</h1> 
				<p>가장 먼저 보여지는 페이지입니다.</p> 
				<Link to="/about">소개</Link> 
			</div> 
		);
	};

	export default Home;
```


### **React Router DOM의 주요 컴포넌트


리액트 라우너는 주로 세 가지 주요 컴포넌트를 제공한다.

1. **BrowserRouter**: HTML5의 History API를 사용하여 브라우저의 주소 표시줄을 조작할 수 있게 해주는 컴포넌트이다. 주소와 관련된 정보를 관리하고, 리액트 라우터의 다른 컴포넌트들이 이 정보를  사용할 수 있도록 제공한다.
2. **Route** : 주어진 경로에 대한 라우팅 규칙을 정의하는 컴포넌트이다. 경로와 컴포넌트를 매핑하여, 해당 경로가 일치할 때 특정 컴포넌트를 렌더링 할 수있도록 디원한다.
3. **LINK**: 클릭 시에 라우터가 제공하는 라우팅 규칙에 따라 새로운 경로로 이동할 수 있는 하이퍼링크를 생성하는 컴포넌트이다. 일반적인 `<a>` 태그와 유사하지만, 리액트 라우터가 제공하는 내부 매커니즘을 활용하여 페이지 간의 부드러운 전환을 가능하게 한다.


### **React router 와 React router dom**

React router 와 React router dom의 차이는 무엇일까?

React Router는 라우팅을 처리하는 핵심 라이브러리이고, React Router DOM은 react router를 웹에서 사용하기 쉽도록 확장한 라이브러리이다.
즉, React Router DOM은 React Router을 포함한다.


- **두 가지로 나뉘어진 이유**
	 React router은 특정 플랫폼에서 라우팅 기능을 지원하기 위해 설계되었다. 특정 플랫폼이라고 하면 웹이나 네이티브 앱을 예를 들 수있다. React router를 사용하여 웹 애플리 케이션을 개발할 때 일반적으로 웹 브라우저와 상호작용 하고 웹에서의 라우팅을 처리하기 위한 도구와 컴포넌트가 필요했다.
	 
	 React router DOM은 React 컴포넌트와 함께 사용되며, HTML5 History API를 사용하여 브라우저의 주소 표시줄을 관리하고, URL경로에 따라 컴포넌트를 렌더링 하여, 링크를 생성하고 활성 상태를 관리하는 등 웹 브라우저에서의 라우팅에 필요한 기능을 제공한다.



### **v5 -> v6**

1. Switch -> Routes 네이밍 변경

``` jsx
// v5
<Switch>
  <Route ... />
</Routes>

// v6
<Routes>
  <Route ... />
</Routes>
```


2. StaticRouter의 import 위치 변경
``` jsx
// v5
import { StaticRouter } from 'react-router-dom';
// v6
import { StaticRouter } from 'react-router-dom/server';
```

3. exact 옵션 삭제

기존의 `/` 라우트의 경우 **React Router의 디폴트 매칭 규칙 으로 인해 앞부분만 일치해도 전부 매칭**되기때문에 정확히 라우트를 일치시키고자 `exact` 속성을 사용하였으나 v6부터 기본적으로 정확히 일치하도록 매칭 규칙이 변하여 **v6에서부터는 `exact` 의 옵션을 더이상 사용하지 않는다**

```jsx
// categores 로 시작되는 모든 라우팅 매칭
<Route path='categories/*' />
```

4. route에서 컴포넌트 렌더링

```jsx
import UserInfo from './UserInfo'; // 

// v5 버전 사용 예시
<Route path='user' component={UserInfo} />
<Route
  path='user'
  render={routeProps => (
    <UserInfo routeProps={routeProps} isLogin={true} />
  )}
/>

// v6 버전 사용 예시
<Route path='user' element={<UserInfo />} />
<Route path='user' element={<UserInfo isLogin={true} />} />
```

5. 중첨 라우팅

``` jsx

//v5
import {
  BrowserRouter,
  Switch,
  Route,
  Link,
  useRouteMatch
} from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Switch>
        <Route path="/user" component={User} />
      </Switch>
    </BrowserRouter>
  );
}

function User() {

  const { path } = useRouteMatch();
  return (
    <div>
      <Switch>
        <Route path={`${path}/detail`}>
          <UserDetail />
        </Route>
      </Switch>
    </div>
  );
}
```

v6버전에서는 **하나의 파일에 모든 경로를 지정하고 중첩영역 렌더링 요소에는 `Outlet` 속성을 제공하여 더욱 간결하게 중첩된 라우트 구조를 사용하도록 개선되었다**

```jsx
import {
  BrowserRouter,
  Routes,
  Route,
  Outlet
} from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='user' element={<User />} >
          <Route path='detail' element={<UserDetail />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

function User() {
  return (
    <>
      <Outlet />
    </>
  )
}
```

6. useHistory 훅 변경 useNavigate

```jsx
// v5
const history = useHistory();

history.push('/home');
history.replace('/home');

// v6
const navigate = useNavigate();

navigate('/home');
navigate('/home', {replace: true});

// v6 에서의 앞으로, 뒤로 가기 사용방법 변화 
<button onClick={() => navigate(-2)}>Go 2 pages back</button>
<button onClick={() => navigate(-1)}>Go back</button>
<button onClick={() => navigate(1)}>Go forward</button>
<button onClick={() => navigate(2)}>Go 2 pages forward</button>
```

7. useRoutes

**기존의 `react-router-config` 가 v6 에서 `useRoutes` 라는 훅으로 이동**되었다. 이제 별도로 패키지를 추가설치 하지 않고 `useRoutes` 훅을 사용해 routes 를 구성할 수 있게 되었다.

```jsx
function App() {
  const element = useRoutes([
    // These are the same as the props you provide to <Route>
    { path: '/', element: <Home /> },
    { path: 'dashboard', element: <Dashboard /> },
    { path: 'invoices',
      element: <Invoices />,
      // Nested routes use a children property, which is also
      // the same as <Route>
      children: [
        { path: ':id', element: <Invoice /> },
        { path: 'sent', element: <SentInvoices /> }
      ]
    },
    // Not found routes work as you'd expect
    { path: '*', element: <NotFound /> }
  ]);

  // The returned element will render the entire element
  // hierarchy with all the appropriate context it needs
  return element;
}
```

---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)