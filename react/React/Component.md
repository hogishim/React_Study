# Component: 코드를 별도의 파일로 분리

### 🧩Component: HTML코드를 별도의 파일로 분리하여 작성하게 해준다

```jsx
function App (){
  return (
    <div>
      (생략)
      <Modal></Modal>
			<Modal/>
    </div>
  )
}

function Modal(){
  return (
    <div className="modal">
      <h4>제목</h4>
      <p>날짜</p>
      <p>상세내용</p>
    </div>
  )
}
```

- App안에서 return안에 HTML 코드가 너무 복잡한 경우, component를 만들어서 코드를 외부에서 불러줄 수 있다
- `<Modal></Modal>`로 선언하거나 `<Modal/>`로 선언해줘 해결할 수 있다

### Component를 쓰는 경우

- 사이트에서 반복적으로 사용하는 덩어리
- 내용이 자주 변경될 것 같은 부분
- 다른 페이지를 만들고 싶은 경우

### Component의 단점

- component가 너무 많으면 관리가 힘들다
- 한 function에서 다른 function의 데이터를 이용하는 것은 쉽지 않은데, props를 이용한다

### 🧩props: function에서의 전달 인자 역활

```jsx
...
function App (){
  let [글제목, 글제목변경] = useState(['남자코트 추천', '강남 우동맛집', '파이썬독학']);
  return (
    <div>
      <Modal colour ={red} 글제목={글제목}></Modal>
    </div>
  )
}

function Modal(props){
  return (
    <div className="modal" style={{color: {props.colour}}>
      <h4>{ props.글제목[0] }</h4>
      <p>날짜</p>
      <p>상세내용</p>
    </div>
  )
}
...
```

- 부모 component(이 경우 App.js)에서 자식 component(이 경우 Modal)로 함수의 전달 인자 같은 `props`를 전달할 수 있다
- `props`는 state값 뿐 아니라 일반 문자열, 숫자, 함수같은 것들도 전달할 수 있다
- props를 이용하여 색 데이터를 전달받고, state값을 전달받아 HTML문서에서 값을 바꿔줄 수 있다
- 부모 → 자식은 가능하지만 자식 → 자식, 자식 → 부모로의 props전달은 불가능하다

### 🧩Export / Import: 코드가 너무 길어진다면 사용

Data.js

```jsx
let a = 10;
export default a;
```

App.js

```jsx
import a from './data.js';
console.log(a)
```

- 긴 object자료형이나 코드가 너무 길어지는 경우 새롭게 component로 분리할 수 있다. 이 경우, 분리된 코드에서 export를 통해 넘겨줘야 하고, 받는 곳에서는 import에서 해당 JS파일의 경로를 불러오고, 그대로 이용할 수 있다