---
title: ES6 이후 비동기식 프로그램의 제어 흐름 패턴
date: 2019-11-11 04:46:32
draft: true
category: 'Node.js'
---

3장에서 콜백을 사용하여 비동기 코드를 다루는 방법과 콜백헬에 대해 다루었다.
콜백은 비동기를 위해 사용하기 좋지만, 다른 대안도 많다.

다른 대안에는 Promise, Generator, async await가 있으며,
이 방식으로 비동기식 프로그램의 제어 흐름을 처리하는 방법을 단순화 할 수 있는 방법을 알아 볼 것이다.

## 4.1 Promise

#### Promise 란?

Promise는 함수가 Promise 객체를 반환할 수 있도록 하는 추상화이다.
Promise는 비동기 작업의 최종 결과를 나타낸다.
아래의 상태값이 있다.

- pending: 대기중 - 아직 비동기 작업이 완료되지 않았다.
- fulfilled: 이행됨 - 비동기 작업이 성공적으로 끝났다.
- rejected: 거부됨 - 작업이 실패하여 종료되었다.
- settled: 처리됨 - promise가 이행되거나 거부되면 처리됨으로 간주된다.

fulfilled 된 값이나, rejected 된 오류를 받으려면 Promise의 then() 메소드를 사용한다.
then() 메소드의 중요한 특성 중 하나는 동기식으로 다른 promise를 반환한다는 것이다.

```js
promise.then([onFulfilled], [onRejected])
```

노드 스터디이지만, 프론트앤드에서 익숙한 비동기 함수로 예를 들자면 아래와 같다.

```js
fetch(url)
  .then(function(response) {
    return response.json()
  })
  .then(function(result) {
    console.log('Request successful', result)
  })
  .catch(function(error) {
    console.log('Request failed', error)
  })
```

에러가 발생한 경우에는 then 메서드의 두 번째 인자인 콜백에서 처리 가능하지만,
catch 될 때까지 전체 체인에 오류를 자동으로 전파할 수 있으므로 체계적인 위의 구조를 추천한다.

promise의 반환값인 Promise 객체를 좀 더 자세히 보고 싶다면,
fetch 함수 하나의 결과만 console로 확인해보자!
(늘 동기로 반환하므로 pending만 확인할 수 있겠지만)

4.1.3 코드 주석
