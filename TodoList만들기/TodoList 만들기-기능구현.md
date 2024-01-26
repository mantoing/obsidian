---
날짜: '"2024-01-26"'
넘버: 
태그: javascript
출처: 
aliases:
---
### 날짜  2024-01-26 13:50

### 태그: #javascript #TodoList만들기


>[!메모]
>

### 원문
---

# 나만의 Todolist만들기

> 42서울의 동아리 중에 하나인 42.gg에서 프론트엔드 작업을 하기전 온보딩으로 TodoList만들기를 진행하면서 어떤식으로 진행하였는지를 회고하기 위해 작성한다!!!!

### 조건? 

최종적으로는 **Next.js**나 **React**로 TodoList를 구현할 과정이지만 프론트엔드 개발에서 가장 중요하다고 할수있는 javascript에 익숙해지기 위해서 **javascript, Html, Css** 로만 구현을 해보자

- 조건
	1. Javascript, Html, Css
	2. todo 생성(Create), 조회(Read) 기능 구현하기 (새로고침 고려 X)
	3. todo 완료 상태 체크 기능 구현하기 (정렬은 선택사항)
	4. todo 수정(Update), 삭제(Delete) 기능 구현하기 (새로고침 고려 X)
	5. 디자인 적용하기
	6. 무료로 배포하기


### 초기 Html세팅

사실 나는 처음 시작을 c와 c++로 시작해서 그런지 html을 대충만 공부한 상태였다.
그래서 사실 jsx형식에서 return에 넣어주는 html만 익숙했던것 같다. 
css와 js파일을 불러오는것 부터 난관이었던것 같다.

```html
	<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      ul {
        list-style-type: none;
      }
      body .tolist.checked {
        text-decoration: line-through;
      }
    </style>
    <title>TodoList</title>
  </head>
```


`<style>` 로 css를 직접적으로 넣어줄수도 있다.


```html
  <body>
    <h1>TodoList</h1>
    <div class="container">
      <input id="toinput" type="text" placeholder="New Task" />
      <button onclick="addBtn()">+</button>
      <ul id="tolist"></ul>
    </div>
    <script src="todo.js"></script>
  </body>
</html>
```

`<button>` 태그의 onClick을 통해 todo.js의 addbtn함수를 실행하며 실질적인 기능이 구현될 수 있도록 트리거를 주었다.


### Todo.js

```js
function enableEditing(element) {
  let oldValue = element.innerHTML;
  let inputField = document.createElement("input");
  inputField.type = "text";
  inputField.value = oldValue;

  inputField.addEventListener("blur", function () {
    element.innerHTML = inputField.value;
  });

  element.innerHTML = "";
  element.appendChild(inputField);
  inputField.focus();
}

function addBtn() {
  let toInput = document.getElementById("toinput");
  let doList = document.getElementById("tolist");
  console.log(toInput.value);
  if (!toInput.value) {
    alert("Plz input value");
    return;
  }

  let toItem = document.createElement("li");
  toItem.className = "tolist";

  let toCheckBox = document.createElement("input");
  toCheckBox.type = "checkbox";

  toCheckBox.addEventListener("change", function (e) {
    let listItem = e.target.closest("li");
    let textBlock = listItem.querySelector(".text-block");
    if (toCheckBox.checked) {
      textBlock.style.textDecoration = "line-through";
    } else {
      textBlock.style.textDecoration = "none";
    }
  });

  let textBlock = document.createElement("span");
  textBlock.className = "text-block";
  textBlock.innerHTML = toInput.value;

  let changeBtn = document.createElement("span");
  changeBtn.className = "changeBtn";
  changeBtn.innerHTML = "&nbsp;&nbsp;&nbsp change";
  changeBtn.addEventListener("click", function () {
    enableEditing(textBlock);
  });
  let deleteBtn = document.createElement("span");
  deleteBtn.className = "deleteBtn";
  deleteBtn.innerHTML = "&nbsp;&nbsp;&nbsp❌";
  deleteBtn.addEventListener("click", function () {
    toItem.remove();
  });
  toItem.appendChild(toCheckBox);
  toItem.appendChild(textBlock);
  toItem.appendChild(changeBtn);
  toItem.appendChild(deleteBtn);
  doList.appendChild(toItem);
  toInput.value = "";
}
```  


```js
  let toInput = document.getElementById("toinput");
  let doList = document.getElementById("tolist");
```
먼저 `getElementById` 를 사용해서 `"toinput", tolist` 를 가져와서 변수에 넣어주어 input의 값과 그값을 추가해서 늘려줄 리스트를 가져왔다.

> getElementById
	-> 문서 내에서 특정 ID를 가진 요소를 선택하고 반환한다.  만약 해당하는 ID를 가진 요소가 없다면 null을 반환한다.
	

```js
	let toItem = document.createElement("li");
	  toItem.className = "tolist";
```

밑에서 부터는 add로 어떠한 값을 나의 todolist에 저장할때 list에 넣어줄것인데 그 list의 구성요소를 하나하나 레고처럼 조립해서 넣어주기 위해 처리한 부분이다.

> createElement()
	-> 새로운 HTML요소를 생성하는 함수. 이 함수를 사하면 javascript에서 동적으로 html요소를 생성할 수 있다. 


```js
toCheckBox.addEventListener("change", function (e) {
    let listItem = e.target.closest("li");
    let textBlock = listItem.querySelector(".text-block");
    if (toCheckBox.checked) {
      textBlock.style.textDecoration = "line-through";
    } else {
      textBlock.style.textDecoration = "none";
    }
  });
```

체크박스를 클릭해서 Todolist에서 완료했다는 느낌을 주고싶어서 여러가지 방법을 처음에 고민했다. 처음에 생각했을때는 체크박스가 체크가 되어있을때 밑으로 목록을 내려서 해결된 list는 아래쪽으로 내리고 해결이 되지않은 list는 위로 올리고 싶었지만 notion이나 obsidian에서 체크박스 느낌으로 처리하고싶어져 변경하여 체크박스가 체크되어있다면 글자에 줄이그어지는 효과를 주었다.

이부분에서 문제가 있었는데 내 코드로 보면 수정버튼 삭제버튼 텍스트 부분이 한 리스트에 들어가 있는상태인데 이럴경우 줄이 수정버튼과 삭제버튼까지 그어지는 오류가 있어서 text-block이라는 쿼리까지만 줄이 그어지도록 설정했다.

> addEventListener()
> element.addEventListener(event, function, useCapture);
> 
>- element는 이벤트 리스너를 추가할 HTML 요소를 나타낸다.
>
>- event는 이벤트의 종류를 나타내는 문자열이다. 예를 들어, 'click', 'mouseover', 'keydown' 등의 이벤트가 있다.
>
>- function은 이벤트가 발생했을 때 실행할 함수를 나타낸다.
>
>- useCapture은 선택적 매개변수로, 이벤트 캡처링을 사용할지 여부를 나타낸다. 기본값은 false이며, 일반적으로 이 값을 변경할 필요는 없다

`e.target.closest` 를 사용해서 이벤트객체의 이벤트 발생지점에서 가장 가까운 `li` 요소를 찾아서 그 요소의 `.text-block` 까지 찾고 만약 체크박스가 체크가 되어있다면  `textDecoration` 를 사용해서 텍스트 속성을 변경해준다.



```js
  function enableEditing(element) {
  let oldValue = element.innerHTML;
  let inputField = document.createElement("input");
  inputField.type = "text";
  inputField.value = oldValue;

  inputField.addEventListener("blur", function () {
    element.innerHTML = inputField.value;
  });

  element.innerHTML = "";
  element.appendChild(inputField);
  inputField.focus();
}
  
  
  changeBtn.addEventListener("click", function () {
    enableEditing(textBlock);
  });
```

`enableEditing`이라는 함수를 추가해서 수정하는것을 처리 하였는데 수정 버튼을 누르면 입력칸이 생겨서 수정을 할수있고 이 입력칸의 포커스에서 벗어나게 된다면 수정이 이루어지게 만드는 함수이다.


```js
deleteBtn.addEventListener("click", function () {
    toItem.remove();
  });
```

삭제 버튼을 클릭하게되면 이벤트가 발생해서 list에 담겨있던 item을 삭제해준다.


```js
  toItem.appendChild(toCheckBox);
  toItem.appendChild(textBlock);
  toItem.appendChild(changeBtn);
  toItem.appendChild(deleteBtn);
  doList.appendChild(toItem);
```

위의 과정에서 붙여준 레고들을 하나의 toitem에 넣어준뒤 최종적으로 list에 담아주고 렌더링하게 된다.

> appendChild()
> -> parentNode.appendChild(newNode);
> 지정된 부모 노드의 자식 노드 목록의 마지막에 새로운 자식 노드를 추가한다.


## 초기화면
<img width="500" alt="스크린샷 2024-01-18 오후 9 13 58" src="https://github.com/Pierrot-jang-pilkyu/ft_transcendence-front_end/assets/90916425/3dba9609-762a-477a-a584-24401860dd00">

## 할일목록추가화면
<img width="500" alt="스크린샷 2024-01-18 오후 9 17 38" src="https://github.com/Pierrot-jang-pilkyu/ft_transcendence-front_end/assets/90916425/53c760f9-df31-455c-8cb5-de8a0a003697">

## 해결된 할일 처리(체크박스 체크)
<img width="500" alt="image" src="https://github.com/Pierrot-jang-pilkyu/ft_transcendence-front_end/assets/90916425/fbe33858-966c-45cd-99e2-26e4cfb00b17">


## 수정화면, 삭제

<img width="500" alt="image" src="https://github.com/Pierrot-jang-pilkyu/ft_transcendence-front_end/assets/90916425/21f6b9ef-0992-4e66-a073-9a2f28ceb0a5">

### 배포

배포는 github에서 제공하는 배포 서비스를 사용했다.

  https://mantoing.github.io/PurejsTodoList/

### 아쉬운점

만들고 난 후에 보니 수정부분의 입력칸이 너무 작은 문제가 있었고 blur로 포커스에 따라 변화가 이루어지도록 이벤트 등록을 해서 그런지 실수로 포커스를 잘못누르면 바로 수정이되는 불편함이 있다... 리팩토링시에 가장먼저 고쳐야할 부분인것 같다.

바닐라 자바스크립트를 사용하고 보니 react가 얼마나 대단한 것인지 깨닫는 계기가 되었다 ㅋㅋㅋ 

다음에는 localStorage에 데이터를 저장하고 불러오는 기능을 추가하고 어떤식으로 구현되었는지 알아보자.



---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)