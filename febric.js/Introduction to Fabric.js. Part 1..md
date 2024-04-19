---
날짜: '"2024-04-19"'
넘버: 
태그: febric
출처: febric공식문서
aliases:
---
### 날짜  2024-04-19 17:55

### 태그: #Febric 

> Febric.js 를위한 튜토리얼
>

### 원문
---

### 0.

오늘은 HTML5 캔버스로 손쉽게 작업할 수 있는 강력한 자바스크립트 라이브러리인 [Fabric.js에](http://fabricjs.com/) 대해 소개하고자 한다.. Fabric은 캔버스에 대한 누락된 객체 모델뿐만 아니라 SVG 파서, 상호 작용 레이어 및 기타 필수 도구 모음을 제공한다. MIT 라이선스를 받은 완전 오픈 소스 프로젝트로, 수년 동안 많은 기여를 해왔다.  

Fabric은 네이티브 캔버스 API 작업의 어려움을 발견한 후 2010년경에 시작되었다. 최초 작성자는 사용자가 직접 의류를 디자인할 수 있는 스타트업인 [printio.ru를](http://printio.ru/) 위한 대화형 디자인 에디터를 만들고 있었다. 당시에는 플래시 앱에만 존재하던 인터랙티브한 기능이었다. 지금도 Fabric을 통해 가능해진 것에 근접한 것은 거의 없다.  

자세히 살펴보겠다!

  
### 왜 Febric인가?

캔버스를 사용하면 요즘 웹에서 정말 멋진 그래픽을 만들 수 있다. 하지만 제공하는 API는 실망스러울 정도로 수준이 낮다. 단순히 캔버스에 몇 가지 기본 도형을 그리고 잊어버리고 싶을 때는 문제다. 하지만 어떤 종류의 상호작용이 필요하거나, 어느 시점에서 그림을 변경하거나, 더 복잡한 도형을 그려야 하는 순간 상황은 급격히 달라진다. Fabric은 이 문제를 해결하는 것을 목표로 한다. 
네이티브 캔버스 방식에서는 간단한 그래픽 명령만 실행할 수 있어 전체 캔버스 비트맵을 맹목적으로 수정할 수밖에 없다.
직사각형을 그리고 싶은가? 
fillRect(왼쪽, 위쪽, 너비, 높이)를 사용하면 된다. 
선을 그리고 싶은가? 
moveTo(왼쪽, 위 )와 lineTo(x, y)의 조합을 사용하면 된다. 마치 붓으로 캔버스에 그림을 그리는 것처럼, 제어할 수 있는 부분이 거의 없이 그 위에 점점 더 많은 페인트를 겹쳐서 칠하는 것과 같다. Fabric은 이러한 낮은 수준에서 작동하는 대신 네이티브 메서드 위에 간단하지만 강력한 객체 모델을 제공한다. 캔버스 상태와 렌더링을 처리하고 '객체'로 직접 작업할 수 있게 해준다. 
이 차이를 보여주는 간단한 예시를 살펴보겠습니다. 캔버스 어딘가에 빨간색 직사각형을 그리고 싶다고 가정해 보자. 네이티브 <캔버스> API를 사용하면 다음과 같이 할 수 있다.

``` js
// reference canvas element (with id="c")
var canvasEl = document.getElementById('c');

// get 2d context to draw on (the "bitmap" mentioned earlier)
var ctx = canvasEl.getContext('2d');

// set fill color of context
ctx.fillStyle = 'red';

// create rectangle at a 100,100 point, with 20x20 dimensions
ctx.fillRect(100, 100, 20, 20);
```

이제 febric에서 똑같이 작성을 해본다면 ? 

```js
// create a wrapper around native canvas element (with id="c")
var canvas = new fabric.Canvas('c');

// create a rectangle object
var rect = new fabric.Rect({
  left: 100,
  top: 100,
  fill: 'red',
  width: 20,
  height: 20
});

// "add" rectangle onto canvas
canvas.add(rect);
```

![[Pasted image 20240419180655.png]]

이 시점에서는 크기 차이가 거의 없으며 두 예제는 매우 유사합니다. 하지만 캔버스 작업 방식이 얼마나 다른지 이미 알 수 있다. 네이티브 메서드에서는 전체 캔버스 비트맵을 나타내는 객체인 **컨텍스트에** 대해 작업한다. 
Fabric에서는 **객체를** 인스턴스화하고, 속성을 변경하고, 캔버스에 추가하는 등 **객체에 대한** 작업을 수행한다. 
이러한 객체는 Fabric의 세계에서 1등 시민이라는 것을 알 수 있다.  

하지만 평범한 빨간색 직사각형을 렌더링하는 것은 너무 지루하다.
최소한 재미있는 무언가를 만들 수 있었을 텐데! 약간 회전하면 어떨까?  

45도 회전해 보자. 먼저 기본 <캔버스> 메서드를 사용한다:

```js
var canvasEl = document.getElementById('c');
var ctx = canvasEl.getContext('2d');
ctx.fillStyle = 'red';

**ctx.translate(100, 100);
ctx.rotate(Math.PI / 180 * 45);
ctx.fillRect(-10, -10, 20, 20);**
```

이제 Febric을 사용해보자:

```js
var canvas = new fabric.Canvas('c');

// create a rectangle with angle=45
var rect = new fabric.Rect({
  left: 100,
  top: 100,
  fill: 'red',
  width: 20,
  height: 20,
  **angle: 45**
});

canvas.add(rect);
```

![[Pasted image 20240419180936.png]]

여기서 무슨 일이 일어났을까?  

Fabric에서는 객체의 '각도' 값을 `45로` 변경하기만 하면 되었다. 하지만 네이티브 메서드를 사용하면 훨씬 더 "재미있게" 작업할 수 있다. 오브젝트를 조작할 수 없다는 점을 기억해라. 
대신 전체 캔버스 비트맵`(ctx.translate`, `ctx.rotate`)의 위치와 각도를 필요에 맞게 조정한다. 그런 다음 직사각형을 다시 그리되 비트맵의 오프셋(-10, -10)을 적절히 조절하여 100,100의 지점에서 렌더링되도록 한다. 보너스 연습으로 캔버스 비트맵을 회전할 때 각도를 라디안으로 변환해야 했다.  

이제 Fabric이 존재하는 이유와 얼마나 많은 저수준 상용구가 숨겨져 있는지 정확히 알기 시작했을 것이다.  

이제 캔버스 상태 추적이라는 또 다른 예를 살펴보겠다.  

이제 익숙한 빨간색 사각형을 캔버스의 약간 다른 위치로 이동하고 싶다고 가정해 보자. 객체를 조작할 수 없다면 어떻게 할 수 있을까? 캔버스 비트맵에서 다른 `fillRect를` 호출하면 될까?  

그렇지 않다. 다른 `fillRect` 명령을 호출하면 실제로 캔버스에 이미 그려진 사각형 위에 사각형이 그려집니다. 앞서 브러시로 그리는 것에 대해 언급했던 것을 기억하나? 이를 '이동'하려면 먼저 **이전에 그린 내용을 지운 다음** 새 위치에 직사각형을 그려야 한다.

```js
var canvasEl = document.getElementById('c');

...
ctx.strokRect(100, 100, 20, 20);
...

// erase entire canvas area
**ctx.clearRect(0, 0, canvasEl.width, canvasEl.height);
ctx.fillRect(20, 50, 20, 20);**
```

그럼 어떻게 febric으로 달성할수 있을까?

![[Pasted image 20240419181243.png]]



---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)