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

```javascript
콜 스택!
마이크로태스크 큐!
태스크 큐!
```

## 🎁 필수로 알아야 할 이벤트 루프의 작동 방식과 태스크 큐들의 처리 방식

### 이벤트 루프의 동작 순서

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbwBXB5%2FbtrG7XCi7RX%2Fzda2mCgMntycvgkcqBspR1%2Fimg.png">

1. 호출 스택이 비었는지 지속적으로 확인.
2. 호출 스택이 비었다면 마이크로 태스크 큐를 확인하고 가장 오래된 태스크부터 꺼내서 호출 스택으로 전달.
   <br>
   마이크로태스크 큐가 없어질때까지 수행.

3. 모든 마이크로태스크 큐가 처리된 직후, 렌더링 작업이 필요하다면 추가적으로 랜더링 작업까지 수행.
4. 매크로태스크 큐를 확인.
5. 매크로태스크 큐에서 가장 오래된 태스크를 꺼내 호출 스택에 전달.
6. 다시 1번으로 돌아간다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdUGQSn%2FbtrG8HyZOut%2FrdgCkY6Rb2Hb8YVWTqNkmk%2Fimg.png">

1. 비동기 작업들이 큐에 쌓여있는 상황.
2. 이벤트 루프는 태스크들을 처리하기 위해 호출 스택이 비었는지 계속 확인하며 비어있다면, 가장 먼저 마이크로 태스크 큐에 쌓여있는 태스크들을 우선적으로 처리 후 매크로 태스크를 처리하기 전 UI 랜더링 작업이 필요하다면 먼저 실행 후 매크로태스크를 수행한다.
3. 매크로태스크를 처리하고 나서 다시 마이크로태스크 큐를 확인하고 나서 다시 확인해서 쌓여있다면 모두 처리하고 UI 랜더링이 필요하다면 실행 후 매크로태스크를 수행한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcPYlKh%2FbtrG7YgXvrU%2FFCRyPI2C1Rj2ssccBgwpik%2Fimg.png">

### ✏ 중요한건 작업 순서

매크로태스크 하나를 처리할 때마다 마이크로태스크 전부를 다 처리하고 랜더링을 실행 후 가장 마지막에 수행한다는 것.

따라서, 마이크로태스크가 모두 처리되기 전까지 UI 랜더링이나 네트워크 요청은 절대 일어나지 않는다.

```javascript
<script>
  console.log('시작'); setTimeout(()=> console.log('타이머')); // (A) Promise.resolve() .then(()=> console.log('프로미스
  1')) // (B) .then(()=> console.log('프로미스 2')); // (C) console.log('끝'); // (D)
</script>
```

```javascript
시작
끝
프로미스 1
프로미스 2
타이머
```
