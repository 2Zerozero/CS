# 마이크로태스크 큐와 매크로태스크 큐

자바스크립트의 이벤트 루프에서 비동기 작업을 처리하는 두 가지 방식이다.

## 작동방식

두 비동기 작업을 처리하는 방식들은 태스크 큐라는 저장공간에 저장된다.

발생한 순서대로 큐에 쌓이며 이벤트 루프에 의해 처리되기 시작하는데 여기서 마이크로태스크와 매크로태스크를 구분할 수 있다.

### 차이점

#### 마이크로태스크

- 마이크로태스크들은 실행하면서 새로운 마이크로태스크를 큐에 추가할 수도 있다.
- 작업의 우선순위가 높다.
- 프로미스(Promises) 핸들러 (then / catch / finally) + await,  Object.observe, process (MutationObserver 등)

#### 매크로태스크

- 매크로태스크가 추가한 매크로태스크는 다음 이벤트 루프가 실행될 때까지 실행되지 않는다.
- 작업의 우선순위가 낮다.
- DOM 이벤트 콜백, 타이머(setTimeout, setInterval), 스크립트 로딩, requestAnimationFrame 등

<img src="https://blog.kakaocdn.net/dn/wlO7d/btrG6XP5Nqh/qxKn1uQf4iXaI3JzuhBIx1/img.gif">

### 작동확인

```javascript
console.log('콜 스택!');
setTimeout(() => console.log('태스크 큐!'), 0);
Promise.resolve().then(() => console.log('마이크로태스크 큐!'));
```
