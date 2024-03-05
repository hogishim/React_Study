# React 18 개선 사항

### Batching

```jsx
setCount(1) 
setName(2) 
setValue(3)
```

- 이렇게 state변경을 연달아 사용하는 경우, 재랜더링이 3번 일어나는 것이 아니라, 마지막에 1회만 처리해준다
- 18버전 이전에는 `Ajax`나 `setTimeout`안에 있을때 batching이 일어나지 않았지만, 18 이후로는 마찬가지로 마지막 1번만 재렌더링 된다
- batching되는 것을 원치 않는다면 `flushSync`라는 함수를 사용한다

### useTransition(): 코드의 실행 시점을 뒤로 넘겨준다

```jsx
import {useState, useTransition} from 'react'

function App(){

const [isPending, startTransition] = useTransition();

return(
	...
	startTransition(()=>{
		...
	})
	...
	isPending ? loading... ? return <div>name</div>
```

- `useTransition`의 함수를 시간이 오래 걸릴 것 같은 요소를 묶어주면, 묶어준 코드를 다른 코드보다 더 나중에 처리해준다
- 따라서 `<input>`같이 즉각적인 반응이 필요한 요소를 우선적으로 처리하고, 시간이 오래 걸리는 부수적인 요소는 나중에 처리해준다
- 직접적인 성능 개선이라기 보다는, 코드의 실행 시점을 옮겨주는 코드다
- `useTransition`의 변수 `isPending`은 감싼 코드가 처리중인 시점이라면 true로 초기화되는 것으로, 이 코드의 실행이 끝날때까지, 만약 `isPending`이 true라면 ‘loading중’같은 내용이 나오도록 할 수 있다

### useDeferredValue(): 변수에 변동이 생기더라도 늦게 처리해준다

```jsx
import {useState, useTransition, useDeferredValue} from 'react'

function App(){
  const [name, setName] = useState('')
  const state1 = useDeferredValue(name)
  
  return (
    <div>
      <input onChange={ (e)=>{ 
          setName(e.target.value) 
      }}/>

      {
        a.map(()=>{
          return <div>{state1}</div>
        })
      }
    </div>
  )
}
```

- `useTransition`과 동일한 기능을 하는데, `useTransition`과는 달리 state나 변수를 넣는다
- 처리 결과를 변수에 저장해놓고 있는다