# JSX

`JSX`는 Javascript에 XML을 추가한 확장 문법입니다.

리팩트로 프로젝트 개발할 때 사용되므로, 공식적인 Javascript 문법은 아닙니다. 브라우저에서 실행되기 전에 babel을 사용하여 일반 Javascript 형태의 코드로 변환됩니다.



## JSX 코드를 babel을 통해 Javascript 코드로 변환

> Babel 이란?
>
> Javascript 컴파일로서, 현재 및 이전 브라우저 환경에서 ES5+ 코드를 이전 버전의 Javascript로 변환하는데 주로 사용되는 도구 체인입니다.

``` javascript
// JSX를 사용한 경우 (Javascript 코드 안에 HTML 코드처럼 사용할 수 있습니다.)
function App() {
  return (
    <h1>Hello World!</h1>
  );
}

// 위의 JSX 문법을 통해 작성하면, babel이 아래의 코드처럼 Javascript로 변환
function App() {
  return React.createElement('h1', null, 'Hello World!');
}
```

> - JSX를 사용하지 않는 경우, 두번째 코드처럼 컴포넌트를 렌더링 할 때 마다 매번 `React.createElement` 함수를 사용해야 합니다. 이런 방식은 매우 불편하고 비효율적입니다.
> - 다만 JSX는 React 개발 시 사용하는 문법이기 때문에 공식적인 Javascript 문법은 아닙니다.



## JSX 문법 규칙

>  JSX에는 다양한 규칙들이 존재합니다.

### 1. 하나의 부모 요소가 감싸는 형태

컴포넌트에 단일 요소가 아닌 여러 요소가 존재한다면 반드시 부모 요소의 내부에 있어야 합니다.

이유는 `Virtual DOM(가상돔)`에서 컴포넌트 변화를 감지할 때 효율적으로 비교할 수 있도록 컴포넌트 내부는 하나의 DOM 트리구조로 이루어져야 한다는 규칙이 있기 때문입니다.

``` javascript
// 에러 케이스
function App() {
  return (
    <h1>Hi</h1>
    <h2>Hello</h2>
  )
}

/* 정상 케이스 */

// 1) <div> </div> 사용
function App() {
  return (
    <div>
      <h1>Hi</h1>
      <h2>Hello</h2>
    </div>
  )
}

// 2) <Fragment> </Fragment> 사용 (div 태그보다 무거운 편이기 때문에 웬만하면 사용 X)
import React, { Fragment } from 'react';

function App() {
  return (
    <Fragment>
      <h1>Hi</h1>
      <h2>Hello</h2>
    </Fragment>
  )
}

// 3) <> </> 사용
function App() {
  return (
    <>
      <h1>Hi</h1>
      <h2>Hello</h2>
    </>
  )
}

export default App;
```



### 2. Javascript 표현식

- JSX 내부에서도 Javascript 표현식을 사용할 수 있습니다. JSX 내부에서 `{ }` 중괄호로 감싸주면 됩니다.
- 유효한 모든 Javascript 표현식을 넣을 수 있습니다.

``` javascript
function App() {
  const name = "Hong";
  return (
    <>
      <h1>Hello</h1>
      <h2>My name is {name} </h2>
    </>
  );
}

export default App;
```



### 3. 조건부 연산자

- JSX 내부의 Javascript 표현식에는 if문, for문을 사용할 수 없습니다.
- 따라서 조건에 따른 내용을 렌더링해야 하는 경우, JSX밖에서 조건문을 사용하여 미리 값을 설정하거나, `{ }` 내부에 삼항 연산자를 사용하면 됩니다.

``` javascript
// 1. JSX 외부에서 조건 설정
function App() {
  let result = '';
  const check = 'Y';
  
  if (check === 'Y') {
    result = <h1>My name is Hong</h1>;
  } else {
    result = <h1>Who are you?</h1>;
  }
  
  return result;
}

// 2. JSX 내부에서 조건 설정
function App() {
	const check = 'Y';
  return (
    <>
      <div>
        {check === 'Y' ? (<h1>My name is Hong</h1>) : (<h1>Who are you?</h1>)}
      </div>
  );
}

// 3. AND 연산자 (&&) 사용 (조건이 만족하지 않을 경우 아무것도 노출되지 않습니다.)
function App() {
  const check = 'Y';
  return (
    <>
      <div>
        {check === 'Y' && <h1>My name is Hong</h1>}
      </div>
  );
}

// 4. 즉시실행함수 사용
function App() {
  const check = 'Y';
  return (
    <>
      {
        (() => {
          if (check === 'Y') {
            return <h1>My name is Hong </h1>;
          } else {
            return <h1> Who are you? </h1>;
          }
        })()
      }
    </>
  );
}
```

> (3) 케이스에서 && 연산자를 통해 조건부 렌더링을 할 수 없는 이유는 React에서 false를 렌더랑 할 때는 null과 마찬가지로 아무것도 나타나지 않기 때문입니다.
>
> 여기서 주의할 점은 `0` 의 경우 통상적으로 false를 의미하지만 falsy(거짓같은) 값으로 취급되어 화면에 렌더링 됩니다.

### 4. undefined 렌더링하지 않기

React 컴포넌트에서는 함수의 return 값이 undefined일 경우 렌더딩 할 때 오류를 발생시킵니다.

``` javascript
function App() {
  const name = 'undefined';
  return name;
}

export default App;
```

> 위와 같이 undefined를 리턴할 경우 다음과 같은 에러가 발생합니다.

``` bash
App(...): Nothing was returned from render. This usually means a return statement is missing. 
Or, to render nothing, return null 
```

혹시라도 어떠한 값이 undefined일 수 있다면 `or 연산자` 를 통해 해당 값이 undefined일 경우 사용할 값을 지정하여 오류를 사전에 방지할 수 있습니다.

``` javascript
// 1. JSX 사용하지 않고 undefined 렌더링
function App() {
  const name = 'undefined';
  return name || '값은 undefined입니다.';
}


// 2. JSX 내부에서 undefined 렌더링
function App() {
  const name = 'undefined';
  return <div>{name || 'Hong'}</div>;
}
```



### 5. React DOM에서 Property 선언은 camelCase를 사용

``` javascript
// 1. JSX 스타일링
function App() {
  // css Style을 작성할 때도 camelCase 사용
  const cssStyle = {
    backgroundColor: 'red',
    fontSize: '16px'
  }
  
  return <div style={style}>Hello World</div>;
}

// 2. className
function App() {
  // 일반적으로 HTML에서 class 속성을 선언할 때와는 다르게 className이라는 속성을 camelCase로 선언
  return <div className="myClass">Hello, World</div>;
}
```



### 6. JSX 내부에 주석 사용하는 법

JSX 내부에서 주석을 사용할 경우 `{/* ~~~ */}`와 같은 형식을 사용합니다.

``` javascript
function App() {
  return (
    <>
      {/* 주석 사용하기 */}
      <div> Hello, World </div>
    </>
  );
}
```

