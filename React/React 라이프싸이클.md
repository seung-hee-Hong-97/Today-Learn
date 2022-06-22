# React 라이프 싸이클

React에서 라이프 싸이클이란 컴포넌트의 생명주기 입니다.

컴포넌트가 생겨나고, 변화하고, 없어지는 일련의 프로세스를 라이프 싸이클이라고 합니다.

`React 16.8버전`  (2019년도)에 추가된 공식 라이브러리인 Hook이 생기기 전까진 Class 기반의 라이프 싸이클만 있었습니다. 하지만 Hook이 생긴 뒤로는 클래스 기반과 Hooks를 사용한  2가지 방식의 라이프 싸이클이 있습니다.



## Class 기반의 라이프 싸이클

전체적인 라이크 싸이클 순서는 아래와 같습니다.

`constructor` => `render`  =>  `componentDidMount` => `componentDidUpdate` => `componentWillUnmount`

<img src="https://velog.velcdn.com/images%2Fjeanbaek%2Fpost%2F60e94ed2-4a82-4a04-9d9d-03d841e44bcb%2Fogimage.png">

>  React에서는 웹 어플리케이션이 실행되고(`Mount`) 종료되기(`UnMount`) 까지의 과정을 세세하게 나누어서 컨트롤 할 수 있습니다. 

### 컴포넌트가 로딩되기 시작하는 Mounting 단계

| 라이프싸이클 메소드 | 특징                                                         | 유의할 점                                                    |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `constructor`       | 클래스 생성자로 로직에서 가장 먼저 생성됩니다. <br> 현재 리엑트에선 생성자를 직접적으로 건드리는 일이 거의 없어졌기 때문에 중요도가 많이 떨어졌습니다. | 이 시점에서는 브라우저에 개발자가 작성한 `JSX` 가 보이지 않습니다. |
| `render`            | 실제 로딩이 이루어지는 시점입니다. constructor 메소드가 실행된 이후 실행됩니다. <br>이 메소드가 실행되면서 JSX가 HTML로 변환되어 우리가 보는 웹 브라우저에 나타나게 됩니다. | 컴포넌트가 로딩될 때에도 실행되지만 컴포넌트의 데이터(state, props)가 업데이트 되었을 때도 동작합니다. <br> 따라서 render 메소드 내에서 `setState` 혹은 `props` 를 변화시키는 메소드를 수행하지 않는 것이 좋습니다. |
| `componentDidMount` | 처음 로딩이 끝난 뒤에 실행되고 Mounting의 마지막 부분입니다. <br>render 메소드에 있는 모든 부분들이 브라우저에 나타나게 되었을 때만 실행됩니다. | 컴포넌트의 데이터가 업데이트 되어도 이 메소드는 다시 실행되지 않습니다. <br>오직 초기 컴포넌트의 로딩 이후에 한번만 실행되는 라이프싸이클 메소드 입니다. |

### 컴포넌트가 업데이트 되는 Updating 단계 (componentDidUpdate)

React에서 컴포넌트의 업데이트 감지는 오적 현재 컴포넌트에서 `state` 혹은 `props` 의 변경 유무로만 판단합니다.

#### state, props의 변경

위에 있는 이미지를 보면 `forceUpdate()` 라는 메소드가 있습니다. 이 메소드는 강제로 재 렌더링을 하도록 도와줍니다.

#### 재 렌더링(re-render)

state의 변경이 일어났기 때문에 React는 효율적으로 state의 변경사항을 감지하여 변경된 부분을 다시 렌더링합니다.

#### componentDidUpdate 실행

컴포넌트의 변경이 완료 되었을 경우 실행되는 메소드입니다.

이 메소드는 render 메소드가 실행되어 업데이트 된 state, props와 업데이트 되기 전인 state, props를 비교 작업을 가능하게 합니다.

### 컴포넌트의 삭제 단계 (componentDidUnmount)

컴포넌트가 사라질 때 실행되는 메소드입니다.

이 메소드에서 컴포넌트 내에 할당했던 여러 변수들을 해제시켜줄 수 있습니다. (Ex - eventListener, setInterval, setTimeout 등등)



## Hook의 개념

Hook을 사용하여 함수형 컴포넌트에서도 클래스형 컴포넌트에서만 가능하던 상태관리를 더 쉽게 할 수 있게 됐습니다.



### Hook이 생기기 이전 함수형 컴포넌트의 단점

함수형 컴포넌트들은 기본적으로 렌더링이 될 때, 함수 안에 작성된 모든 코드가 다시 실행됩니다.

클래스형 컴포넌트들은 method의 개념이기 때문에 re-render가 되어도 `render()` 를 제외한 나머지 method 및 state는 그대로 보존되어 다시 실행되지 않습니다.

이는 함수형 컴포넌트들이 **기존에 가지고 있던 상태(state)를 전혀 기억 할수 없게 만듭니다.**

따라서 항수형 컴포넌트를 `Stateless Component` 라고 합니다.

**=> 함수형 컴포넌트가 리렌더링될 때 무조건 새롭게 선언, 초기화, 메모리에 할당이 됩니다.**



### Hook의 등장으로 바뀐 점

브라우저에 메모리를 할당 함으로써, **함수형 컴포넌트가 상대(State)를 가질 수 있게 되었습니다,**

다시 말해, 함수 내에 써져 있는 코드 및 변수를 기억할 수 있게 되었습니다.



### React 공식 홈페이지에서 Hook을 만든 이유

1. 컴포넌트 사이에서 상태(State) 로직 재사용의 어려움
   - Render props, HOC 등
2. 복잡한 클래스형 컴포넌트들은 이해하기 어려움
   - 각종 라이프싸이클 메소드들
3. 클래스 자체 개념을 이해하기 어려움
   - this, bind 등



## Hook의 라이프 싸이클

React Hook은 함수형 Component에서 클래스형 Component의 기능을 구현한 개념입니다.

클래스에서만 사용 가능했던 state를 hook을 이용해 함수형 Component에서도 `useState` 를 이용해서 상태변수를 선언할 수 있습니다. 

또한 `useEffect` 를 통해 클래스형 Component의 라이프 싸이클 메소드인 `componentDidMount` , `componentDidUpdate` , `componentWillUnmount` 와 같은 목적으로 라이프 싸이클을 관리할 수 있습니다.

<img src="https://velog.velcdn.com/images%2Fdenmark-choco%2Fpost%2F45c244b4-0e73-4662-b4ac-d10bebab15eb%2Fcc006f00-a420-11e9-99a6-d0bdf5f0c7bb.png">



## useEffect

`useEffect`는 컴포넌트 안에서 불러내어 state 변수나 props에 접근이 가능하고, 첫번째 렌더링과 이후의 모든 업데이트에서 수행됩니다. 두번째 인자로 조건을 걸어 첫번째 렌더링이나 특정한 조건일 때만 수행되도록 하는 것도 가능합니다.

``` javascript
import React, { useState, useEffect } from 'react';

 export const App = () => {
   // 비구조화 할당
   cosnt [count, setCount] = useState(0);
   
   // componentDidMount, componentDidUpdate와 같은 방식
   useEffect(() => {
     console.log('component did mount with useEffect');
   });
   
   return (
     <div>
       <h2>You Clicked {count} times</h2>
       <button onClick={() => {
           setCount(count + 1)
         }}> Click Button </button>
     </div>
   );
 };

export default App;
```

> 위 코드는 Click Button 버튼을 클릭하면 h2 태그의 state 변수가 1씩 증가하는 예시입니다.
>
> 이런 식으로 코딩을 하면 react component가 Mount된 이후에 console.log가 보여지기 때문에 `componentDidMount` 의 역할을 한다고 볼 수 있습니다.



``` javascript
useEffect(() => {
  console.log('component did mount with useEffect');
}, []);
```

> 위의 코드와 같이 `[]` 를 추가하면 state가 변화하더라도 component를 재 랜더링하지 않겠다는 의미입니다.
>
> 따라서 버튼을 계속 해서 클릭해도 state는 변하지만 console.log는 찍히지 않습니다. 이 말인 즉슨 render가 되지 않았기 때문에 mount되지 않는다는 의미입니다.



``` javascript
useEffect(() => {
  console.log('component did mount with useEffect');
}, [count]);
```

> 위의 코드와 같이 `[count]`를 추가하면 count라는 state 변수가 변경될 시 component를 재 랜더링하겠다는 의미입니다.



마지막으로 `componentWillUnmount` 를 살펴보겠습니다.

``` javascript
import React, { useState, useEffect } from 'react';

export const App = () => {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    console.log('component did mount with useEffect');
    
    return () => {
      console.log('component is finished');
    }
  }, [count]);
  
  return (
    <div>
      <h2>You Clicked {count} times</h2>
      <button onClick={() => {
          setCount(count + 1);
        }}> Click Button </button>
    </div>
  )
};

export default App;
```

> componentWillUnmount의 역할을 할 수 있는 코드로서, component가 UnMount될 때 정리하거나 unscribe해아 할 것이 있다면  useEffect의 return 값으로 전달합니다.