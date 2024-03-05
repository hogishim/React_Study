# 필요 없는 문법 - 알아만 두자!

### 🧩public 폴더: react는 개발 완료후, build해주는 과정에서 개발한 내용을 한 폴더에 압축. src폴더에 있는 내용은 압축이 되지만 public 폴더 내용은 유지된다

- 내용을 그대로 보존하고 싶다면, public폴더에 넣어주는 것이 좋다

```jsx
<img src="/logo192.png" />
```

- public 폴더에 내용은 바로 이미지 경로를 입력해주면 된다

```jsx
<img src={process.env.PUBLIC_URL + '/logo192.png'} />
```

- 첫 코드와 같이 입력해놓는 경우에는, ~.com같은 경우에는 문제가 없지만, ~.com/home과 같은 link에서는 문제가 발생할 수 있기 때문에, 다음과 같이 링크의 앞부분에 내용을 추가해야 한다

### 🧩class문법: 이제는 더이상 사용하지 않는 react의 코딩 방식

```jsx
class Modal2 extends React.Component {
  constructor(props){
    super(props);
    this.state = {
      name : 'kim',
      age : 20
    }
  }

  render(){
    return (
			<button onClick={()=>{ this.setState({age : 21}) }}>버튼</button>
      <div>안녕 { this.props.프롭스이름 }</div>
    )
  }

}
```

- class에는 기본적으로 `constructor-super-render`가 있어야 한다
- state를 만들고 싶다면 `this.state ={…}`안에 object자료형 형식으로 작성한다
- 자료를 꺼내고 싶다면 `this.state.name`과 같은 방식으로 꺼낼 수 있다
- state를 바꾸고 싶다면 `this.setState(…)`안에 `key: value`형식으로 지정하여 state를 바꿔줄 수 있다
- props를 사용하고 싶다면 `constructor, super`에 `props`를 등록해준다
- 기존에 쓰던 방식과 동일하게 `<Modal2>…</Modal2>`혹은 `<Modal2/>`로 component를 불러올 수 있다
- component보다 훨씬 복잡하기 때문에 현재는 잘 사용하지 않는다

### 🧩Context API: props 넘겨주는게 귀찮을때 사용

app.js

```jsx
import {createContext} from "react";
...

export let Context1 = createContext();

function App(){

return(

...

<Context1.Provider value={{tab, shoes}}>
	<Details />
</Context1.Provider>

...

)

...
```

Details.js

```jsx
import {useContext} from 'react'
import {Context1} from './../App.js'

function Detail(){

let a = useContext(Context1)
//let {shoes} = useContext(Context1)

}
```

- Context를 만들고, 다른 component에서 이용하기 위해 export 해준다
- `<Context>`로 컴포넌트를 감싸고, value로는 전달하고자 하는 state값을 넣어준다
- 자식에서 이용할때는, Context를 경로에 맞춰서 불러오고, `useContext()`를 통해서 가져온 context를 이용할 것이라고 선언한다
- 이후에는 자유롭게 props 없이 전달받은 state를 사용할 수 있다
- Detail의 자식에서 state를 이용하고자 할때, 번거롭게 props를 두번 넘겨주는 과정을 거치지 않고도, Detail의 자식에서 바로 코드를 사용할 수 있다
- state가 변경되면 불필요하게 하위 componet가 모두 재렌더링 되고, import를 일일이 하는게 귀찮아서 잘 사용하지 않는다
- props 처리가 귀찮으면 redux같은 외부 라이브러리를 그냥 사용하는게 좋다