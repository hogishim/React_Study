# Ajax: 서버와 통신하기

### 🧩AJAX: 서버에 GET, POST 요청등을 하는 경우에 새로고침 없이 데이터를 주고 받을 수 있도록 해준다

![Untitled](Ajax/Untitled.png)

### Server: 유저가 데이터나 작업을 요청하여 결과를 받아오는 대상

![Untitled](Ajax/Untitled%201.png)

- 요청시에는 어떤 데이터인지, 어떤 방법으로 요청할지 기재한다
- 데이터를 가져올때는 GET, 데이터를 서버로 보내는 경우에는 POST
- 서버에 요청하는 방식
    - `XMLHttpRequest`: 이전에 사용하던 문법
    - `fetch`: 최신 문법
    - `axios`: 외부 라이브러리를 이용하여 통신
    
    ```
    npm install axios
    ```
    

### Axios: 외부 라이브러리를 이용한 통신

```jsx
import axios from 'axios'

function App(){

  let [shoes, setShoes] = useState("...");

	
  return (
    <button onClick={()=>{
      axios.get('https://codingapple1.github.io/shop/data2.json').then((result)=>{
        let copy = [...shoes, ...result.data]
        setShoes(copy)
      })
      .catch(()=>{
        console.log('실패함')
      })
    }}>버튼</button>
  )
}
```

![Untitled](Ajax/Untitled%202.png)

- `axios`를 import 해온다
- 버튼을 클릭하면, 이벤트로 `get()`안에 있는 url로 요청을 보내게 된다
- '[https://codingapple1.github.io/shop/data2.json](https://codingapple1.github.io/shop/data2.json)' 이 url로 get 요청을 하여 해당 url에서 결과를 반환하는 것을 받아오게 된다
- 결과를 성공적으로 받았다면 then에 있는 내용을 실행하게 된다
- then에서 실행될 코드는 state변수에 url로 부터 가져온 데이트를 넣어주는 코드이다
- 받아온 결과는 `result.data`안에 들어있게 된다
- copy변수에 [{}, {}, {}, {}, {}, {}, {}]와 같은 형식으로 기존 데이터 + url애서 받아온 데이터가 초기화된다
- state가 변경되었기 때문에 받아온 데이터가 업데이트 된 내용으로 다시 rendering된다
- 결과를 받는 도중 오류가 발생하면 `catch()` 안에 있는 내용을 실행하게 된다

```jsx
Promise.all( [axios.get('URL1'), axios.get('URL2')] ).then(...
```

- 만약 여러 url에서 동시에 받아와야하는 경우에는 다음과 같이 `Promise.all`을 이용하여 안에 받아와야하는 url들을 넣어주면 된다
- 이러한 코드는 보통 두 url 이상에서 데이터를 받아와야 하는데, 두 경우 모두 성공한 경우에 코드를 실행시키고 싶어하는 경우에 보통 사용한다

```jsx
axios.post('URL', {name : 'kim'})
```

- 서버에 내용을 작성하기 위해서는 첫 인자로 url을 넣어주고, 두번째 인자로는 전달하고자하는 값을 넣어준다
- 완료시 특정 코드를 실행시키고 싶다면 `then()`안에 실행하고자 하는 코드를 넣어주면 된다

```jsx
fetch('URL').then(result => result.json()).then((result) => { console.log(result) } )
```

- 서버로 요청을 주고 받을때는 array 형식이 아닌, 문자열 형식으로만 주고 받을 수 있다
- 서버와 통신하게 되면, `post, get`을 사용하면 자동으로 JSON형식으로 변환시켜주기 때문에 별도의 변환 과정 없이 데이터를 바로 이용할 수 있다
- 그러나 `fetch`를 통해서 데이터를 받아오는 경우에는 명시적인 형 변환이 필요하다. 만약 fetch를 이용하게되면 받아오는 데이터를 JSON → object/array 형식으로 변형해야 기존에 get 코드 처럼 데이터를 이용할 수 있다