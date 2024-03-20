# 이벤트 전파

특정 이벤트가 발생하면 어떤 순서로 발생할지 정하는 것을 이벤트 전파라고 한다.

이벤트가 발생하였을 때, 브라우저가 이벤트 리스너를 실행시킬 대상 요소를 결정하는 과정을 의미한다.

## 이벤트 전파 방식

[리액트에서 일어나는 이벤트 전파](https://codesandbox.io/p/sandbox/react-event-propagation-01-hslto6?file=%2Fsrc%2Findex.js%3A6%2C1&fontsize=14&hidenavigation=1&theme=dark)

### 이벤트 버블링

이벤트 버블링은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미한다.

<img src="../Images/Event Bubbling/event-bubbling.png">

### 이벤트 캡쳐링

이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식.

즉, 상위 요소에서 아래 요소로 전달되어 가는 특성을 의미한다.

<img src="../Images/Event Bubbling/event-capturing.png">

### 이벤트 전파 중지

이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해하는데 아래의 API는 해당 이벤트가 전파되는 것을 막는 기능을 한다.

이벤트 캡쳐의 경우에도 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않게 된다.

[stopPropagation](https://www.tcpschool.com/react/react_event_propagation)

```javascript
function logEvent(event) {
  event.stopPropagation();
}
```

```javascript
// 이벤트 버블링 예제
divs.forEach(function (div) {
  div.addEventListener('click', logEvent);
});

function logEvent(event) {
  event.stopPropagation();
  console.log(event.currentTarget.className); // three
}
```

```javascript
// 이벤트 캡쳐 예제
divs.forEach(function (div) {
  div.addEventListener('click', logEvent, {
    capture: true, // default 값은 false입니다.
  });
});

function logEvent(event) {
  event.stopPropagation();
  console.log(event.currentTarget.className); // one
}
```

## 이벤트 위임

이벤트 버블링과 캡쳐는 사실 이벤트 위임을 위한 선수 지식이라고 해도 과언이 아니다.

이벤트 위임을 한 문장으로 요약해보면 `하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식` 이라 할 수 있다.

### 개념

이벤트 위임은 비슷한 방식으로 여러 요소를 다뤄야 할 때 사용된다.

이벤트 위임을 사용하면 요소마다 핸들러를 할당하지 않고, 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도 여러 요소를 한꺼번에 다룰 수 있으며, 공통 조상에 할당한 핸들러에서 `event.target` 을 이용하면 실제 어디서 이벤트가 발생했는지 알 수 있어, 이를 이용해 이벤트를 핸들링을 할 수 있다.
