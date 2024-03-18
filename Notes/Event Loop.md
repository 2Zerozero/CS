# Event Loop 란?

이벤트 루프란 브라우저의 동작 타이밍을 제어하는 관리자 라고 할 수 있다.

## 브라우저 동작을 제어하는 관리자

`싱글 스레드` 인 자바스크립트의 작업을 멀티 스레드로 돌려 작업을 동시에 처리시키게 하거나, 동시에 여러 작업을 할 때 작업의 우선순위를 결정하기 위해 존재하는 것이 바로 `Event Loop` 이다.

### 동작 과정

자바스크립트의 `setTimeout` 이나 `fetch` 와 같은 비동기 자바스크립트 코드를 브라우저 Web APIs 에 맡기고, 백그라운드 작업이 끝난 결과를 콜백 함수 형태로 큐(Callback Queue)에 넣고 처리 준비가 되면 호출 스택(Call stack)에 넣어 마무리 작업을 진행한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbEeJN4%2FbtsabeBnUWX%2Fexb9jS9LXWWW7oM1Yk832K%2Fimg.png">

## 이벤트 기반(Event Driven) 프로그래밍

이벤트 루프를 이용한 프로그램 방식을 뜻하며, 프로그램의 흐름이 이벤트에 의해 결정되는 방식.

### 예시

사용자의 키보드 입력 또는 마우스 클릭과 같은 이벤트가 발생하면, 그에 맞는 콜백 함수가 실행된다.

자바스크립트에서는 대표적으로 `addEventListener(이벤트명, 콜백함수)` 가 존재한다.

```
<button id="myButton">Click me</button>
<script>
    const button = document.getElementById('myButton');
    button.addEventListener('click', function(event) {
        console.log('Button clicked!');
    });
</script>
```

버튼을 클릭하면, 이벤트가 발생하며 콘솔에 `Button clicked!` 가 출력된다.

## 장점

- 이벤트와 콜백 함수를 기반으로 처리하기 때문에 비동기 작업을 쉽게 처리할 수 있다.
- 멀티 스레드 언어에 비해 단순하고 직관적인 코드를 작성 가능하다.
- 브라우저와 같은 환경에서도 안정적인 실행이 가능해 사용자와의 상호작용을 높일 수 있다.
