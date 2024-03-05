# React-query: 실시간 데이터에 유리

### React query: SNS, 코인 거래소 같은 실시간으로 데이터를 보여줘야하는 사이트에서 유용하다

- 요청에 실패했을때 알아서 ajax 요청 재시도를 몇번 해준다

```
npm install react-query@3
```

- 설치를 위해 입력

Index.js

```jsx
import { QueryClient, QueryClientProvider } from "react-query"  //1
const queryClient = new QueryClient()   //2

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <QueryClientProvider client={queryClient}>  {/*3*/}
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  </QueryClientProvider>
); 
```

- react-query로부터 `QueryClient`와 `QueryClientProvider`를 import해온 이후에, `QueryClient` 변수를 하나 만들어준다
- `<App />`을 `<QueryClientProvider>` tag로 감싸주고, 안에 `queryClient` 변수를 넣어주면 된다

App.js

```jsx
import { QueryClient, QueryClientProvider, useQuery } from '@tanstack/react-query' 

function App(){
  let result = useQuery('name', ()=>
    axios.get('https://codingapple1.github.io/userdata.json')
    .then((a)=>{ return a.data })
  )
}
```

- `useQuery`로 axios를 감싸준다
- axios 요청을 통해 받아온 값을 then안에 return을 통해 설정해준다

### React query의 장점

- Ajax요청의 성공 실패 여부를 바로 파악할 수 있다
    
    ```jsx
          { result.isLoading && 'loading' }
          { result.error && 'error' }
          { result.data && result.data.name }
    ```
    
    - 결과로 나오는 result에 다양한 ajax의 현재 상태가 알아서 저장된다
    - `isLoading`: 데이터를 받아오는 과정일때 true가 된다
    - `error`: 데이터를 받아오는데 실패했을때 true가 된다
    - `data`: 데이터를 받아오는데 성공했다면, 받아온 데이터가 들어가게 된다
- 알아서 Ajax 재요청 해준다
    
    ```jsx
      let result = useQuery('name', ()=>
        axios.get('https://codingapple1.github.io/userdata.json')
        .then((a)=>{ return a.data }), 
    		{staleTime: 2000}
      )
    ```
    
    - 페이지에 들어간 상태에서 일정시간 경과하거나 다른 페이지로 갔다가 돌아오는 등 이러한 여러 경우에 알아서 다시 ajax를 요청해준다
    - 재요청을 끄거나 재요청 간격 조정(staleTime)을 하는 것이 가능하다
- 실패해도 알아서 다시 요청한다: 일정 횟수를 반복적으로, 오류가 나더라도 더 시도해본다
- Ajax를 통해 가져온 결과 props통해 전송 불필요: 그냥 하위 component에서도 동일하게 Ajax호출을 하고, props를 통해 넘길 필요가 없다. 알아서 반복적으로 호출하는 Ajax는 다시 호출하지 않고 값을 가져온다

### React-query 단점

- Ajax 요청을 계속 날리는 방식이기 때문에 HTTP1을 쓰는 서버나 브라우저는 비효율적일 수 있다
- 웹소켓이나 server-sent events같은 가벼운 방식으로도 구현 가능하다

### Redux Toolkit Query: Redux에서 react-query와 비슷하게 사용

- setting code가 조금 복잡하다
- Ajax요청 코드가 다양하고 많으면 그것들을 편하게 관리할 수 있도록 해준다