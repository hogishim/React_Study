# React Introduction

### ⭐React: single page application을 만드는 경우에 유리한 자바스크립트 라이브러리

### 🧩React 프로젝트 생성: Node.js설치, VS Code설치

- 작업할 폴더를 하나 만들고, 폴더에서 CMD창을 열고, `npx create-react-app [프로젝트명]`을 입력해주면 알아서 필요한 내용들이 자동으로 생성된다

![Untitled](React%20Introduction/Untitled.png)

- `node_modules`폴더에는 프로젝트 구동에 필요한 라이브러리 코드가 들어있다
- `public`파일은 static변수를 모아놓는 곳이다
- `src`는 소스파일이 들어있는 폴더이다
    
    ![Untitled](React%20Introduction/Untitled%201.png)
    
    - App.js의 내용이 main으로 들어가는 code로, App.js의 내용이 index.js를 통해서 main HTML파일인 index.html에 표시되게 된다

### 🧩React에서 사용하는 JSX기본 문법

```jsx
...

function App(){

  var data = 'red';
  return (
    <div className="App">
      <div className="black-nav">
        <div>개발 blog</div>
        <div className=data>{data}</div>
				<div style={ {color : 'blue', fontSize : '30px'} }> 글씨 </div>
      </div>
    </div>
  )
}

...
```

- App에서 HTML요소로 보여질 내용은 return에 들어간다
- return에 들어가는 내용은 기본적으로는 HTML의 코드와 비슷하지만 HTML이 아니기 때문에 몇가지 차이가 존재한다
- HTML과는 달리, `<p class=”…”>`형식으로 class명을 지정하는 것이 아니라, `<p className=”…”>`으로 지정한다. JSX에서는 class를 코드상 객체를 만들어 줄때 이용한다
- `var, let, const`같은 변수를 HTML코드에서 가져와서 쓸 수 있고, Class명이나 속성에도 적용할 수 있다
- 스타일을 적용할때, `style=”color: red”`와 같이 하는 것이 아니라, `{ { … } }`안에 스타일을 넣어준다. 또한, `font-size`가 아니라, `fontSize`로 써줘야 한다

```jsx
return (
	
	/*
	<div>...</div>
	<div>...</div>
	*/

	<>
		<div>...</div>
		<div>...</div>
		<img />
	</>
	
)
```

- return구문 안에는 반드시 하나의 태그가 감싸고 있어야 한다. 주석처리된 코드처럼 작성하는 경우, 가장 바깥에 있는 태그가 두개기 때문에 오류가 난다. 따라서 `<div>`를 하나 더 써서 바깥을 감싸거나, `<> … </>`로 코드  밖을 감싸면 된다
- 기존 HTML에서는 닫는 태그가 없더라도 무조건 `<img></img>`와 같이 닫아주거나, `<img />`와 같은 형식으로 써줘야 한다

### 🧩onClick event: 클릭된 경우 이벤트 처리

```jsx
<div onClick={ setCount }>
<div onClick={ function(){ ... } }> 
<div onClick={ () => { ... } }>
```

- click했을때 동작을  `onClick`으로 구현할 수 있다
- 클릭 된 경우에는 별도의 함수를 실행하거나, 함수를 안에 선언해주고 자바스크립트를 통해 동적으로 처리해줄 수 있다
- 클릭하면 state 값을 업데이트하는 것과 같은 코드를 짤 수 있다

```jsx
...

function App (){

  let [modal, setModal] = useState(false);

  return (

    <div>
      ...
      <button onClick={ ()=>{ setModal(true) } }> {글제목[0]} </button>
      { 
         modal == true ? <Modal></Modal> : null
      }
			...
    </div>
  )
}
```

- jsx의 return부분에 `if-else`를 쓰는 것은 불가능하다. 삼항연산자로 조건문으로 활용한다
- 버튼을 클릭하면 modal창이 나오도록 하는 코드이다. 버튼을 누르면 setModal로 state값이 변경되고, 다시 삼항연산자를 거쳐서 <Modal>을 보여주게 된다
- 자바스크립트와는 달리, react는 HTML을 직접적으로 조작하지 않는다

### 🧩map: for대신에 이용하는 반복문

```jsx
[2,3,4].map(function(){
	...
});

var arr = [2,3,4];
arr.map(function(a, i){
  console.log(a);
	console.log(i);
});

var a1 = [2,3,4];
var a2 = a1.map(function(a){
  return a * 10
});
```

- jsx return구문 내부에는 for문 사용하는 것이 불가능하다
- 길이만큼 반복적으로 실행하는데, function의 전달 인자로는 데이터가 들어가게 된다. 따라서 function의 첫 전달 인자로 변수명을 선언하고, 변수명을 log에 출력하면 데이터가 그대로 나오게 된다
- 두번째 인자는 index로, log를 출력하면 i가 0, 1, 2…와 같이 나오게 된다
- 새로운 자료형을 만들고, 그 안에 return 값을 통해서 새로운 데이터를 넣어줄 수 있다

```jsx
{ 
...
    [1,2,3].map(function(){
       return ( <div key={i}>안녕</div> )
     }) 
...
}

**{ 
        글제목.map(function(a, i){
          return (
          <div className="list">
            <h4>{ a }</h4>
            <p>{date[i]}</p>
          </div> )
        }) 
      }
...**
```

- for문 대신 map을 이용해서 return안에 반복하여 만들고 싶은 코드를 넣어주면 된다
- 첫 코드와 같이 만드는 경우, 오류가 났다고 인식될 수 있는데, 이는 `<div>`가 여러개인데 구분이 안가기 때문에 생기는 것으로 `key={i}`로 여러 tag를 구분시켜 줄 수 있다
- function의 전달인자로 들어오는 `a, i`를 이용하여 원하는 내용을 출력한다
- 항상 주의해야 하는 것은, script내용이 나오기 위해서는 {…}안에 내용을 작성해야 한다
- `map`을 쓰지 않고 for문을 사용하고 싶은 경우, return의 윗부분에 array에 for문을 포함한 코드를 작성하고, array를 HTML문서상에서 불러와서 사용할 수 있다

### 🧩input 처리: 사용자의 input처리

```jsx
...
function App (){

  let [입력값, 입력값변경] = useState('');
  return (
    <input onChange={(e)=>{ 

      입력값변경(e.target.value) 
      console.log(입력값)

    }} />
  )
}
...
```

- 사용자로 부터 input을 받을 수 있고, input type은 여러가지가 있다
- 입력값이 변경되는 경우는 `onChange, onInput`같은 것으로 handling할 수 있다
- onChange의 전달 인자로 이벤트에 대한 여러가지 유용한 기능을 이용할 수 있다
    - `e.target`: 현재 이벤트가 발생한 부분을 알려준다
    - `e.preventDefault()`: 이벤트 동작을 막아줄 수 있다
    - `e.stopPropagation()`: 이벤트가 상위 요소에도 적용되는 이벤트 버블링을 막아줄 수 있다
    - `e.target.value`: 이벤트에서 나오는 값을 저장해준다. 여기서 `e.target.value`로 나오는 값은 사용자가 input에 입력한 값으로 사용자가 입력한 값을 log로 출력해준다

### 🧩이미지 넣기

```css
.main-bg {
  height : 300px;
  background-image : url('./bg.png');
  background-size : cover;
  background-position : center;
}
```

- CSS파일에서 원하는 이미지의 경로를 `url(…)`안에 넣으면 이미지가 파일에 나오게 된다

```jsx
import bg from './bg.png'

function App(){
  return (
    <div>
      <div className="main-bg" style={{ backgroundImage : 'url(' + bg + ')' }}></div>
    </div>
  )
}
```

- CSS파일 말고 JS파일에서 직접 넣고 싶다면, import하는 과정이 필요하다
- 변수라는 것을 인식하기 위해서 `url( + 변수명 +)` 과 같이 써줘야 한다

```jsx
<img src={'https://codingapple1.github.io/shop/shoes' + props.i + '.jpg'} />
```

- 링크 중간에 변수를 추가하고 싶은 경우, 링크를 `{…}`로 두르고, 중간에 `+…+`에 변수를 넣어주면 정상적으로 변수가 들어간 링크가 나온다