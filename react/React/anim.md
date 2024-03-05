# 애니메이션 효과 주기

### 🧩탭 UI 만들기

```jsx
<Nav variant="tabs"  defaultActiveKey="link0">
    <Nav.Item>
      <Nav.Link eventKey="link0">버튼0</Nav.Link>
    </Nav.Item>
    <Nav.Item>
      <Nav.Link eventKey="link1">버튼1</Nav.Link>
    </Nav.Item>
    <Nav.Item>
      <Nav.Link eventKey="link2">버튼2</Nav.Link>
    </Nav.Item>
</Nav>
<div>내용0</div>
<div>내용1</div>
<div>내용2</div>
```

- Bootstrap에서 디자인을 그대로 가져와서 이용한다. 선언 전 import해와야 한다
- 위에 nav 아이템을 3개 만들어주고, 3개를 만들어 주는데 각각의 아이템은 eventKey로 구분한다
- `defaultActiveKey`는 페이지 로드시 어떤 버튼에 눌린 효과를 줄지 결정하는 부분이다

```jsx
let [tab, setTab] = useState(0)
```

- 탭의 상태를 저장할 state를 만들어준다. 0, 1, 2중 하나를 표시한다
- 0을 기본 state로 지정한다

```jsx
{tab == 0? <div>내용0</div> : null }
```

```jsx
function Detail(){
  let [탭, 탭변경] = useState(0)

	...
  
  return (
		<Nav variant="tabs"  defaultActiveKey="link0">
	    <Nav.Item>
	      <Nav.Link onClick={()=>{ setTab(0) }} eventKey="link0">버튼0</Nav.Link>
	    </Nav.Item>
	    <Nav.Item>
	      <Nav.Link onClick={()=>{ setTab(1) }} eventKey="link1">버튼1</Nav.Link>
	    </Nav.Item>
	    <Nav.Item>
	      <Nav.Link onClick={()=>{ setTab(2) }} eventKey="link2">버튼2</Nav.Link>
	    </Nav.Item>
		</Nav>
	
    <TabContent 탭={탭}/>

	...
  )

...

}

function TabContent(props){

  if (props.탭 === 0){
    return <div>내용0</div>
  }
  if (props.탭 === 1){
    return <div>내용1</div>
  }
  if (props.탭 === 2){
    return <div>내용2</div>
  }
}
```

- 삼항 연산자를 3개 사용해서 구현하는 방법도 있지만, 코드가 지저분해질 수 있다
- 삼항 연산자를 사용하지 않고, 탭에 대한 state를 props로 넘겨서 component에서 if-else문으로 처리해줄 수 있다
- 눌리는 버튼에 따라서 state값을 바꿔주면, state를 props로 넘긴 이후에, state에 해당하는 값에 맞춰서 0, 1, 2일때 각각 다른 <div>를 return해준다

### 코드 작성 tip

```jsx
...
function TabContent(props){
  return [ <div>내용0</div>, <div>내용1</div>, <div>내용2</div> ][props.tab]
}
...
```

- 배열 형식으로 저장해놓고, state가 0, 1, 2로 올것이기 때문에 각 index마다 맞는 탭의 내용이 출력되게 된다

```jsx
function TabContent({tab}){
  return [ <div>내용0</div>, <div>내용1</div>, <div>내용2</div> ][tab]
}
```

- props를 쓰는 것이 너무 귀찮다면 전달 인자로 `{tab}`을 써주면 `props.tab`이 아니라, 그냥 tab만 써서 바로 이용할 수 있다

### 🧩컴포넌트 전환 애니메이션 주기

### 컴포넌트 전환 애니메이션 단계

1. 애니메이션 동작 전 classname 만들기
2. 애니메이션 동작 후 classname 만들기
3. transition 추가하기

### 컴포넌트 전환 애니메이션 구현

```css
.start {
  opacity : 0
}
.end {
  opacity : 1;
  transition : opacity 0.5s;

```

- 초기에는 `opacity`를 0으로 두어서, 안보이게 해놓는다
- 애니메이션 효과를 주기 위해, 특정 행동을 한 이후에는 end라는 class를 부착하여 투명도를 1로 하여 보이게 해주고, 애니메이션 효과를 주기 위해 transition을 0.5초 지속하도록 한다

```jsx
function TabContent({tab}){

  let [fade, setFade] = useState('')

  useEffect(()=>{
    setFade('end')
  }, [tab])

  return (
		//<div className={`start ' + ${fade}`}>
    <div className={'start ' + fade}>
      { [<div>내용0</div>, <div>내용1</div>, <div>내용2</div>][탭] }
    </div>
  )
}
```

- 전환 효과가 발생해야하는 경우는 버튼을 눌러 다른 탭으로 전환되는 경우에 서서히 투명도가 변경되어 전환이 되어야 한다. 즉, 탭이 전환되는 경우는 tab의 상태가 변경되는 경우이기 때문에 `useEffect`를 사용하여 tab의 state가 변경되는 경우에 end라는 class를 탈부착 시키면 된다
- start라는 기존 class에서 버튼을 눌러서 tab이 전환된다면 end를 부착하여 등장하는 효과를 주면 된다
- 위 코드와 같이 하면 변경되어야할 것 같은데 변경이 되지 않는다. 이는 end가 탈부착이 되어야 나타나는 효과가 발생하는 것인데 그냥 붙어만 있기 때문에 애니메이션 효과가 발생하지 않는다
- 애니메이션 효과를 주려면 end를 탈부착시킬 수 있어야 한다

```jsx
function TabContent({tab}){

  let [fade, setFade] = useState('')

  useEffect(()=>{
    setTimeout(()=>{ setFade('end') }, 100)
	  return ()=>{
	    setFade('')
	  }
  }, [tab])

  return (
    <div className={'start ' + fade}>
      { [<div>내용0</div>, <div>내용1</div>, <div>내용2</div>][탭] }
    </div>
  )
}
```

- 때는 과정이 있는 코드를 구현해야 한다. 만약 `setFade(’’)`를 써준다면 end가 떼진 이후에 다시 붙여지면서 등장하는 효과의 애니메이션을 줄 수 있을 것이다
- `setTimeout`을 쓰지 않는다면 `setFade(’ ‘)`를 쓰더라도 애니메이션 효과가 발생하지 않을 것이다
- react 18이후부터는 `automatic batch`기능이 생겨서 state 변경함수가 여러개 있는 경우에는 각각을 처리하는게 아니라 한번에 처리해버리기 때문에 end로 변경, ‘ ’로 변경이 한번에 일어나버리기 때문에 결국 탈부착 효과가 나지 않는다
- 두 state변경 함수를 따로 실행되도록 `setTimeout`을 걸어주면 애니메이션 효과가 적용된다
- `setTimeout`외에도 `flushSync()`를 사용하여 `automatic batching`을 막아줄 수 있다