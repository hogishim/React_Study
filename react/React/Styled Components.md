# Styled Components

### 🧩Styled Components: CSS파일을 수정할 필요 없이 react코드에서 수정 가능

- `npm install styled-components`를 터미널 창에 입력하여 먼저 필요 파일을 설치한다

```jsx
import styled from 'styled-components';

let Box = styled.div`
  padding : 20px;
  color : grey
`;
let YellowBtn = styled.button`
  background : yellow;
  color : black;
  padding : 10px;
`;

function Detail(){
  return (
    <div>
      <Box>
        <YellowBtn>버튼임</YellowBtn>
      </Box>
    </div>
  )
}
```

- 새롭게 component를 만들어서 HTML코드에 태그로 추가할 수 있다
- backtick기호 안에 CSS처럼 코딩을 해주면 된다
- CSS파일을 따로 import할 필요 없기 때문에, loading이 더 빨라지고, CSS파일을 분리하지 않으면 다른 JS파일도 영향을 받을 수 있는데, 이 styled-component는 다른 파일에 영향을 주지 않는다

```jsx
import styled from 'styled-components';

let YellowBtn = styled.button`
  background : ${ props => props.bg };
  color : ${ props => props.bg == 'blue' ? 'white' : 'black' };
  padding : 10px;
`;

function Detail(){
  return (
    <div>
        <YellowBtn bg="orange">오렌지색 버튼임</YellowBtn>
        <YellowBtn bg="blue">파란색 버튼임</YellowBtn>
    </div>
  )
}
```

- 위와 같이 prop를 이용하여 공통적인 특징을 가지고, 세부 내용이 다른 버튼 같은 것들을 더 짧게 만들어줄 수 있다
- 삼항 연산자 이용하여 상황에 따라 다른 속성을 주는 것도 가능하다