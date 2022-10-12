# [React] Redux

Redux(리덕스)란 Javascript 상태관리 라아브러리 입니다.



## Redux의 세가지 원칙

### `1. Single source of truth`

- 동일한 데이터는 항상 같은 곳에서 가지고 옵니다.
- 스토어라는 하나뿐인 데이터 공간이 있다는 의미

### `2. State is read-only`

- React에서는 setState 메소드를 활용해야만 상태 변경이 가능합니다.
- Redux에서도 Action이라는 객체를 통해서만 상태 변경을 할 수 있습니다.

### `3. Change are made with pure functions`

- 변경은 순수함수로만 가능
- Reducer와 연관되는 개념
- Stop(스토어) - Action(액션) - Reducer(리듀서)



## Store, Action, Reducer의 의미와 특징

### `Store (스토어)`

상태가 관리되는 오직 하나의 공간입니다.

- 컴포넌트와는 별개로 스토어라는 공간이 있어서 그 스토어 안에서 애플리케이션에 필요한 상태를 담습니다.
- 컴포넌트에서 상태 정보가 필요할 때 스토어에 접근



### `Action (액션)`

- Action은 애플리케이션에서 스토어에 운반할 데이터를 뜻합니다.
- Javascript 객체 형식으로 되어 있습니다.

``` javascript
{
  type: 'ACTION_CHANGE_USER',
  payload: {
    name: 'kim',
    age: 40
  }
}
```



### `Reducer (리듀서)`

- Action을 바로 Store에 전달하는 것이 아니라 Reducer에 먼저 전달
- Reducer가 Action을 보고 Store의 상태를 업데이트
- Action을 Reducer에 전달하기 위해선 `dispatch()` 메소드를 사용

> 1. Action 객체가 dispatch( ) 메소드에 전달됨
> 2. dispatch( ) 메소드를 통해 Reducer를 호출
> 3. Reducer는 새로운 Store를 생성



## React에서 Redux 사용한 상태 관리 예제

버튼 클릭 시 1씩 증가 혹은 감소하는 프로그램을 위한 모듈과 컨테이너를 생성하는 예제입니다.



### `1. React Redux 모듈 설치`

필요한 모듈은 `redux` 와 `react-redux` 두가지입니다.

아래의 명령어를 입력하여 모듈을 설치합니다.

``` bash
$ npm install redux react-redux
```

> react-redux는 React 환경에 맞는 Redux를 사용할 수 있게 해줌



### `2. Provider로 감싸기`

``` jsx
/* src/index.js */

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import rootReducer from './module/index';

const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store = {store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```



### `3. 사용될 모듈 생성`

``` javascript
/* src/module/counter.js */

const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

const initialState = 0;

export default function counter(state = initialState, action) {
  switch (action.type) {
    case INCREASE:
      return state + 1;
      
    case DECREASE:
      return state - 1;
      
    default:
      return state;
  }
}
```

> `+` 버튼 클릭 시 1씩 증가, `-` 버튼 클릭 시 1씩 감소



```javascript
/* src/module/counter2.js */

const INCREASE = "counter2/INCREASE";

const initialState = { value = 0 };

export const increase = (num) => ({
  type: INCREASE,
  number: num,
});

export default function counter2(state = initialState, action) {
  switch (action.type) {
    case INCREASE:
      return { ...state, value: state.value + parseInt(action.number) };
    
    default:
      return state;
  }
}
```

> 버튼 클릭 시, `input` 에 적힌 숫자만큼 증가



### `4. combinereducer로 Reducer 함수들 통합`

``` javascript
/* src/module/rootReducer.js */

import { combineReducers } from 'redux';
import counter from "./counter";
import counter2 from "./counter2";

const rootReducer = combineReducers({ counter, counter2 });

export default rootReducer;
```



### `5. 컨테이너 생성`

``` jsx
/* src/Containers/CouterContainer.js */

import React from 'react';
import Counter from "../components/Counter";
import { useSelector, useDispatch } from 'react-redux';
import { increase, decrease } from "../module/Counter";

const CounterContainer = () => {
  const number = useSelector((state) => state.counter);
  const dispatch = useDispatch();
  
  const onIncrease = () => {
    dispatch(increase());
  };
  
  const onDecrease = () => {
    dispatch(decrease());
  };
  
  return (
    <Counter
        number={number}
        onIncrease={onIncrease}
        onDecrease={onDecrease}
    ></Counter>
  );
};

export default CounterContainer;
```



```jsx
/* src/Containers/CounterContainer2.js */

import React from 'react';
import Counter2 from "../components/Counter2";
import { useSelector, useDispatch } from 'react-redux';
import { increase } from '../module/counter2';

const CounterContainer2 = () => {
  const number2 = useSelector((state) => state.number2);
  const dispatch = useDispatch();
  
  cnost onIncreaes2 = (num) => {
    dispatch(increase(num));
  };
  
  return <Counter2 number2={number2.value} onIncrease2={onIncrease2} />;
};

export default CounterContainer2;
```



### `6. 화면에 보여줄 컴포넌트 생성`

``` jsx
/* src/components/Counter.js */

import React from 'react';

const Counter = ({ number, onIncrease, onDecrease }) => {
  return (
    <div>
      <h1> {number} </h1>
      <button onClick={onIncrease}> +1 </button>
      <button onClick={onDecrease}> -1 </button>
    </div>
  );
};

export default Counter;
```



```jsx
/* src/components/Counter2.js */

import React, {useState} from 'react';

const Counter2 = ({ number2, onIncrease2 }) => {
  const [num, setNum] = useState(0);
  const onChange = (event) => {
    setNum(event.target.value);
  };
  
  return (
    <div>
      <hr />
      <h1>CounterContainer2</h1>
      <div>
        <button onClick={() => onIncrease2(num)}> + </button>
        <input type="text" onChange={onChange} value={num} />
        <p>DisplayNum</p>
        {number2}
      </div>
    </div>
  );
};

export default Counter2;
```



### `7. App에 컨테이너 추가`

``` jsx
import React from 'react';
import CounterContainer from './Container/CounterContainer';
import CounterContainer2 from './Container/CounterContainer2';

const App = () => {
  return (
    <div>
      <CounterContainer />
      <CounterContainer2 />
    </div>
  );
};

export default App;
```

