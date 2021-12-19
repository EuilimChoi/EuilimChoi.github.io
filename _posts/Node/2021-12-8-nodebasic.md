---
layout: single
title: "서버 Req, Res 등등"
categories: "node"
tag: ["server", "Javascript","Node"]
toc: true

sidebar:
  nav: "sidebar_main"
tagline: " "
header:
  overlay_image: /assets/images/DSCF3606.JPG
  overlay_filter: 0.5
---

대부분 클라이언트 (ex : 크롬) 같은 애들이 요청을 보내면 서버에서 요청한 데이터를 전달해주는 방식이다.

요청은 메소드는 크게 4가지가 있다.

- 기존 데이터를 조회하는 리퀘스트 - **GET**
- 새 데이터를 추가하는 리퀘스트 - **POST**
- 기존 데이터를 수정하는 리퀘스트 - **PUT**
- 기존 데이터를 삭제하는 리퀘스트 - **DELETE**

이러한 요청을 URL 형태로 보내주면 서버가 그걸 보고 작업한 데이터를 돌려준다. 어떤 리퀘스트에 대해 어떻게 반응할지는 서버 부분에서 보면되는 것이고 대충 아래와 같은 코드로 짜여진 서버가 있고 클라이언트의 요청을 받아 **대문자, 소문자로 바꿔주는 서버다.**

## Server code:

```javascript
const http = require("http");
const PORT = 5000;
const ip = "localhost";

const server = http.createServer((request, response) => {
  if (request.method === "POST") {
    if (request.url === "/lower") {
      let data = "";
      request.on("datacol", (chunk) => {
        data = data + chunk;
      });
      request.on("endevent", () => {
        data = data.toLowerCase();
        response.writeHead(200, defaultCorsHeader);
        response.end();
      });
    } else if (request.url === "/upper") {
      let data = "";
      request.on("data", (chunk) => {
        data = data + chunk;
      });
      request.on("end", () => {
        data = data.toUpperCase();
        response.writeHead(200, defaultCorsHeader);
        response.end(data);
      });
    } else {
      response.writeHead(404, defaultCorsHeader);
      response.end("");
    }
  }

  if (request.method === "OPTIONS") {
    response.writeHead(200, defaultCorsHeader);
    response.end("");
  }

  console.log(
    `http request method is ${request.method}, url is ${request.url}`
  );
});

server.listen(PORT, ip, () => {
  console.log(`http server listen on ${ip}:${PORT}`);
});

const defaultCorsHeader = {
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE, OPTIONS",
  "Access-Control-Allow-Headers": "Content-Type, Accept",
  "Access-Control-Max-Age": 10,
};
```

## Client code:

```javascript
class App {
  init() {
    document
      .querySelector("#to-upper-case")
      .addEventListener("click", this.toUpperCase.bind(this));
    document
      .querySelector("#to-lower-case")
      .addEventListener("click", this.toLowerCase.bind(this));
  }

  post(path, body) {
    fetch(`http://localhost:5000/${path}`, {
      method: "POST",
      body: JSON.stringify(body),
      headers: {
        "Content-Type": "application/json",
      },
    })
      .then((res) => res.json())
      .then((res) => {
        this.render(res);
      });
  }

  toLowerCase() {
    const text = document.querySelector(".input-text").value;
    this.post("lower", text);
  }

  toUpperCase() {
    const text = document.querySelector(".input-text").value;
    this.post("upper", text);
  }

  // 이부분이 응답을 받아서 보여주는 코드;
  render(response) {
    const resultWrapper = document.querySelector("#response-wrapper");
    document.querySelector(".input-text").value = "";
    resultWrapper.innerHTML = response;
  }
}

const app = new App();

app.init();
```

## HTML :

```html
<html>
  <head>
    <meta charset="utf-8" />
    <style>
      .input-text {
        resize: none;
        font-size: 1.5rem;
        width: 80%;
        height: 10ch;
        border: 1px solid #000;
      }
      #response-wrapper {
        width: 80%;
        font-size: 2rem;
        border: 1px solid #000;
        height: 10ch;
      }
      button {
        font-size: 2rem;
      }
    </style>
  </head>
  <body>
    <div id="root">
      <h2>요청</h2>
      <textarea
        placeholder="여기에 작성한 데이터를 서버로 보내면 응답으로 받을 수 있어야 합니다."
        class="input-text"
      ></textarea>
      <div>
        <button id="to-upper-case">toUpperCase</button>
        <button id="to-lower-case">toLowerCase</button>
      </div>
      <h2>응답</h2>
      <pre id="response-wrapper"></pre>
    </div>

    <script src="./App.js"></script>
  </body>
</html>
```

뭐 별거 없다 대충 아래와 같은 페이지에서 버튼을 누르면 그에 따른 값을 리턴해 주는 것

![image-20211210131938328](https://user-images.githubusercontent.com/91925895/145516732-e3d3cd4a-4453-46f7-9b02-9f1f97943734.png)
