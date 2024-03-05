# State: 값이 변동되는 경우 handling

### 🧩State: HTML안에 데이터를 유동적으로 바꿔준다

```jsx
...

function App(){
 
  let [글제목, b] = useState('남자 코트 추천');
	let [count, setCount] = useState(0);
  let posts = '강남 우동 맛집';

  return (
    <div className="App">
      <div className="black-nav">
        <div>개발 blog</div>
      </div>
      <div className="list">
        <h4>{ 글제목 }</h4>
        <p>2월 17일 발행</p>
      </div>
    </div>
  )
}

...
```

- 기존 변수와 달리, `useState`를 사용하면 유동적으로 값을 변경시킬 수 있다. 값이 바뀌면, HTML문서에 있는 내용이 알아서 바뀐다
- `const [a, b] = useState(…)` 으로 작성하는데, a는 state의 이름, b는 state값을 변경할때 이용하는 함수, 마지막으로 `useState`안에는 초기 값을 넣어준다
- 위의 페이지를 예로 들 때, 개발 blog라는 페이지에서 타이틀에 해당하는 부분은 유동적으로 바뀔일이 없기 때문에 그냥 변수로 사용하고, 안에 데이터로 들어가는 글제목, 날짜의 경우에는 데이터에 따라 유동적으로 바뀔 수 있기 때문에 `useState`를 사용하는 것이 더 좋다

```jsx
...

let [count, setCount] = useState(0);

//count = count + 1;
setCount(2);

...
```

- count의 값을 HTML에서 이용한다고 가정할때, 주석처리된 코드와 같이 변수를 변경 하더라도 변경된 count가 반영이 안된다
- data를 바꾸고 바로 HTML에 적용시키고 싶다면, state를 선언했을때 만들어준 초기화하는 함수를 이용해서 state값을 바꿔준다

### 배열의 state변경

```jsx
  return (
    <button onClick={ ()=>{ 
      title[0] = '여자코트 추천';
      title(setTitle)
    } }> 수정버튼 </button>
  )
```

- 배열의 특정 값을 변경시키고자 할때, 다음과 같이 직접 title의 원소를 변경시켜줄 수 있자만, 보통 배열을 복사하고 그 복사한 배열을 다시 set을 통해 값을 초기화해준다

```jsx
  return (
    <button onClick={ ()=>{ 
      let copy = title;
      copy[0] = '여자코트 추천';
      setTitle(copy)
    } }> 수정버튼 </button>
  )
```

- 위 코드와 같이 새로운 변수를 복사하여 set을 통해 값을 변경시키려고 하더라도 변경되지 않는다
- 이는 `let copy = title`은, copy라는 새로운 객체를 만드는 것이 아니라 title이 만들때 생긴 객체를 동일하게 참조하기 때문에 setTitle에서는 값이 변경되었다고 인식하지 못하여, state가 수정되지 않는 것이다

```jsx
...

let [title, setTitle] = useState( ['남자코트 추천', '강남 우동맛집', '파이썬 독학'] );  
  
  return (
    <button onClick={ ()=>{ 
      let copy = [...title];
      copy[0] = '여자코트 추천';
      setTitle(copy)
    } }> 수정버튼 </button>
  )
...
```

- `spread operator`: …은 괄호를 벗겨주라는 내용으로, `let copy = […title]`은 title과 같은 객체를 참조하게 되는 것이 아니라, 내용을 그대로 복사하여 새로운 객체를 만들어준다
- 같은 객체를 pointing하는 것이 아니기 때문에, set을 통해 값을 변경하고자 할때, 값이 다른 것으로 인식하여 값이 변경된다