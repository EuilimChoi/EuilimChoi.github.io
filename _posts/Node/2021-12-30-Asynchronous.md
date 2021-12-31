---
layout: single
title: "node js 비동기"
categories: "node"
tag: ["server", "Javascript", "Node"]
toc: true

sidebar:
  nav: "sidebar_main"
tagline: " "
header:
  overlay_image: /assets/images/DSCF3606.JPG
  overlay_filter: 0.5
---

클라이언트와 서버와 통신을 위해 서버 또는 클라이언트에서는 주로 비동기 방식으로 서로 정보를 요청한다.
하지만 서버와 클라이언트와 통신을 하는 과정은 실제로 컴퓨터 내부에서 프로세스를 처리하는 것보다 시간이 걸리기에 우리는 주로 비동기 (Asynchronous) 방식으로 데이터 요청을 보내고 데이터가 도착할떄 까지할 수 있는 다른 일을 먼저 처리해야 한다.

## async/await

왜 callback, promise로도 비동기 처리가 가능한데, async/await까지 알아야 할까

가장 큰 이유는 여전히 Promise도 가독성이 썩 좋지 않고, async/await 코드가 깔끔하고 가독성이 좋아야 눈으로 로직 파악이 가능하고 디버깅이 쉬워지기 때문이다. 그리고 또 async/await로 코드의 양도 줄일 수 있다.

간단하게 사용법 예시는 아래와 같다.

```javascript
function workP(sec) {
  // promise 로 구현한 함수
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(new Date().toISOString());
    }, sec * 1000);
  });
}

function justFunc() {
  // 일반 함수
  return "just Function";
}

async function asyncFunc() {
  //async로 구현한 함수
  return "async Fucntion";
}

console.log(justFunc());
console.log(asyncFunc());
console.log(workP());
```

일반 함수인 justFunc()은 리턴으로 준 문자열 'jusf function'이 그대로 리턴되지만,
asyncFunc()과 workP()는 둘 다 Promise 객체를 리턴된다.
async는 Promise객체를 리턴하기 때문이다.

## await 적용하기

```javascript
function workP(sec) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("workP function");
    }, sec * 1000);
  });
}
async function asyncFunc() {
  const result_workP = await workP(3);
  console.log(result_workP);
  return "async function";
}

asyncFunc().then((result) => {
  console.log(result);
});
```

이렇게 하면 awati를 붙인 함수의 실행이 끝나는 동안 기다리게 됨으로 다음 코드를 실행할 수 없다. 즉 서버에서 통신을 통해 무언가 받아오거나 보내야줘야 한다면 이러한 방식으로 코드를 작성할 수 있겠다.
또한 .then을 사용해 해당 정보를 받은 후 다음 로직으로 연결해 줘도 된다.
