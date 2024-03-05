# Async/Sync

### Sync: 동기방식. 코드 적은 순서대로 윗줄부터 차례대로 실행되는 것

```jsx
console.log(1)
console.log(2)
console.log(3)
```

- 이런 일반적인 코드는 synchronous하게 처리되는 것으로, 코드를 적은 순서대로 차례대로 실행되는 것

### Async: 비동기 방식, 코드를 적은 순서대로 실행이 아니라 오래 걸리는 코드는 놔두고 다른 코드를 먼저 실행

- `Ajax`, `eventlistener`, `setTimeout`, `setState`함수 같은 코드는 다른 코드와 달리 아래있는 코드가 더 먼저 실행될 수 있다

```jsx
console.log(1)
axios로 get요청 이후에 console.log(2) 실행
console.log(3)
```

- 이렇게 되면, 1, 2, 3순서가 아니라 더 오래 걸리는 `axios get`은 건너 뛰고 1, 3이 실행되고, 이후에 2가 실행이 된다
- `axios`가 `console.log`보다 더 빨리 끝나도, 무조건 1, 3, 2 순서대로 진행된다

```jsx
<button onClick={()=>{

  setCount(count+1);
  if ( count < 3 ) {
    setAge(age+1);
  }
         
}}>increase age</button> 
```

- 이런 코드가 있을때, `count`가 0에서 3 전까지 if문 안의 내용이 실행되야 하지만 실행해보면 `count`가 3이 된 이후에도 한번 setAge가 실행된다
- 이는, set 함수는 async방식이기 때문에, `count`가 3이되는 시점보다 if문 실행이 먼저 되기 때문에, 실제로 `count`가 3이되었더라도 `setCount`가 `state`를 3으로 만들지 않았기 때문에, if문에서는 3보다 작다고 생각하고 실행시킨 이후에 `setCount`가 반영이 된다

```jsx
useEffect(()=>{
    
 }, [count]) 
```

- `async`처리를 원활하게 하기 위해서는, 다음과 같이 `count`를 변경된 시점에서 다음에 실행시켜야 하는 코드를 넣어주면 된다
- Ajax 요청을 보낼때 이러한 비동기 문제가 많이 발생한다. 예를 들어 .map으로 서버에서 받아온 데이터를 보여줄 경우, 비동기로 서버에 받아온 내용이 set되기 전에 .map이 실행되어 오류가 발생할 수 있다. 이러한 경우를 대비해, async 처리를 잘 해두어야 한다