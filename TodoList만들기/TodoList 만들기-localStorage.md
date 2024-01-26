---
날짜: '"2024-01-26"'
넘버: 
태그: javascript
출처: 
aliases:
---
### 날짜  2024-01-26 16:46

### 태그:

>[!메모]
>

### 원문
---

# localstorage에 저장하기!


기본기능 구현 이후로 todolist목록을 저장하려고 하는데 백엔드가아닌 프론트엔드인 나는 3가지 방법중 하나를 골랐다.

1. localstorage에 저장하기.
2. local server에 저장하기
3. mock api에 저장하기

이중에서 localstorage가 백엔드의 유무에 상관없이 프론트엔드 개발자가 개발할때 많이 사용한다고 해서 localstorage에 저장해보기로 했다.

### 웹스토리지 (Web Storage)

웹 스토리지?
웹 스토리지는 웹 브라우저 내에서 데이터를 임시적으로 또는 영구적으로 저장할 수 있는 매커니즘이다! 

웹 스토리지는 몇가지 특징을 가지고 있는데
1. 간단한 인터페이스: javascript를 사용하여 간단한 API를 제공한다.
2. 도메인 별로 관리 : 각 도메인은 자체의 웹 스토리지를 가지며, 다른 도메인과는 데이터를 공유할 수 없다. 보안과 개인 정보 보호를 강화하는데 도움이 된다.
3. 클라이언트 측 데이터 저장: 모든 데이터는 클라이언트 측에서 저장되므로 서버로의 요청없이도 브라우저에서 사용할 수 있다.
4. 비동기적 접근: 웹 스토리지 API는 비동기적으로 작동하며, 데이터를 읽고 쓰는 작업은 브라우저가 자체적으로 처리한다.

웹 스토리지에는 로컬스토리지(local storage) 와 세션스토리지(session storage) 라는 두가지 저장 메커니즘을 가지고 있다. 그렇다면 이것들은 무엇일까 ?

### 로컬스토리지(local storage) vs 세션스토리지(session storage)

**로컬 스토리지** 

로컬 스토리지는 영구적인 데이터 저장소,  사용자의 브라우저에 데이터를 저장한다. 데이터들은 도메인 레벨에서 관리되며, 도메인이 같은 페이지들끼리는 모두 같은 로컬 스토리지를 공유한다. 저장형식은 map과 유사하게 저장이 된다.

***장점***
- 영구적으로 데이터를 저장할 수 있어 브라우저를 닫았다가 다시 열어도 데이터가 유지된다.
- 로컬 스토리지에 저장된 데이터는 사용자가 명시적으로 삭제하지 않는 이상 계속 유지가 된다.

***단점***

- 로컬 스토리지에 저장된 데이터는 삭제하기 전까지는 계속 유지되므로, 보안에 취약할 수 있다.
- 대용량의 데이터를 저장하면 브라우저의 성능에 영향을 줄 수 있다.


**세션 스토리지**

세션 스토리지는 세션(session) 동안만 데이터를 유지하는 저장소이다.
세션이 끝나면 데이터가 삭제된다. 브라우저를 닫았다가 다시 열면 새로운 세션이 시작되므로, 세션 스토리지에 저장된 데이터도 사라진다.

***장점***

- 세션 동안만 데이터를 유지하기 때문에 보안이 더 강력하다.
- 브라우저를 닫으면 데이터가 자동으로 삭제되므로, 개인 정보 보호에 더 유리하다.


***단점***

- 세션 동안에만 데이터를 유지하기 때문에, 브라우저를 닫았다가 다시 열면 데이터가 사라져 사용자 경험에 영향을 줄 수 있다.
- 사용자가 다른 페이지로 이동하면 세션도 종료되어 데이터가 삭제된다.

> 그렇다면 언제 뭘 써야해?!
> 로컬 스토리지: 사용자 설정, 캐싱된 데이터, 로그인 정보등을 저장할때 유용!
> 세션 스토리지: 세션 중에만 필요한 임시 데이터를 저장하는데 유용!

저장용량은 둘다 거의 비슷한데 브라우저에 따라서 5MB~10MB까지 저장 가능하다.


이제 로컬 스토리지와 세션스토리지를 알아보았고 직접 구현을 해보자!


### 구현

전에 기능을 구현한 부분들을 전체적으로 바꾸고 함수를 나누었다!

```js
function addBtn() {
  let toInput = document.getElementById("toinput");
  if (!toInput.value) {
    alert("Plz input value");
    return;
  }

  appendTodoItem(toInput.value);
  updateLocalStorage();
  toInput.value = "";
}

```

먼저 트리거 역활을 하는 addbtn이 호출되면 appendTodoItem에 input값을 담아준 후 리스트를 생성해준다!
그후에 로컬 스토리지에 값을 저장하는 updateLocalStorage를 호출!

```js
function appendTodoItem(todoItem) {
  let doList = document.getElementById("tolist");
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
    updateLocalStorage();
  });

  let textBlock = document.createElement("span");
  textBlock.className = "text-block";
  textBlock.innerHTML = " &nbsp;&nbsp;" + todoItem;

  let changeBtn = document.createElement("span");
  changeBtn.className = "changeBtn";
  changeBtn.innerHTML = " change";
  changeBtn.addEventListener("click", function () {
    enableEditing(textBlock);
  });

  let deleteBtn = document.createElement("span");
  deleteBtn.className = "deleteBtn";
  deleteBtn.innerHTML = " ❌";
  deleteBtn.addEventListener("click", function () {
    toItem.remove();
    updateLocalStorage();
  });
  toItem.appendChild(toCheckBox);
  toItem.appendChild(deleteBtn);
  toItem.appendChild(changeBtn);
  toItem.appendChild(textBlock);
  doList.appendChild(toItem);
}
 ```

달라진 부분은 addEventListener부분에서 계속해서 updateLocalStorage를 불러주는 부분이 있다. 그 이유는 어떠한 변화가 나의 todoList에서 일어나게 된다면 그 즉시 로컬 스토리지에 현재 나의 todolist상태를 덮어 써버리는것이다. (끝나고 난 뒤에 보니 무언가 비효율적인것같음...)

```js
function updateLocalStorage() {
  let todoItemList = [];
  let doList = document.getElementById("tolist");
  let todoListItems = doList.getElementsByClassName("text-block");

  for (let item of todoListItems) {
    todoItemList.push(item.innerHTML);
  }
  saveTodoList(todoItemList);
}

function saveTodoList(items) {
  localStorage.setItem("todoItems", JSON.stringify(items));
}

```

먼저 리스트를 생성해 준다음 우리가 계속해서 추가해주던 tolist를 가져와 text 부분만 리스트에 담아준 후에 그 담아준 리스트를 json형식으로 로컬스토리지에 저장해버린다.


[![스크린샷 2024-01-26 오후 7 49 36](https://private-user-images.githubusercontent.com/90916425/299969340-dd689d88-a612-470d-9eb6-f8e62637904d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDYyNjc4NTIsIm5iZiI6MTcwNjI2NzU1MiwicGF0aCI6Ii85MDkxNjQyNS8yOTk5NjkzNDAtZGQ2ODlkODgtYTYxMi00NzBkLTllYjYtZjhlNjI2Mzc5MDRkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI2VDExMTIzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTQ5MTRiODc1MjIzMzAwYzFhMjNhZmM3ODhhNDc5NmFhYzliMTU2Y2NlNTc0MzdjZmQzNmUxM2RjY2FiZTQ1ZTcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.ag5RZ_mHHbYJGKIl7HUJRgi39bjasvpo23xXledjKiE)](https://private-user-images.githubusercontent.com/90916425/299969340-dd689d88-a612-470d-9eb6-f8e62637904d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDYyNjc4NTIsIm5iZiI6MTcwNjI2NzU1MiwicGF0aCI6Ii85MDkxNjQyNS8yOTk5NjkzNDAtZGQ2ODlkODgtYTYxMi00NzBkLTllYjYtZjhlNjI2Mzc5MDRkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI2VDExMTIzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTQ5MTRiODc1MjIzMzAwYzFhMjNhZmM3ODhhNDc5NmFhYzliMTU2Y2NlNTc0MzdjZmQzNmUxM2RjY2FiZTQ1ZTcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.ag5RZ_mHHbYJGKIl7HUJRgi39bjasvpo23xXledjKiE)

위에 보는것 처럼 관리자 도구의 애플리케이션 > local storage 부분에 들어가면 나온다. 아무것도 없을 경우에 빈 배열을 저장하게 된다.

[![스크린샷 2024-01-26 오후 7 51 26](https://private-user-images.githubusercontent.com/90916425/299969356-1a8be4d2-0309-4fbd-90a0-6a2f61619648.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDYyNjc4NTIsIm5iZiI6MTcwNjI2NzU1MiwicGF0aCI6Ii85MDkxNjQyNS8yOTk5NjkzNTYtMWE4YmU0ZDItMDMwOS00ZmJkLTkwYTAtNmEyZjYxNjE5NjQ4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI2VDExMTIzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc0YzI2ZGY2YzBlN2MzNGQwZWNmZmQ5NjQ5ZDBkNGIxYmRiOTY4YWI0MmE3MWJhOGVmYWFhZGEwN2U2ZjMzNmQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.ggxGrgv-OGgjvyftMvY-2FApuRtDsHP2I_1xi-Jf7qc)](https://private-user-images.githubusercontent.com/90916425/299969356-1a8be4d2-0309-4fbd-90a0-6a2f61619648.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDYyNjc4NTIsIm5iZiI6MTcwNjI2NzU1MiwicGF0aCI6Ii85MDkxNjQyNS8yOTk5NjkzNTYtMWE4YmU0ZDItMDMwOS00ZmJkLTkwYTAtNmEyZjYxNjE5NjQ4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI2VDExMTIzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc0YzI2ZGY2YzBlN2MzNGQwZWNmZmQ5NjQ5ZDBkNGIxYmRiOTY4YWI0MmE3MWJhOGVmYWFhZGEwN2U2ZjMzNmQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.ggxGrgv-OGgjvyftMvY-2FApuRtDsHP2I_1xi-Jf7qc)

값을 넣었을때는 이런식으로 저장이 된다 공백문자를 넣어주는것을 볼수있는데 이것은 나의 잘못이다...ㅠ 공백문자를 넣어주는 이유는 css를 잘못넣어서 change와 떨어뜨리기위해 무식하게 넣은것이다... 리팩토링 예정이다

아무튼 이런식으로 로컬 스토리지에 저장이 되어있는데 이러면 도메인별로 로컬스토리지에 저장이 되었기때문에 로컬스토리지 저장소만 삭제하지 않는다면 언제든 다시 접속하게 된다면 저장해놓았던 로컬스토리지를 가져올수 있다.

```js
document.addEventListener("DOMContentLoaded", function () {
  loadTotoList();
});

function loadTotoList() {
  let storedItems = JSON.parse(localStorage.getItem("todoItems")) || [];
  storedItems.forEach(function (item) {
    appendTodoItem(item);
  });
}
```

이 부분이 처음에 화면이 렌더링 되기전 js파일을 html에 적용하는 순간에 먼저 실행된다. Dom의 내용이 불러져 올때 로컬스토리지에 json형식으로 저장되어있던 값들을 직접적으로 불러오는 부분이다.


### 아쉬운 점

위에서도 언급했던 저장하는 부분에서 살짝 아쉬운 부분이 있는데 나는 무식하게 변경이 일어날때마다 모든 정보를 그대로 로컬스토리지에 덮어쓰기로 저장하는 부분이 무언가 비효율적이고 저 map형식으로 저장되었던 값의 인덱스로 접근해서 변경되는 부분만 수정한다면 더 효율적일것 같다. 

또한 css가 부족해서 그런지 layout과 배치부분이 좀 부족해서 공백문자로 공백을 생성해버리는 말도안되는 일을 해버렸다.

당장 css공부부터 시작해야겠다..😢 


---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)