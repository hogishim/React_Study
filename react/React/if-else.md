# if-else: 조건에 따른 rendering

### 🧩if/else: return문 안에 JSX에서는 기존에 JS에서 사용하는 if-else를 사용 못하기 때문에 다른 방식이 필요하다

### 기본 if/else문

```jsx
function Component() {
  if ( true ) {
    return <p>참이면 보여줄 HTML</p>;
  } else {
    return null;
  }
} 

//같은 코드, 다른 방식
function Component() {
  if ( true ) {
    return <p>참이면 보여줄 HTML</p>;
  } 
  return null;
} 
```

- return 밖에 써주는 방식으로, 조건에 따라서 다른 return문을 보내준다
- 아래 코드와 위의 코드는 동일한 기능의 코드다. 코드는 return을 만난 시점에서 아래 코드는 더이상 실행하지 않기 때문에 위 조건을 만족하지 않으면 아래 코드가 실행되고, 만족하면 if문 안의 코드가 실행된다

### Ternary operator(삼항 연산자)

```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 이게</p>
        : <p>거짓이면 이게</p>
      }
    </div>
  )
} 

```

- if/else를 대신하여 JSX안에서도 사용 가능하다
- 가장 앞에 조건, 해당 조건을 만족하면 ?이후의 내용이, 그리고 만족하지 않는 경우에는 : 이후의 내용이 렌더링 된다

```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : ( 2 === 2 
            ? <p>안녕</p> 
            : <p>반갑</p> 
          )
      }
    </div>
  )
} 
```

- 중첩 사용도 가능하지만 가독성이 굉장히 떨어지기 때문에 다른 방법을 찾아보는 것이 좋다

### &&연산자

```jsx
function Component() {
  return (
    <div>
      { 1 === 1 && <p>참이면 보여줄 HTML</p> }
    </div>
  )
}
```

- 조건이 참이면 &&뒤가 랜더링 되고, 참이 아니라면 null로, 아무것도 랜더링하지 않는다

### Switch문

```jsx
function Component2(){
  var user = 'seller';
  switch (user){
    case 'seller' :
      return <h4>판매자 로그인</h4>
    case 'customer' :
      return <h4>구매자 로그인</h4>
    default : 
      return <h4>그냥 로그인</h4>
  }
}
```

- if를 연달아 사용해야하는 경우가 생기면, switch문을 사용하는 것이 여러면에서 더 유리하다
- 검사해야할 조건이 한가지인 경우에만 가능하다

### Object/array자료형 응용

```jsx
var tabUI = { 
  info : <p>상품정보</p>,
  shipping : <p>배송관련</p>,
  refund : <p>환불약관</p>
}

function Component() {
  var currentState = 'info';
  return (
    <div>
      {
        tabUI[currentState]
      }
    </div>
  )
} 
```

- 보여주고 싶은 내용을 배열에 담아두고, index를 이용하여 특정 원소를 꺼내는 방식으로, 조건에 따라 원하는 내용을 보여주도록 할 수 있다