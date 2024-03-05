# Localstorage: 브라우저를 새로고침해도 값을 유지

### 🏬Local storage

- key-value형식으로 저장한다
- 사이트마다 5MB정도 문자 데이터만 저장할 수 있다 - object/array 저장 불가
- 유저가 임의로 삭제하지 않는 이상, 영구적으로 저장
- state를 자동 저장되게 하고 싶다면 redux-persist, Jotai, Zustand같은 library를 사용한다

↔session storage: 유사하게 데이터를 저장하고 있지만, 데이터가 브라우저를 닫으면 사라진다

### Local storage 이용 방법

```jsx
localStorage.setItem('name', 'moozi');
localStorage.getItem('name');
localStorage.removeItem('name')
```

- `setItem`: 저장할 key와 value를 적어줄 수 있다
- `getItem`: 저장해놓은 값을 key값을 통해 가져올 수 있다
- `removeItem`: 저장해놓은 값을 key값을 통해 삭제한다

### Object/Array형식 자료 저장 방법

```jsx
localStorage.setItem('obj', JSON.stringify({name:'kim'}) );

var a = localStorage.getItem('obj');
var b = JSON.parse(a)
```

- local storage에는 ‘문자’만 저장되기 때문에 object/array파일의 저장 방식은 JSON을 이용한다
- `JSON.stringfy()`: object나 array를 “”로 감싸서 컴퓨터에서 문자열로 인식하게 만들어 localstorage에 저장할 수 있게 만들어준다
- `JSON.parse()`: local storage에서 값을 다시 가져와야 하는 경우에는 다시 JSON을 parsing하는 과정이 필요하기 때문에 parse과정을 거친 이후에 값을 사용할 수 있다

### 최근 본 상품의 ID를 local storage에 저장하기

```jsx
...
useEffect(()=>{
  let val = localStorage.getItem('watched')
  val = JSON.parse(val)
  val.push(item.id)

  //Set으로 바꿨다가 다시 array로 만들기
  val = new Set(val)
  val = Array.from(val)
  localStorage.setItem('watched', JSON.stringify(val))
}, [])
...
```

- 값을 local storage로 부터 가져오고, 배열 형식으로 되어있는 JSON을 parsing 해준다
- ID값을 배열에 새로 넣어준다
- 중복을 제거하기 위해서 set을 통해 중복 값을 제거해주고, 다시 set을 array형식으로 변환해준다
- 마지막으로 다시 JSON형식으로 바꿔주고, 값을 localstorage에 저장한다

```jsx
useEffect(() => {
  let val = localStorage.getItem('watched');
  val = JSON.parse(val);

  // 중복 체크
  if (!val.includes(item.id)) {
    val.push(item.id);
    localStorage.setItem('watched', JSON.stringify(val));
  }
}, []);

```

- ID가 이미 있는지 확인하기 위해서 includes를 통해 값이 있는지 확인하고, 없다면 push해준다