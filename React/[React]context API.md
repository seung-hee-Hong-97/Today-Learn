# [React] Context API

컴포넌트들이 많아지면 여러 레벨을 거쳐 최상위 컴포넌트에서 `props`로 원하는 상태와 함수를 전달 하였지만, 이렇게 되면 `props` 가 필요하지 않은 컴포넌트들도 하위 컴포넌트에 값을 전달하기 위해 어쩔 수 없이 `props` 를 받아야 하는 경우가 생깁니다.

따라서 `Context API`를 통하여 한번에 원하는 값을 전달하는 방법을 다루겠습니다.

`Context API` 뿐만 아니라, `Redux` 및 `Mobx`와 같은 상태 관리 라이브러리를 사용할 수도 있지만 복잡하기 때문에 학습 난이도가 높습니다.



## Context API 사용법

### `1. createContext() 함수를 사용하여 Context 생성`

- `src` 디렉토리 하위에 `contexts` 디렉토리를 생성하고 그 안에 Store 파일을 만들거나, 최상위 컴포넌트에 바로 입력해도 상관 없음

- Context의 이름은 본인 마음대로 정하고, 하위에서 import로 받아오기 위해 export를 시킵니다.
- `{props.children}` 컴포넌트 태그 사이에 내용을 보여주는 props가 children 입니다.

``` javascript
/* Store.js */

import React, { createContext, useState } from 'react';

export const NameContext = createContext({
  name: '', // 기본 값 설정
  handleChange: () => { } // 함수도 전달 가능
});

const Store = (props) => {
  const [name, setName] = useState('');
  let [i, setI] = useState(0);
  
  const handleChange = () => {
    // Code...
  };
  
  return (
    <NameContext.Provider value={{ name, handleChange }}>
      { props.children }
    </NameContext.Provider>
  );
};

export default Store;
```



### `2. 최상위 컴포넌트에서 Provider로 래핑`

- `Context API`의 데이터에 접근해야 하는 하위 컴포넌트를 return 내부에서 Provider로 래핑
- 전달할 데이터는 `value = { }` 안에 겍체 형태로 넣음

``` javascript
/* App.js */

import React from 'react';
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Store from './Store/Store';

const App = () => {
  return (
    <Store>
      <BrowserRouter>
        <NameContext.Provider value={{ name: 'kim', handleChange }}>
          <Route path='/start' element={<Start />} />
          <Route path='/Main' element={<Main />} />
          <Route path='/Result' element={<Result />} />
        </NameContext.Provider>
      </BrowserRouter>
    </Store>
  );
};

export default App;
```



### `3. 하위 컴포넌트에서 context API 설정`

- 설정했던 context를 import
- `useContext`를 사용하여 value 값을 가져옴

``` javascript
/* Start.js */

import React, { useContext } from 'react';
import { NameContext } from './App.js';

const start = () => {
  const { name, handleChange } = useContext(NameContext);
  
  return (
    <div>
      <span onClick={handleChange}>{name}님 안녕하세요. </span>
    </div>
  );
};

export default Start;
```

