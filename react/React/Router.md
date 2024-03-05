# Router: 여러 페이지를 이동

### 🧩Routing: 페이지를 쉽게 나눌 수 있다

```jsx
import { BrowserRouter } from "react-router-dom";

...

<React.StrictMode>
      <BrowserRouter>
        <App />
      </BrowserRouter>
  </React.StrictMode>
```

- 먼저 terminal에서 `npm install react-router-dom`을 설치해준다
- 이후에 index.js파일로 가서 import해오고, `<App />`을 `<BrowserRouter>`태그로 감싸주면 된다

```jsx
import { Routes, Route, Link } from 'react-router-dom'

function App(){
  return (
		
		<Link to="/">Home</Link>
		<Link to="/detail">Details</Link>

	   ...
    <Routes>
			<Route path="/" element={ <div>메인페이지</div> } />
      <Route path="/detail" element={ <div>상세페이지</div> } />
      <Route path="/about" element={ <div>어바웃페이지</div> } />
			<Route path="*" element={ <div>없는페이지</div> } />
    </Routes>
  )
}
```

- `<Routes>`안에 `<Route>`가 페이지를 나누는 부분이다. `<Routes>` 밖에 있는 코드는 페이지가 바뀌더라도 그대로 유지된다
- `<Routes>`안에 있는 각각의 `<Route>`는 링크가 바뀜에 따라 element안에 있는 내용으로 바뀌게 된다
- `“/”`는 그냥 메인페이지, `“/details”`는 링크/details라는 링크로 접속하게 되는 경우에 나오는 내용이다
- 실질적으로 사용자가 직접 URL에서 링크를 입력해서 접속하는 경우는 드물다. `<a>`태그와 비슷하게, `<Link>`태그를 누르게 되면, `to=””`에 설정한 링크로 이동하게 된다
- path에 `*`을 넣으면, 위에 나와있는 path링크 외에 링크로 접속하게되는 경우, 해당 페이지가 나오게 된다

### useNavigate(): 페이지 이동 기능 만들기

```jsx
import { Routes, Route, Link, useNavigate, Outlet } from 'react-router-dom';

function App(){
  let navigate = useNavigate()
  
  return (
    ...
    <button onClick={()=>{ navigate('/detail') }}>이동버튼</button>
		...
  )
}
```

- 먼저 `useNavigate`를 사용하기 위해 import해온다
- 버튼 뿐 아니라, 다른 태그도 on click에 대한 처리로, 앞서 import한 `useNavigate`를 이용하여 `navigate( )`안에 `<Link to=” “>`안의 내용과 동일하게 동작하게 만들 수 있다

### nested Routes: /…/…형식의 url을 더 쉽게 handling

```jsx
<Route path="/about" element={ <About/> } >  
  <Route path="member" element={ <div>멤버들</div> } />
  <Route path="location" element={ <div>회사위치</div> } />
</Route>
```

- …`/about/member` url에 대한 route를 보여주기 위해서는 `path=”/about/member”`처럼 작성하지 않고, nested형식으로 만들어줄 수 있다
- 이렇게 만들면, `<About />`밑부분에 member의 내용이 표시되게 된다

About.js

```jsx
function About(){
  return (
    <div>
      <h4>about페이지</h4>
      <Outlet></Outlet>
    </div>
  )
}
```

- About에 `<Outlet>`을 추가해야만 `<About />`의 밑 부분에 member, location의 내용이 나오게 된다

### 상세페이지 여러개 추가하기

```jsx
<Route path="/detail/0" element={ <Detail shoes={shoes}/> }/>
<Route path="/detail/1" element={ <Detail shoes={shoes}/> }/>
<Route path="/detail/2" element={ <Detail shoes={shoes}/> }/>
...

<Route path="/detail/:id" element={ <Detail shoes={shoes}/> }/>
```

- 위 코드와 같이 링크마다 코드를 추가하는 것은 지나치게 코드가 길어진다
- `/detail/:id`는 detail뒤에 아무 내용이 오더라도 다 처리한다. 이 id를 이용하여, 각 id마다 다른 내용을 handling 해줄 수 있다

Detai.js

```jsx
import { useParams } from 'react-router-dom'

function Detail(){

let {id} = useParams();
  console.log(id)

...
```

- id를 `useParams`을 통해 받아오고, id값을 기반으로 다른 내용을 보여주도록 코드를 짤 수 있다
- 이러한 코드는, 사용자가 특정 상품을 선택했을때, 해당 상품의 id를 받아와서 서버에 데이터를 호출하게 되는 경우와 같을때 사용할 수 있다