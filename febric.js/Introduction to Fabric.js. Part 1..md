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

매우 중요한 차이점에 주목해라. 
Fabric을 사용하면 콘텐츠를 **수정**하기 전에 더 이상 콘텐츠를 지울 필요가 없다. 
여전히 오브젝트로 작업하고 속성을 변경한 다음 캔버스를 다시 렌더링하여 "새로운 그림"을 얻을 수 있다.

**요약**

Fabric.js는 HTML5 캔버스에서 그래픽을 쉽게 다룰 수 있는 자바스크립트 라이브러리입니다. 기존의 캔버스 API보다 훨씬 직관적으로 작업할 수 있습니다. 예를 들어, 빨간색 직사각형을 그리는 코드는 다음과 같습니다:

```js
const canvas = new fabric.Canvas('myCanvas');
const rect = new fabric.Rect({
  left: 100,
  top: 100,
  width: 50,
  height: 50,
  fill: 'red'
});
canvas.add(rect);

```

이렇게 간단한 코드로도 원하는 그래픽을 만들 수 있습니다. Fabric.js를 사용하면 그래픽 작업이 훨씬 더 쉬워집니다.
### Objects

직사각형으로 작업하는 방법은 이미 `fabric.Rect` 생성자를 인스턴스화하여 살펴봤다. 
물론 Fabric은 원, 삼각형, 타원 등 다른 모든 기본 도형도 다룬다. 
이 모든 도형들은 `패브릭` "네임스페이스" 아래에 `fabric.Circle`, `fabric.Triangle`, `fabric.Ellipse` 등으로 노출된다.

원을 그리고 싶은가? 원 개체를 만들고 캔버스에 추가하기만 하면 된다. 다른 기본 도형도 마찬가지이다:
```js
var circle = new fabric.Circle({
  radius: 20, fill: 'green', left: 100, top: 100
});
var triangle = new fabric.Triangle({
  width: 20, height: 30, fill: 'blue', left: 50, top: 50
});

canvas.add(circle, triangle);
```

![[Pasted image 20240419181608.png]]

...그리고 100,100 위치에 녹색 원이 그려져 있고 50,50 위치에 파란색 삼각형이 그려져 있다.

#### Manipulating objects

직사각형, 원 등 그래픽 개체를 만드는 것은 시작에 불과하다. 
언젠가는 이러한 객체를 수정하고 싶을 때가 있을 것이다. 특정 동작을 통해 상태 변화를 트리거하거나 애니메이션을 재생해야 할 수도 있다. 또는 특정 마우스 상호작용에 따라 오브젝트 속성(색상, 불투명도, 크기, 위치)을 변경하고 싶을 수도 있다.  

Fabric은 캔버스 렌더링과 상태 관리를 처리한다. 
우리는 오브젝트 자체만 수정하면 된다.  

앞선 예제에서는 `set` 메서드와 `set({ 왼쪽: 20, 위쪽: 50 })` 호출이 어떻게 객체를 이전 위치에서 "이동"시키는지 보여줬다. 비슷한 방식으로 객체의 다른 속성도 변경할 수 있다. 하지만 이러한 속성은 무엇일까요?  

위치 -  **left**, **top**; 치수 — **width**, **height**; 렌더링 — **fill**, **opacity**, **stroke**, **strokeWidth**; 스케일링, 회전 — **scaleX**, **scaleY**, **angle**; 심지어 뒤집기 — **flipX**, **flipY** 기울이기 **skewX**, **skewY**

Fabric에서 뒤집힌 오브젝트를 만드는 것은 flip 속성을 `true로` 설정하는 것만큼이나 쉽다.  

`get` 메서드를 통해 이러한 프로퍼티를 읽고 `set` 메서드를 통해 설정할 수 있다. 
빨간색 사각형의 프로퍼티 중 일부를 변경해 보겠다:

```js
var canvas = new fabric.Canvas('c');
...
canvas.add(rect);

rect.set('fill', 'red');
rect.set({ strokeWidth: 5, stroke: 'rgba(100,200,200,0.5)' });
rect.set('angle', 15).set('flipY', true);
```

![[Pasted image 20240419182046.png]]

먼저 "채우기" 값을 "빨간색"으로 설정하여 기본적으로 객체를 빨간색으로 만든다. 다음 문은 **strokeWidth** 및 **stroke** 값을 모두 설정하여 직사각형에 옅은 녹색의 5px 획을 지정한다. 마지막으로 **angle** 및 **flipY** 속성을 변경한다. 세 문장이 각각 약간 다른 구문을 사용했음을 알 수 있다.  

이는 `집합이` 범용적인 메서드라는 것을 보여준다. 아마 자주 사용하게 될 것이므로 가능한 한 편리하게 만들었다.  

세터에 대해서는 살펴봤는데, 게터는 어떤가? 
물론 일반적인 `get` 메서드도 있지만 특정 `get*` 메서드도 많이 있다. 객체의 "width" 값을 읽으려면 `get('width')` 또는 `getWidth(`)를 사용하면 된다. **scaleX** 값을 가져오려면 `get('scaleX')` 또는 `getScaleX()` 등을 사용한다. 각 "공용" 객체 프로퍼티("획", "획폭", "각도" 등)에 대해 `getWidth` 또는 `getScaleX와` 같은 메서드가 있다.  

이전 예제에서 객체가 방금 `set` 메서드에서 사용한 것과 동일한 구성 해시로 생성되었음을 알 수 있다. **완전히 동 일하기** 때문이다. 객체를 생성할 때 "구성"하거나 그 후에 `set` 메서드를 사용할 수 있다:

```js
var rect = new fabric.Rect({ width: 10, height: 20, fill: '#f55', opacity: 0.7 });

// or functionally identical

var rect = new fabric.Rect();
rect.set({ width: 10, height: 20, fill: '#f55', opacity: 0.7 });
```

#### Default options

이 시점에서 '구성' 객체를 전달하지 않고 객체를 만들면 어떻게 되는지 궁금할 수 있다. 
여전히 해당 프로퍼티가 있을까?

물론이다. Fabric의 객체에는 항상 기본 속성 세트가 있다. 생성 중에 생략하면 이 기본 속성 세트가 객체에 "주어진" 것이다. 직접 확인해 보자:

```js
var rect = new fabric.Rect(); // notice no options passed in

rect.get('width'); // 0
rect.get('height'); // 0

rect.get('left'); // 0
rect.get('top'); // 0

rect.get('fill'); // rgb(0,0,0)
rect.get('stroke'); // null

rect.get('opacity'); // 1
```

직사각형에는 기본 속성 세트가 있다. 
사각형의 위치는 0,0이고, 검은색이며, 완전히 불투명하고, 획과 **치수가** 없다(너비와 높이는 0입니다). 
치수가 없으므로 캔버스에서 볼 수 없다. 그러나 너비/높이에 양수 값을 부여하면 캔버스의 왼쪽/상단 모서리에 검은색 사각형이 확실히 나타난다.

![[Pasted image 20240419182625.png]]

#### Hierarchy and Inheritance

패브릭 객체는 서로 독립적으로 존재하는 것이 아니다. 매우 정밀한 계층 구조를 형성한다.  

대부분의 오브젝트는 루트 `fabric.Object에서` 상속받는다. `fabric.Object는` 2차원 캔버스 평면에 배치된 2차원 도형을 나타낸다. 왼쪽/위쪽 및 너비/높이 속성과 기타 여러 그래픽 특성을 가진 엔티티이다. 채우기, 획, 각도, 불투명도, 뒤집기 등 개체에서 보았던 속성은 `fabric.Object에서` 상속하는 모든 Fabric 개체에 공통적으로 적용된다.  

이러한 상속을 통해 `fabric.Object에서` 메서드를 정의하고 모든 하위 "클래스"에서 공유할 수 있다. 예를 들어 모든 객체에서 `getAngleInRadians` 메서드를 사용하려면 `fabric.Object.prototype에` 메서드를 생성하기만 하면 된다:

```js
fabric.Object.prototype.getAngleInRadians = function() {
  return this.get('angle') / 180 * Math.PI;
};

var rect = new fabric.Rect({ angle: 45 });
rect.getAngleInRadians(); // 0.785...

var circle = new fabric.Circle({ angle: 30, radius: 10 });
circle.getAngleInRadians(); // 0.523...

circle instanceof fabric.Circle; // true
circle instanceof fabric.Object; // true
```

보시다시피 메서드는 모든 인스턴스에서 즉시 사용할 수 있다.  

자식 '클래스'는 `fabric.Object에서` 상속하지만, 종종 자체 메서드와 프로퍼티를 정의하기도 한다. 
예를 들어 `fabric.Circle에는` "radius" 프로퍼티가 필요하다. 그리고 잠시 후에 살펴볼 `fabric.Image에는` 이미지 인스턴스의 출처가 되는 HTML <img> 요소에 액세스/설정하기 위한 `getElement/setElement` 메서드가 있어야 한다.  

프로토타입으로 작업하여 사용자 정의 렌더링 및 동작을 얻는 것은 고급 프로젝트에서 매우 일반적이다.

### Canvas

이제 객체를 자세히 다루어 보았으니, 켄버스로 다시 돌아가 보자

모든 Febric의 예제에서 캔버스 객체를 생성했을때 가장먼저 볼수있는 것은 `new fabric.Canvas('...')` 이다.  **febric.canvas** 는 <canvas> 요소로 감싸져있고 보호하면서 모든 페브릭 객체를 합리적으로 관리한다.

---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)