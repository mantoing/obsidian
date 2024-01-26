---
날짜: '"2024-01-26"'
넘버: 
태그: javascript
출처: 
aliases:
---
### 날짜  2024-01-26 20:20

### 태그:

>[!메모]
>

### 원문
---

# 외부에 저장은 어떤식으로 해야하지?


> localstorage까지 저장을 하고 난 이후에 무언가 외부로 다운로드 하는건 어떨까 ? 라는 생각을 했다. 어떤식으로 저장하지?

처음에 생각한것은 txt파일 형식으로 외부에 저장하는것을 생각했는데 나중에 하고싶은 프로젝트가 canvas를 사용할것 같아서 canvas를 사용해서 저장하는 것을 해보고 싶었다.


### html2canvas

그래서 생각해 낸것이 이미지로 캡처해서 캡처한 이미지를 다운로드 하는것으로 결정했다. 그래서 찾아본결과

**html2canvas** 라는 javascript라이브러리를 발견했다.

이 라이브러리를 사용하면 웹 페이지의 특정 부분이나 전체 페이지를 이미지로 캡처하여 어떠한 처리를 할 수 있다.

**html2canvas** 

1. ***웹 페이지 스크린샷:*** 웹페이지의 특정 부분이나 전체 페이지를 스크린 샷으로 캡처할 수 있다.
2. ***이미지 생성:*** 캡처한 HTML요소를 기반으로 이미지를 생성할 수 있다.
3. ***동적 컨텐츠 처리:*** javascript로 생성된 동적 컨텐츠를 포함하여 웹페이지의 모든 콘텐츠를 캡처할 수 있다.

사용하는것은 매우 간단한데

1. ***라이브러리 로드*** 먼저 라이브러리를 웹페이지에 로드한다.
2. ***캡처 대상 선택*** 캡처할 HTML요소를 선책한다. 
3. ***캡처 및 처리*** 함수를 호출하여 선택한 요소를 캡처한다. 


### 구현

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>TodoList</title>
  </head>
  <body>
    <div class="allcontainer">
      <div class="contentcontainer">
        <span class="title">TodoList</span>
        <button onclick="saveBtn()" id="savebtn">Save</button>
        <div class="inputandlist">
          <div class="inputcontainer">
            <input id="toinput" type="text" placeholder="New Task" size="40" />
            <button onclick="addBtn()" class="btn">+</button>
          </div>
          <div class="ulcontainer">
            <ul id="tolist"></ul>
          </div>
        </div>
      </div>
    </div>
    <script src="todo.js"></script>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
  </body>
</html>
```

먼저 script태그를 이용해서 **html2canvas** 라이브러리를 로드한다.
그리고 save 버튼을 만들어 준 후에 `onClick` 이벤트로 save할수있게 해준다

```js
function saveBtn() {
  let contentContainer = document.querySelector(".contentcontainer");
  let filename = "contentcontainer_capture";

  convertToPNG(contentContainer, filename);
}
```

먼저 `querySelector` 를 사용해서 캡처할 영역을 지정해주었다. (contentcontainer)

그 후에 정해진 파일이름으로 convertToPNG함수에 넣어준다.

```js
function convertToPNG(element, filename) {
  html2canvas(element).then(function (canvas) {
    let link = document.createElement("a");
    link.href = canvas.toDataURL("image/png");
    link.download = filename + ".png";
    link.click();
  });
}
```

이제 여기서 **html2canvas** 를 사용하게 되는데 
함수의 동작을 설명하자면 다음과 같다.

1. `html2canvas(element)`을 호출하여 주어진 HTML 요소(element)를 캡처한다. 이 때, html2canvas는 Promise 객체를 반환한다.
    
2. 캡처가 완료되면 `then` 함수가 실행된다. 여기서 캡처된 내용은 캔버스(canvas)에 그려진다.
    
3. `document.createElement("a")`를 사용하여 `<a>` 요소인 링크를 생성한다.
    
4. 생성한 링크의 `href` 속성에는 캔버스(canvas)의 데이터 URL(data URL)을 설정한다. `canvas.toDataURL("image/png")`을 호출하여 PNG 형식의 데이터 URL을 생성한다.
    
5. 생성한 링크의 `download` 속성에는 다운로드될 파일의 이름을 설정한다. 여기서는 함수의 두 번째 파라미터인 `filename`에 ".png" 확장자를 추가하여 설정한다.
    
6. `link.click()`을 호출하여 링크를 클릭하면 파일 다운로드가 자동으로 시작된다.

### 회고

처음에 구상할때는 상당히 어렵겠구나 라고 겁을 잔뜩 먹었었는데 js가 엄청나다는걸 다시 한번 느꼈다 ㅋㅋ **html2canvas** 라이브러리 사용과 js에서 canvas기능이 있다는것을 새롭게 알수 있었다.

---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)