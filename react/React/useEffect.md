# useEffect: rendering

### 🧩useEffect: mount되거나 component가 업데이트 되는 경우에 실행된다

```jsx
import {useState, useEffect} from 'react';

function Detail(){

  useEffect(()=>{

    console.log('hi')
  });
  
  return (생략)
}
```

- component는 `mount, update, unmount`의 상태를 가질 수 있다
- `useEffect`는 rendering이 끝날때마다 실행된다. 그러나 index.js의 `<React.StrictMode>`라는 태그 때문에, console.log의 내용이 두번 실행된다
- 따라서 서버에서 데이터를 받아오는 등, 연산이 오래 걸리는 것이 있다면, `rendering`을 해서 일단 보여준 이후에, 연산을 진행하는 것이 더 나을 수도 있다

```jsx
useEffect(()=>{ 
  let a = setTimeout(()=>{ setAlert(false) }, 2000)
  return ()=>{
    clearTimeout(a)
  }
}, [])

useEffect(()=>{ 
  let a = setTimeout(()=>{ setAlert(false) }, 2000)
  return ()=>{
    clearTimeout(a)
  }
}, [integer])
```

- `useEffect`에 뒤에 `[]`을 넣으면, 최초 rendering될때 한번만 실행되고, 다른 요소들이 업데이트 되는 경우가 있더러도 최초 한번만 실행된다. `[]`가 없으면, 재 rendering될 때마다 실행된다
- `useEffect`안에 `return()`은 `useEffect`안에 코드가 실행되기 이전에 실행된다
- `useEffect`는 `mount`되는 경우에, `return`은 `unmount`되는 경우에 실행되는 것으로, react의 특성상 여러번 rendering되는 경우가 많은데, 이때마다 timer를 새로 설정하거나, 데이터를 받아오는 코드가 있는 경우에 문제가 발생할 수 있다
- `unmount`되어 다시 실행되는 경우가 생긴다면, 이전 내용을 날리고 시작하는 것이 좋다. 따라서 일단 실행 전에 날린 이후에, `useEffect`안에 내용을 실행하면 된다. 이를 `clean up function`이라고 부른다
- `useEffect`에 `[integer]`를 넣어주면, rendering될때와, integer가 업데이트 되는 경우마다 실행된다