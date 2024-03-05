# Node 서버와 연동

### 🖥️Node로 서버 구현

- Node.js를 설치한다
- 작업 폴더를 만들고 editor로 오픈한다
- server.js파일을 만든다

```jsx
const express = require('express');
const path = require('path');
const app = express();

app.listen(8080, function () {
  console.log('listening on 8080')
}); 
```

- server.js파일에 해당 내용을 입력한다

```
npm init -y
npm install express
```

- 터미널에서 다음 명령어를 통해 install한다
- 서버 미리보기는 nodemon을 통해서 가능하다

```
npm run build
```

- 리액트 개발이 끝난 경우에는 `npm run build`를 입력하면 react 완성본 HTML 파일인 index.html이 build폴더에 생성되고, 서버에 해당 파일을 보내주면 된다
- build는 react파일 수정할때마다 하는게 아니라, 배포하기 전 단게에서만 해주면 된다

server.js

```jsx
app.use(express.static(path.join(__dirname, 'react-project/build')));

app.get('/', function (req, rep) {
  rep.sendFile(path.join(__dirname, '/react-project/build/index.html'));
});
```

- `/`로 접속을 하면, 해당 경로에 있는 react의 index.html을 보여준다
- `localhost:8080`으로 접속하면 `localhost:3000`으로 접속했을때의 화면을 볼 수 있다

```jsx
app.get('*', function (req, rep) {
  rep.sendFile(path.join(__dirname, '/react-project/build/index.html'));
});
```

- URL에 `[localhost:8080/detail](http://localhost:8080/detail)` 이런식으로 입력하는 것은 서버측에서 routing을 해줄 수 없다. 따라서 다음과 같은 코드를 입력해주어야 리액트의 routing 처리가 가능하다
- `*`는 client가 URL에 아무거나 입력하면 라우팅을 해달라라는 것으로, 이렇게 입력해주면 react routing이 잘 되는데, 이 코드는 항상 가장 아래에 놓아야 한다

### Node의 DB 데이터 요청

- React는 보통 client-side rendering을 해준다
- 데이터를 front에서 받아오기 위해서는 GET 요청을 받을때 코드를 만들어주고, front에서 상품 목록을 보여주고 싶을때 서버 주소로 GET 요청을 날리면 원하는 데이터를 받아올 수 있다

```jsx
app.use(express.json());
var cors = require('cors');
app.use(cors());

```

- 이 코드를 넣고 시작하면 리액트와 nodejs 서버간 Ajax요청이 처리될 수 있다

```jsx
npm install cors
```

- CORS를 터미널에서 설치한다
- `express.json`은 유저가 보낸 array/object를 출력하기 위해 필요하다
- CORS는 다은 도메인 주소끼리 ajax 요청을 주고 받는 경우에 필요하다

```jsx
app.get('/product', function(req, rep){
	rep.json({name: 'baekjoon'})
};
```

- 해당 경로로 요청이 오면, reply는 `json()`안에 있는 내용으로 해준다

### 서브 디렉토리에 리액트 보여주고 싶은 경우

server.js

```jsx
app.use( '/', express.static( path.join(__dirname, 'public') ))
app.use( '/react', express.static( path.join(__dirname, 'react-project/build') ))

app.get('/', function(요청,응답){
  응답.sendFile( path.join(__dirname, 'public/main.html') )
}) 
app.get('/react', function(요청,응답){
  응답.sendFile( path.join(__dirname, 'react-project/build/index.html') )
})
```

- `/react`로 접속하면 리액트 HTML이, `/` 로 접속하면 public 폴더에 있는 그냥 main.html으로 setting된다

package.json

```jsx
{
  "homepage": "/react",
  "version": "0.1.0",
  ... 
} 
```

- 리액트 프로젝트 내의 homepage라는 항목을 발행을 원하는 sub directory명으로 입력해주면 된다
- 이렇게 하면 server.js에서 `/react` 접속하면 리액트 프로젝트가, `/` 접속하면 일반 HTML을 보여준다
- ~~사실 딱히 필요 없는 내용이다~~