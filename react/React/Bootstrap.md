# Bootstrap 사용하기

### 🧩Bootstrap 라이브러리: 기존 Bootstrap을 react에 맞게 변형함

- editor에서 *`npm install react-bootstrap bootstrap`*으로 react내용을 설치해준다

```jsx
import 'bootstrap/dist/css/bootstrap.min.css';
```

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
  integrity="sha384-9ndCyUaIbzAi2FUVXJi0CjmCapSmO7SnpJef0486qhLnuZ2cdeRhO02iuK6FUUVM"
  crossorigin="anonymous"
/>
```

- 위 첫번째 코드를 App.js에 추가하거나, 아래 코드 내용을 App.css에 추가해서 React를 이용할 수 있다

### Bootstrap 이용하기

```jsx
import {Button} from 'react-bootstrap'

function App(){
  return (
    <div>
      <Button variant="primary">Primary</Button>
    </div>
  )
}
```

- Bootstrap 사이트에서, 원하는 버튼을 불러온 이후에, Bootstrap 버튼을 문서에 만들어 줄 수 있다
- `<Button>`이라는 component를 사용하기 때문에 import시켜주는 코드가 있어야 한다