# [React] styled-components

- Javascript 파일 내에서 CSS를 사용할 수 있게 해주는 CSS-in-JS 라이브러리로 React 프레임워크를 주요 대상으로 한 라이브러리 입니다.
- Javascript 코드 내에서 일반 CSS로 구성 요소의 스타일을 지정할 수 있습니다.



## styled-components 사용 이유

Components가 많을 경우, Class명이 중복 될 수 있는 문제가 발생할 수 있습니다.

기본적으로 Components간의 Class명은 중복이 되면 안됩니다.

이를 방지하기 위해 `Class 선언 없이 component에 CSS를 직접 선언하는 방식` 을 사용하면 각 components간의 중복이 발생할 일도 없고, Class명이 중복되는 일도 없게 됩니다.

> module.css를 사용하면 Components간의 Class 명이 중복되어도 상관 없습니다.



## 설치 방법

``` bash
$ npm install styled-components
```



## 사용 방법

사용하려는 Component에 직접 `Import` 하여 사용합니다.

**1. 기본적인 사용 방법**

``` jsx
import styled from 'styled-components';

const Wrapper = styled.div`
  padding: 20px;
  background-color: red;
`;

const App = () => {
  return (
    <Wrapper>
    </Wrapper>
  )
}
```

> - 스타일링을 원하는 태그명을 변수에 대입
> - `styled.태그` 뒤에 백틱을 넣어준 뒤, 그 안에 원하는 CSS 스타일링



**2. 응용 방법**

``` jsx
/* Detail.jsx */

import styled from 'styled-components';

const Box = styled.div`
  padding: 20px;
  background-color: red;
`;

const Title = styled.h4`
  font-size: 24px;
  color: blue;
`;

const Button = styled.button`
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 14px;
  line-height: 1.5;
  border: 1px solid lightgray;
  
  color: ${(props) => props.color || 'gray'};
  background: ${(props) => props.background || 'white'};
`;

const Detail = (props) => {
  return (
    <div className='detail'>
      <div className='container'>
        <Box>
          <Title>styled-components 예제</Title>
        </Box>
        
        <Button>Default Button</Button>
        <Button color='green' background='pink'>Pink Button</Button>
      </div>
    </div>
  )
};
```



**3. 생성한 styled-components 재사용**

``` jsx
import styled from 'styled-components';

const Button = styled.div`
  color: red;
  font-size: 14px;
  margin: 4px;
  padding: 4px 6px;
  border: 2px solid red;
  border-radius: 5px;
`;

// Button의 속성을 상속 받아 새로운 anchor 태그 생성
const CloudButton = styled(Button.withComponent('a'))`
  color: lightblue;
  border-color: lightblue;
`;

const App = () => {
  return (
    <>
      <Button>Normal Button</Button>
      <CloudButton>Cloud Button</CloudButton>
    </>
  );
};
```

> withComponent 메소드는 새로운 style-component를 생성하고 다른 태그에 적용 시킵니다.
>
> 위의 코드처럼 withComponent('a')일 경우 button 태그의 styleComponent를 a 태그에 적용시킨다는 의미입니다.



**4. Mixin Css Props**

```jsx
import styled, { css } from 'styled-components';

// styled-components의 css를 활용하여 공통적으로 사용할 스타일링을 정의할 수 있습니다.
const FlexCenter = css`
  display: flex;
  justify-content: center;
  align-items: center;
`;

const FlexBox = div`
  ${FlexCenter};
`;

const Circle = (radius, stroke = '10') => css`
  position: absolute;
  border-radius: 50%;
  height: ${radius * 2}px;
  width: ${radius * 2}px;
  border: ${stroke}px solid rgb(0, 0, 0, 0.5);
`;
```

> styled-component는 자주 사용하는 CSS를 관리 하기 위해 `css props`를 사용합니다.



**5. 전역 스타일 적용**

Component 단위로 스타일을 작성하게 되면 전역 스타일을 적용하는 법에 대해 숙지해야 합니다.

기존 SCSS에서 `common.scss` , `variable.scss`, `App.scss` 등을 만들어서 사용했습니다.

통상적으로 styled-components 형태로 스타일링을 할 때 전역 스타일 적용을 하게 되면 `GlobalStyles.js` 파일을 생성하여 적용하게 됩니다.

``` js
// 전역 스타일 적용 예시

import { createGlobalStyle } from 'styled-components';
import reset from 'styled-reset';

const GlobalStyle = createGlobalStyle`
  ${reset}
  
  * {
    box-sizing: border-box;
  }
  
  body {
    padding: 0;
    margin: 0;
    font-family: 'Noto Sans KR', sans-self;
  };
  
  a {
    text-decoration: none;
  }
  
  ul {
    list-style: none;
  }
  
  button {
    display: flex;
    cursor: pointer;
    outline: none;
  }
  
  // ... 스타일링
`;
```



styled-components는 `GlobalStyle`과 `ThemeProvider` 를 사용하여 전역으로 사용할 수 있는 요소들을 쉽게 정의 할 수 있습니다. `src/styles/` 하위에 GlobalStyle 파일을 생성합니다.

위와 같이 스타일링을 하고 나면, 최상위 컴포넌트에 래핑하여 적용시키면 됩니다.

``` jsx
// index.jsx

import * as ReactDOM from 'react-dom/client';
import App from './App';
import GlobalStyle from './styles/GlobalStyles';
import theme from './styles/Theme';
import { ThemeProvider } from 'styled-components';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <>
    <GlobalStyle />
    <ThemeProvider theme={theme}>
      <App />
    </ThemeProvider>
  </>
);
```



추가적으로 `src/styles` 하위에 `Theme.js` 파일을 생성하여 아래와 같은 코드를 생성하여 `ThemeProvider` 의 props로 넘겨줍니다.

``` js
// Theme.js

const Theme = {
  white: '#ffffff';
  
  // other 색상 코드 
};

export default Theme;
```



상단의 Theme에 있는 색상을 사용하기 위해서는 아래와 같은 코드를 입력합니다.

``` js
const Container = styled.div`
  background-color: ${props => props.theme.white}
`;
```



## MUI와 styled-components 함께 사용하기

[MUI](https://mui.com/)를 styled-components에서도 사용할 수 있습니다.

기본적으로 MUI를 사용하기 위한 환경 설정은 전부 되어 있다고 가정하겠습니다.

``` jsx
// MUI의 아이콘 중 CheckBox 아이콘을 styled-components와 함께 사용하는 예제

import CheckBoxIcon from '@mui/icons-material/CheckBox';

const CheckBox = styled(CheckBoxIcon)`
	font-size: 18px;
	color: gray;
	
	&:hover {
	  cursor: pointer;
	}
	
	// ... 스타일링
`;
```

> `styled(사용할 MUI 아이콘)` 을 통해 함께 사용할 수 있습니다.
